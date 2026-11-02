# CHANGELOG · asc-watcher

## 2026-11-02

- **上线官方产品宣传片影院模块 (`#video`) 与中英双语 78 秒 Launch-Film (`index.html`, `index.en.html`, `en/index.html`, `style.css`, `promo.mp4`, `promo-en.mp4`, `cover.png`, `cover-en.png`)**：
  - 在 `index.html`、`index.en.html` 与 `en/index.html` 顶部导航栏新增「宣传片 / Film」锚点，首屏 CTA 区域新增 `#heroWatchBtn`（「🎬 观看官方宣传片 / Watch Launch Film」平滑滚动并自动播放）；
  - 新增 `#video` 官方产品宣传片影院模块（`.promo-video-sec`），分别挂载中文版 `promo.mp4`（配 `cover.png`）与英文版 `promo-en.mp4`（配 `cover-en.png`），BGM 采用契合开发者数据看板题材的 `bgm-tech-house.mp3`（Tech House ~124 BPM）；
  - 集成 8 个时间戳章节快跳胶囊（`00:00` 销售与趋势校准 → `00:10` iPhone Duo 双屏工作台 → `00:20` 一键过审发布与里程碑 → `00:30` 全球评论秒级回复 → `00:40` 全球地域与90天曲线 → `00:50` AI 增长军师与 TG 闭环 → `01:00` 本机密钥与桌面小组件 → `01:10` 品牌终章），支持播放进度实时高亮联动与点击快跳。

## 2026-10-30

- **接入官方 App Store 下载跳转链接 (`https://apps.apple.com/app/id6828810863`)**：
  - 在 `site.config.json` 中配置 `links.appStore: "https://apps.apple.com/app/id6828810863"`，自动注入全站 JSON-LD `SoftwareApplication.installUrl`、`llms.txt`、`llms-full.txt` 与英文档案；
  - 在 `index.html`、`index.en.html`、`en/index.html` 的顶部导航栏 CTA、首屏主下载按钮（`.appstore-btn`）、三档价格方案卡片按钮（`#pricing`）及页脚链接中接入 App Store 官方跳转链接（同时保留「先试玩交互演练舱」与「查看 .p8 密钥配置指南」快捷入口），并在 `support.html` / `support.en.html` 补充商店直达入口。
- **修复浏览器旧版 `style.css` 缓存导致样式错位并新增 iPhone Duo 双屏截图展示 (`style.css`, `index.html`, `index.en.html`, `en/*.html`, `tools/build_devlog.py`)**：
  - **原因**：首次部署后升级企业级 `style.css` 时，`<link rel="stylesheet" href="style.css">` 未带版本哈希参数，导致浏览器直接复用首次缓存的旧版暖米色样式表（缺失 `.appstore-btn` 等新类名，使得 Apple SVG 图标失去尺寸约束放大）；
  - **修复**：全站所有 HTML 页面及生成脚本统一加上缓存穿透版本号 `style.css?v=20261030v3`，为 `<svg>` 补齐原生 `width="22" height="22"` 属性兜底，在 `style.css` 中同时兼容 `.appstore-btn` / `.store-btn` / `.hide-sm` / `.updated` / `.pager`（全站 20 个 HTML 页面 CSS 类覆盖率 100%），并在 `#shots` 实机画廊中新增第 07 幕 **iPhone Duo 折叠双屏工作台** 海报。
- 使用 `/Users/blake/Agy/Code/app-ai-creator` 企业级「Playground-First」官网脚手架全面升级站点视觉与交互：
  - 升级 `style.css` 为科技蓝 (`#3b82f6`) + 赛博青 (`#06b6d4`) + 翡翠绿 (`#10b981`) 明暗双生设计系统；
  - 重构 `index.html` 与 `index.en.html`：新增 `#heroCanvas` 动态粒子星网、发光旋转 App 徽章、首屏双舞台实机预览、3 模式可交互演练舱（`#playground`：柱状图单日构成点选拆解、用户评论直接回复模拟、过审版本一键发布 + 实时端侧遥测与白盒执行链）、实机真容截图灯箱画廊（`#shots`）、7 维对比矩阵（`#compare`）、100 次免弹窗 + Pro 透明定价矩阵（`#pricing`）与 FAQ 手风琴（`#faq`）；
  - 更新 `site.config.json` 并通过 `make legal-huawei && make build && make geo-check` 零错误校验。
- 初始化 ASC Watcher 官方网站（`https://ai-chen2050.github.io/asc-watcher`）：
  - 编写 `site.config.json` 唯一真相源；
  - 完成中英双语首页（`index.html` / `index.en.html`）、功能机制与问答（`geo.html` / `geo.en.html`）、技术支持（`support.html` / `support.en.html`）、隐私政策（`privacy.html` / `privacy.en.html` 及 `.huawei` 渠道版）、用户协议（`terms.html` / `terms.en.html` 及 `.huawei` 渠道版）与首篇开发日记；
  - 生成 `llms.txt`、`llms-full.txt`、`sitemap.xml`、`robots.txt` 并通过 `make geo-check` 一致性体检。
