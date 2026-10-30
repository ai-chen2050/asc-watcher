# 第三方技能来源与跨技术栈适配指南

本目录及 Flutter 模板下的设计工程技能来自开源社区杰出项目 [emilkowalski/skills](https://github.com/emilkowalski/skills)（由前 Vercel / Linear 设计工程师 Emil Kowalski 创作，MIT 许可证）。

脚手架已将该套件全量整合进模板体系，使以后由 `aic` 创建的每一个 Flutter App、Godot 游戏以及 Web 官网在开发过程中，都可以由 AI 随时调用这些高水准的交互标准与审计规范。

---

## 技能全景清单

### 1. 跨项目通用技能（位于 `templates/shared/.claude/skills/`，所有工程共享）

| 技能 | 包含文件 | 核心职责与触发场景 |
|---|---|---|
| `emil-design-eng/` | `SKILL.md` | **设计工程主法则**：组件质感打磨、微交互、视觉层级、Before/After 对照审查表 |
| `apple-design/` | `SKILL.md` | **Apple 流动交互原则**：源自 WWDC，涵盖可打断性、弹簧物理、速度接力、橡皮筋与空间排版 |
| `animate/` | `SKILL.md`<br>`RECIPES.md` | **从零构建动效**：严格频次门禁（高频严禁动效）、缓动与时长定则、打断与退出逻辑 |
| `animate-expo/` | `SKILL.md`<br>`RECIPES.md` | **跨平台移动动效**：手势驱动、底层物理弹簧、触感反馈、BottomSheet 与页面转场 |
| `animation-performance/` | `SKILL.md` | **帧率预算与掉帧排查**：60/120fps 性能守护、主线程避坑、合成层属性与渲染重绘诊断 |
| `review-animations/` | `SKILL.md`<br>`STANDARDS.md` | **动效代码严格审查**：按工匠标准挑刺（缓动、时长、打断性、无障碍 reduced-motion） |
| `improve-animations/` | `SKILL.md`<br>`AUDIT.md`<br>`PLAN-TEMPLATE.md` | **全库动效体检与分级规划**：针对已有工程全面审计，输出高性价比改造计划 |
| `find-animation-opportunities/` | `SKILL.md` | **挖掘动效机会**：识别最具质感杠杆的核心微交互，**明确严禁滥用动效的区域** |
| `animation-vocabulary/` | `SKILL.md` | **动效精确专业词汇**：将“弹一下/滑出来”转化为衰减、过冲、刚度、阻尼等精确提示词 |
| `break-ui/` | `SKILL.md`<br>`CATALOG.md` | **真实边界破坏性测试**：用真实最坏数据（超长各国人名、无断字邮箱、空列表、极端数字）压测 UI |
| `mobile-native/` | `SKILL.md` | **移动端原生感修补**：消除网页感，修正 100vh、点击高亮闪烁、300ms 延迟、安全区避让 |
| `prototype/` | `SKILL.md`<br>`PICKER.md` | **快速原型实验场**：同一界面出 N 个方向不同的自包含原型，置于切换器内直观比选 |
| `pick-ui-library/` | `SKILL.md` | **UI 依赖选型评估**：避免引入废弃、高隐患或自造轮子的劣质外部包 |
| `ask-sonner/` | `SKILL.md`<br>`API.md` | **轻量通知与反馈规范**：优雅非阻塞 Toast 的堆叠、展开与消除微交互 |

### 2. 原生与多平台补充（位于 `templates/flutter-app/files/.claude/skills/`）

| 技能 | 包含文件 | 核心职责与触发场景 |
|---|---|---|
| `write-swift/` | `SKILL.md` | **现代 Swift 6 规范**：值类型建模、结构化并发、WidgetKit 小组件、App Intents、MethodChannel 原生层编写 |

### 3. 脚手架原创 AI 技能（协同工作流）

| 技能 / 命令 | 核心职责与触发场景 |
|---|---|
| `finesse-ui` | 从零构思页面时的沟通确认 SOP（先说清楚“感觉与禁忌”，AI 描述效果确认后再动手） |
| `taste-skill` | 老页面重构改版 SOP（“保留什么、换成什么感觉”，调整版面密度与动效） |
| `/impeccable` (`impeccable`) | 交付前五维量化审计打分（`/impeccable audit`）与高频死角逐项抛光（`/impeccable polish`） |
| `design-handoff` | HTML 高保真原型精准转译为 Flutter Widget、Godot 场景树或微信小程序 Skyline 组件 |
| `wechat-platform` | 微信小程序（Skyline + TS + 令牌转 WXSS）与微信小游戏（Godot 4 WASM 分包 + 原生 SDK 桥接 + 开放数据域排行 + `miniprogram-ci`）全栈规范 |

### 4. 微信生态参考开源项目与致谢

本脚手架的微信小程序与微信小游戏适配工具链参考并吸收了以下优秀开源项目的工程思路：

| 开源项目 | 领域 | 吸收与致谢的核心机制 |
|---|---|---|
| [`mkdevkit/godot-minigame`](https://github.com/mkdevkit/godot-minigame) & [`godothub/godot-minigame`](https://github.com/godothub/godot-minigame) | Godot 4 → 微信小游戏 | WASM Brotli 压缩拆入 `engine` 分包、`IndexedDB` 重定向至 `wx.env.USER_DATA_PATH`、WebAudio 切后台恢复 |
| [`AnranS/godot_for_minigame`](https://github.com/AnranS/godot_for_minigame) | 小游戏 Runtime 适配 | `weapp-adapter.js` 浏览器 DOM/BOM/WebGL2 垫片与开放数据域 `sharedCanvas` 桥接 |
| [`wechat-miniprogram/skylint`](https://github.com/wechat-miniprogram/skylint) & [`api-typings`](https://github.com/wechat-miniprogram/api-typings) | 微信小程序 Skyline/TS | Skyline (`glass-easel`) 渲染引擎规范、TypeScript 类型定义与 WebView 兼容性约束 |
| [`NewFuture/miniprogram-template`](https://github.com/NewFuture/miniprogram-template) | 小程序 TS 工程化 | 原生 TypeScript 模块化、多环境构建与严格代码规范 |
| [`ruochuan12/mini-ci`](https://github.com/ruochuan12/mini-ci) & [`seepine/action-miniprogram-ci`](https://github.com/seepine/action-miniprogram-ci) | 微信自动化 CI/CD | 基于 `miniprogram-ci` 的命令行一键上传体验版与终端预览二维码生成 |

---

## 跨技术栈适配对照表（AI 阅读技能时请遵循此映射）

技能库中的示例多数以 Web (CSS/JS) 或 React 为表达载体。在 Flutter 应用、Godot 游戏或 iOS 原生开发中，其物理与设计逻辑完全通用，请按如下对照进行语义映射：

| 设计/动效原则 (Web 原文) | Flutter (Dart) 落地 | Godot (GDScript) 落地 | iOS 原生 (Swift / SwiftUI) |
|---|---|---|---|
| `transition: transform 200ms ease-out` | `AnimatedContainer` / `TweenAnimationBuilder` + `Curves.easeOutCubic` | `create_tween().set_ease(Tween.EASE_OUT).set_trans(Tween.TRANS_CUBIC)` | `.animation(.easeOut(duration: 0.2), value: ...)` |
| 只动 `transform` 与 `opacity`（避开重排） | 仅在 `Transform` / `Opacity` 上做补间，禁止动 `width`/`padding` 引发 `relayout` | 仅动 `position`/`scale`/`modulate`，禁止动画修改 `custom_minimum_size` 触发布局重排 | 仅动 `.scaleEffect` / `.opacity` / `.offset`，避免触发容器重算 |
| 按钮 `:active` 缩放反馈 (`scale(0.97)`) | `GestureDetector` / `Listener` 驱动 `AnimatedScale(scale: isPressed ? 0.96 : 1.0)` | `button_down` / `button_up` 驱动 Tween 缩放 `Vector2(0.96, 0.96)` | `ButtonStyle` 中使用 `configuration.isPressed ? 0.96 : 1.0` |
| 永远不要从 `scale(0)` 开始弹出 | 弹窗入场使用 `Tween(begin: 0.92, end: 1.0)` 配合 `FadeTransition` | 弹出节点初始缩放设为 `Vector2(0.92, 0.92)`，淡入同时微放大 | `.scaleEffect(isPresented ? 1.0 : 0.92)` 配合 `.opacity` |
| 下拉/浮层进入严禁 `ease-in`（迟钝感） | 必须使用 `Curves.easeOut` 或 `Curves.fastOutSlowIn` | 必须使用 `Tween.EASE_OUT` | 必须使用 `.easeOut` 或 `Spring.smooth` |
| 弹簧物理 (stiffness, damping, mass) | `SpringSimulation` / `flutter_animate` 的 spring 物理仿真 | `Tween.TRANS_SPRING` / `TRANS_ELASTIC` 或按帧阻尼积分 | `withAnimation(.spring(response:dampingRatio:))` |
| 动画可打断性 (Interruptibility) | 新动效必须基于当前进度或 `controller.value` 接管，切忌强制跳回 0 重播 | 调用 `tween.kill()` 后直接从当前节点实时属性起新 Tween | SwiftUI 原生隐式动画自动继承速度与当前位置 |
| 减弱动态效果 (`prefers-reduced-motion`) | 严格遵守 `MediaQuery.of(context).disableAnimations`，直接跳转终态 | 检查游戏内设置与系统无障碍设置，跳过耗时 Tween | `@Environment(\.accessibilityReduceMotion)` 判断 |
| 避免主线程/重绘丢帧 (`will-change`) | 为动画复杂子树包裹 `RepaintBoundary`，隔离开销 | 启用 CanvasItem 的单独图层或合并批处理 | 开启 `.drawingGroup()` 或离屏缓存 |
| `break-ui` 最坏真实数据注入 | 在 `test/*_test.dart` 中用超长各国文本、特长商品名、0 金额、缺失字段进行 Widget 压测 | 在测试用例中注入超长字符串检查 Label 的 `autowrap_mode` 与截断 | 针对 Preview 注入 `traits: .fixedSize` 极端数据 |
| `mobile-native` 安全区与触控穿透 | 使用 `SafeArea`、设置 `HitTestBehavior.opaque`，保证 44×44pt 最小点击热区 | 使用 `Control` 节点配置锚点，设置 `mouse_filter = MOUSE_FILTER_PASS` | 使用 `.ignoresSafeArea(edges:)` 与 `.contentShape(Rectangle())` |

---

## 许可证说明

`emilkowalski/skills` 采用 MIT 许可证：

```text
MIT License

Copyright (c) 2026 Emil Kowalski

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
