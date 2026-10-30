---
name: wechat-platform
description: 微信生态（微信小程序 Skyline/TS 与微信小游戏 Godot 4/Canvas/WebGL）架构、跨端适配、开放数据域、米大师/激励广告变现、隐私合规拦截与 miniprogram-ci 自动化交付全流程规范。当开发微信小程序、微信小游戏、把 Godot 4 导出到微信小游戏、或将 Flutter 应用与微信小程序做跨端协同/设计令牌同步时激活本技能。
---

# 微信平台（微信小程序 & 微信小游戏）工程与合规全书

本技能沉淀自开源社区成熟方案（`mkdevkit/godot-minigame`、`godothub/godot-minigame`、`wechat-miniprogram/skylint`、`NewFuture/miniprogram-template`、`ruochuan12/mini-ci`）与真实微信提审踩坑教训。

---

## 一、微信小程序：原生 TypeScript + Skyline (`glass-easel`) 架构规范

### 1. 为什么首选 Skyline + Glass-easel
- **传统 WebView 的瓶颈**：每个页面一个 WebView 实例，内存高、缺少原生手势协商与共享元素转场、长列表容易白屏。
- **Skyline 渲染引擎**：微信官方新一代接近原生渲染管线的引擎，配合 `glass-easel` 组件框架与 `worklet` 动画，支持 60/120fps 丝滑手势返回、半屏弹窗与跨页共享元素。
- **标配 `app.json` 开启方式（含 WebView 兜底意识）**：
  ```json
  {
    "renderer": "skyline",
    "rendererOptions": {
      "skyline": {
        "defaultDisplayBlock": true,
        "defaultContentBox": true,
        "disableABTest": true,
        "sdkVersionBegin": "3.0.0",
        "sdkVersionEnd": "15.99.99"
      }
    },
    "componentFramework": "glass-easel",
    "lazyCodeLoading": "requiredComponents",
    "darkmode": true,
    "themeLocation": "theme.json"
  }
  ```
- **Skyline 排版三大铁律（不看必踩）**：
  1. `defaultDisplayBlock: true` 下，`view` 默认是 `display: flex; flex-direction: column` 或块级布局，**不支持 `float`、`display: inline`、`table` 布局** —— 一律用 Flex 或 Grid；
  2. **滚动必须显式使用 `<scroll-view type="list" scroll-y>`**，Skyline 页面默认整页不滚动（不像 WebView 那样溢出就自动滚）；
  3. **自定义导航栏与胶囊按钮安全区**：右上角固定有微信官方胶囊按钮（`wx.getMenuButtonBoundingClientRect()`），顶部操作栏必须读取 `top` 与 `height` 对齐，且右侧预留 `windowWidth - rect.left + 12px` 的避让间距，**严禁把搜索框或个人中心图标放在右上角被胶囊遮挡**！

### 2. 设计令牌同源流水线（`design-spec/tokens.json` → `tokens.wxss` & `tokens.ts`）
- **禁止在页面 `.wxss` 里手写 `#RRGGBB` 或 `rgba(...)` 字面量**！
- 运行 `python3 tool/tokens_to_wxss.py`（或 `make tokens`），从与 Flutter 主应用同源的 `design-spec/tokens.json` 生成：
  - `miniprogram/styles/tokens.wxss`（同时产出 `:root, page` 亮色变量与 `@media (prefers-color-scheme: dark)` 暗色变量）
  - `miniprogram/theme.json`（供 `app.json` 导航栏、窗口背景色自动随系统切换明暗）
  - `miniprogram/core/tokens.ts`（供 Canvas / Skyline Worklet 动画按类型读取）

### 3. 微信隐私协议强制拦截门禁（基础库 2.32.3+ 硬性要求）
- **事故现场**：未在后台更新《用户隐私保护指引》或未在前端监听 `wx.onNeedPrivacyAuthorization`，调用剪贴板（`wx.setClipboardData`）、相册（`wx.saveImageToPhotosAlbum`）、头像昵称、位置或麦克风时，**API 会直接静默返回 `fail privacy permission is not authorized`**，甚至提审直接被拒。
- **标准做法**：
  - 全局挂载 `<privacy-popup>` 组件；
  - 在 `wx.onNeedPrivacyAuthorization(resolve => ...)` 被触发时弹出自定义隐私弹窗；
  - 弹窗内的「同意」按钮**必须使用微信原生 `<button id="agree-btn" open-type="agreePrivacyAuthorization" bindagreeprivacyauthorization="onAgree">`**，普通的 `bindtap` 无法向微信底层解锁隐私接口权限！

---

## 二、微信小游戏：Godot 4 导出适配 & 原生小游戏核心攻坚

### 1. Godot 4 Web 导出 → 微信小游戏四大底层鸿沟（参考 `mkdevkit/godot-minigame` & `godothub/godot-minigame`）

| 鸿沟 | 为什么标准 Godot Web 导出跑不起来 | 脚手架 `tools/build_wxgame.py` 的解决方式 |
|---|---|---|
| **① 4MB 主包体积红线** | 微信小游戏**主包严格限制 ≤ 4MB**（全部分包总和 ≤ 30MB），而 Godot 导出的 `.wasm` 原始体积常达 20MB+，`.pck` 也很大 | 将 `.wasm` 执行 Brotli 最高级压缩（`.wasm.br` 压至 ~4.5MB）并拆入 `engine/` 独立分包（`WXWebAssembly.instantiate('engine/index.wasm.br')` 原生支持 `.wasm.br`）；大体积 `.pck` 重命名为 `.bin` 放入分包或 CDN 异步拉取 |
| **② 无 IndexedDB 导致存档丢失** | Godot Web 默认将 `user://` 挂载在浏览器 `IndexedDB`（`/userfs`）上，微信小游戏环境无 `IndexedDB`，关掉小游戏存档全丢 | 通过 `godot-wx-polyfill.js` 拦截 `user://` 持久化同步，映射到微信原生 `wx.getFileSystemManager()` + `${wx.env.USER_DATA_PATH}`（本地 200MB 持久化配额） |
| **③ `SharedArrayBuffer` 与单线程导出** | 微信小游戏 iOS/Android 容器不支持浏览器 COOP/COEP 跨源隔离头，若开启 Godot `threads/thread_support` 会直接崩 | 微信小游戏导出预设必须设 `variant/thread_support=false`（单线程 WebGL2 导出），并禁用 `wasm-eh` 异常处理冲突 |
| **④ 切后台 / 接电话后全游戏静音** | 微信切入后台、拉起分享卡片或接听语音后，`WebAudioContext` 被微信强制挂起且不自动恢复 | 在胶水层监听 `wx.onShow` 与 `wx.onAudioInterruptionEnd`，自动遍历已创建的 `AudioContext` 执行 `.resume()` |

### 2. 微信小游戏原生 SDK 五大能力桥接（`WeChatBridge.gd` / `wx_game_sdk.js`）

1. **静默登录 (`wx.login`)**：
   - 首屏严禁强制弹窗索要头像昵称（微信已废弃强授权 `wx.getUserInfo` 弹窗，审核必拒）；
   - 启动时静默调用 `wx.login()` 拿 `code` 即可标识玩家，需要展示头像昵称时使用 `wx.createUserInfoButton` 或头像昵称填写能力。
2. **主动转发分享 & 朋友圈 (`wx.shareAppMessage` / `wx.showShareMenu`)**：
   - 必须先调 `wx.showShareMenu({ withShareTicket: true, menus: ['shareAppMessage', 'shareTimeline'] })`，否则右上角菜单里的「转发给朋友」和「分享到朋友圈」是置灰不可点的；
   - **严禁诱导分享**：不能根据 `shareAppMessage` 的回调直接发放核心奖励（微信早在几年前就移除了分享成功回调 `success` 的真实性，點进聊天界面直接返回也会触发，且明文诱导分享会被封禁能力）。奖励机制一律走**激励视频广告**。
3. **激励视频广告 (`wx.createRewardedVideoAd`) —— 变现主力**：
   - 必须是**全局单例**（反复 `createRewardedVideoAd` 会导致旧监听器累积，看完一次广告发 N 次奖励！）；
   - **只在 `onClose(res => { if (res && res.isEnded) { ... } })` 中发放奖励**，中途退出 `res.isEnded === false` 严禁发奖；
   - 必须监听 `onError` 提供降级提示（如「今日广告已看完，可改用分享或稍后再试」），防止无广告填充时玩家卡死。
4. **开放数据域好友排行榜 (`wx.getOpenDataContext()` + `sharedCanvas`)**：
   - 微信为了保护关系链隐私，规定 `wx.getFriendCloudStorage`（获取微信好友分数排行）**只能在独立的 `openDataContext`（开放数据域）子目录中调用**；
   - 开放数据域不能调网络请求、不能写主域存储，只能把排行榜画在 `sharedCanvas` 上，主域通过 `wx.getOpenDataContext().postMessage({ type: 'UPDATE_SCORE', score })` 传消息，再把 `sharedCanvas` 作为纹理绘制到主屏幕。
5. **虚拟支付与 iOS 平台红线（米大师 `wx.requestMidasPayment`）**：
   - **铁律**：微信小游戏/小程序在 **iOS 端默认受苹果虚拟支付政策限制**（未开通 iOS 小游戏虚拟支付专属通道时，iOS 端严禁出现"购买钻石/月卡/¥6"等虚拟商品付费入口，否则审核秒拒）；
   - **代码级渠道门禁**：桥接层自动检测 `wx.getDeviceInfo().platform === 'ios'`：
     - Android 端：正常拉起 `wx.requestMidasPayment`；
     - iOS 端：自动隐藏直接付费按钮，切换为「观看激励视频获取」或引导至微信客服会话（`wx.openCustomerServiceConversation`）。

---

## 三、自动化交付（`miniprogram-ci`）与提审自检

- 不要每次发版都手动打开微信开发者工具点上传！
- 在微信公众平台「开发 → 开发管理 → 开发设置 → 小程序代码上传」生成 `private.<appid>.key`（放进项目根或 `~/.wechat/`，**已加入 `.gitignore`，严禁入库**），并配置开发机 IP 白名单。
- 运行：
  - `make preview` —— 命令行编译并生成终端预览二维码；
  - `make upload` —— 自动递增/读取版本号，开启 `es6: true, minify: true, autoPrefixWXSS: true`，一键推送体验版到微信后台。
