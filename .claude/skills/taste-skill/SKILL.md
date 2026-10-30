---
name: taste-skill
description: 给老页面或简陋页面做高质感审美改版。当用户提出“用 Taste Skill 改这个页面，保留 XX，换成 XX 感觉”或觉得现有 UI 太简单、不够优雅高级时触发。遵循“先复述保留项与审美目标，对版面、动效、信息密度三维重构”的工作流。
---

# Taste Skill — 既有页面审美改版与质感跃迁

> **万能公式**：`用 Taste Skill 改这个页面，保留 [XX功能/数据]，换成 [XX感觉]`

## 核心理念

老页面之所以显得“简陋、像十年前的后台表单、没有高级感”，通常不是因为功能少，而是犯了三个致命问题：
1. **信息平铺无主次**：所有字段同等字号、同等边距堆成一列，没有呼吸感与视觉焦点；
2. **状态切换全靠闪现**：弹窗、切页、勾选、展开没有任何空间连贯性；
3. **表面材质干瘪**：缺乏微光渐变、微晶图标舱、精细描边与明暗自适应对比。

**Taste Skill** 在严格守住既有业务逻辑与数据契约的前提下，从**版面层级、连贯动效、信息密度**三个维度彻底重塑页面质感。

---

## 标准三步工作流（Mandatory Workflow）

### Step 1：复述理解，校准边界（Restate & Align）
在改代码前，先用简明清单向用户复述：
1. **🔒 严格保留项（Keep Intact）**：原有的数据模型、状态回调、路由出口、测试 Key 以及用户点名保留的交互结构。
2. **✨ 质感跃迁方案（Taste Upgrade）**：
   - **版面重构（Layout）**：如何把平铺列表重组为“主视觉 Hero 舞台 + 双列/多维精巧宫格 + 吸顶/悬浮主操作”？
   - **动效注入（Motion）**：哪里加入阶梯入场（Stagger）、滚动阻尼视差（ScrollAnimatedPanel）、原地翻转/展开或完成粒子反馈（CompletionFeedbackButton）？
   - **信息密度（Density）**：哪些低频表单项可以折叠进胶囊或半屏抽屉（Progressive Disclosure），让首屏一眼清爽？

### Step 2：三维审美升级（3-Axis Transformation）

#### 维度一：版面与表面材质（Surface & Hierarchy）
- **告别单调Radio/列表，升级双列流光选择卡**：将干瘪的 `RadioListTile` 或竖排文字列表重构为「顶部当前组合实时预览横幅 + 双列流光选中卡片（带徽标与副文案）」。
- **顶部动态微交互橱窗（Hero Showcase）**：在付费页（Pro Paywall）、引导页（Onboarding）、总览页顶部引入可自动轮播 + 手势滑动的微交互橱窗（桂冠口碑、扇形悬浮卡、动态生长柱状图、贴纸星环）。
- **微晶圆环图标舱（Icon Pod）**：所有卡片标题与列表项左侧配备渐变/半透明底色的微晶图标舱，右侧状态使用胶囊徽标（Pill Badge）。
- **明暗双生高对比（Adaptive Light/Dark）**：严禁写死暗色或浅色背景，使用语义令牌（`t.bg`, `t.card`, `context.isDark`），浅色下通透温润、暗色下深邃有微光描边。

#### 维度二：自然连贯动效（Continuity & Causal Physics）
- **替换呆板按钮**：
  - 提交/打卡/保存按钮替换为 `CompletionFeedbackButton` 或 `TriStateSubmitButton`；
  - 右下角原生 `FloatingActionButton` 替换为随滚动智能收缩/展开的 `BrandFloatingButton`；
  - 加入/数量调节替换为 `MorphingStepperButton`。
- **替换生硬弹窗**：底部弹窗与对话框使用 `showAnimatedBrandSheet` / `showAnimatedBrandDialog`（弹簧缩放 + 滑入 + 毛玻璃背景过渡），注意消除双拖拽横条（`showDragHandle: false`）。
- **滚动视差包装（`ScrollAnimatedPanel`）**：长页面多卡片列表接入滚动阻尼视差，注意**静止与滚动状态严禁改变 Widget 树层级深度**，防止子组件内部展开/输入状态丢失。

#### 维度三：信息密度与渐进式折叠（Progressive Disclosure）
- **默认收起次要维度**：日期、分类、高阶参数默认收起在顶栏紧凑胶囊（如 `📅 今天 · 午餐 ▾`），多维明细折叠为单行摘要条，点击才就地平滑撑开（`InPlaceExpandableList` / `AnimatedSize`）。
- **消除重复入口**：同一业务流程在相邻页面只保留一处最自然的入口，去除冗余副标题与解释性废话。

### Step 3：自检收尾
改版完成后，建议用户运行 `/impeccable audit` 与 `/impeccable polish` 进行像素级收尾检查。
