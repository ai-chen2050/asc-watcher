---
name: finesse-ui
description: 从零打造高质感、有呼吸感与流体微交互的 UI 页面。当用户提出“用 finesse UI 做一个 XX 页面，要 XX 感觉，不要 XX”或需要从零设计新页面/新组件时触发。遵循“先复述视觉与动效蓝图让用户确认，再动手写代码，不满意局部精修”的渐进式高定工作流。
---

# Finesse UI — 从零打造高质感页面

> **万能公式**：`用 finesse UI 做一个 [XX页面/组件]，要 [XX感觉]，不要 [XX]`

## 核心理念

AI 直接生成的默认页面往往陷入“能用但廉价”的通病：大面积纯白/纯黑生硬背景、千篇一律的矩形边框、突兀的瞬时显隐、缺乏触感与物理惯性。
**Finesse UI** 的核心是在写第一行代码前，先把**视觉层次、表面材质、微交互因果律与反模式红线**对齐清楚。

---

## 标准四步工作流（Mandatory Workflow）

### Step 1：拆解三要素（Parse the Brief）
从用户的提示词中提取并补全三个核心维度：
1. **做什么页面（What）**：核心用户任务、主行动点（Primary CTA）、高频交互与次要信息。
2. **要什么感觉（Vibe & Craft）**：
   - 如：*“温暖治愈的日式杂货店感”*、*“Apple 健康级别的精密流体感”*、*“小红书杂志画报感”*、*“克制通透的毛玻璃拟物感”*。
   - 将抽象感觉映射为具体的设计令牌（色温、圆角半径、阴影扩散度、字重对比、弹簧阻尼）。
3. **不要什么（Anti-Patterns）**：
   - 如：*“不要生硬的表格线”*、*“不要满屏五颜六色的后台管理风”*、*“不要跳页打断心流”*、*“不要默认呆板的 FloatingActionButton”*。

### Step 2：先描述蓝图，等待确认（Describe Before Coding）
**在动手写代码之前**，用精炼的语言（控制在 15 行内）向用户描述即将呈现的效果：
- **空间与版面**：顶部氛围光晕、主视觉舞台（Hero Stage）、中部交互区、底部吸顶/悬浮操作区。
- **材质与色彩**：主底色、卡片悬浮层级、微晶图标舱（Icon Pod）、明暗双生对比度。
- **灵魂微交互**：入场阶梯动效（Staggered Entrance）、手势按压反馈（Pressable Tilt / Scale）、状态连续形变（Continuity Morph）。
- 询问用户：*“这个视觉与交互方向符合预期吗？确认后我立即为您实现，或者告诉我哪部分需要调整。”*（若用户明确要求“直接写”，则输出简要蓝图后同回合直接生成代码）。

### Step 3：高质感实现（High-Craft Implementation）
编写代码时必须落实以下 6 条铁律：
1. **顶部环境微光（Ambient Radial Glow）**：页面或半屏弹窗顶部布置柔和的径向渐变氛围光晕（不透明度 `0.10 ~ 0.22`），消除死白/死黑顶栏。
2. **微晶图标舱（Icon Pod）**：列表项、设置项、权益卡的图标绝不裸放，装入带 10%~15% 主题色半透明底色 + 高光细边框的圆角方舱中。
3. **就地形变，拒绝跳页（In-Place Continuity）**：能原地展开、原地翻转、原地变成输入框/步进器的，绝不新开页面打断心流。
4. **物理因果与触感同步（Causal Motion & Haptics）**：
   - 入场一律用减速曲线（`Curves.easeOutCubic` / `Curves.easeOutBack`），退场用加速曲线（`Curves.easeInCubic`）；
   - 高频点击动效控制在 `120ms ~ 220ms`，并在触发瞬间同步调用 `HapticFeedback.lightImpact()` 或 `selectionClick()`。
5. **渐进式信息披露（Progressive Disclosure）**：次要参数、多维明细默认折叠为一行精致胶囊摘要（如 `🔥 520 kcal · 蛋白 32g  [展开微调 ▾]`），点击才平滑撑开。
6. **单行中文与小屏防溢出**：所有中文标签强制 `maxLines: 1, overflow: TextOverflow.ellipsis` 或 `FittedBox(fit: BoxFit.scaleDown)`，在 375px 窄屏与长英文文案下零溢出。

### Step 4：局部手术式微调（Surgical Iteration）
当用户提出修改意见时，只针对不满意的局部区域（版式、色温、阻尼、字号）做精准微调，严禁推翻已确认满意的模块。

---

## 内置高质感组件提示词库（可直接复用或组合）

### A. 6 大质感微交互组件（`ux_micro_interactions.dart`）
1. **多选筛选标签 (`MultiSelectFilterChips`)**：做一排可多选的筛选标签，选中时左侧长出对勾、底色填满、宽度平滑撑开，相邻标签同步让位，筛选按钮上的数量跳动更新。
2. **可删除的标签 (`DeletableTagInput`)**：在输入框里做一组可删除的标签，点击叉号时标签缩成圆点后消失，后续标签依次平滑补位，输入框高度随之收缩。
3. **输入联想浮层 (`InlineMentionAutocomplete`)**：输入时在光标旁弹出联想列表，随输入实时筛选，选中后以标签形式插入正文，浮层收起。
4. **拨动字号刻度 (`KeyboardFontScaleBar`)**：在键盘顶部加一排横向字号刻度，当前值居中高亮，两侧渐隐，左右拨动时正文字号实时变化，停下后吸附到最近的刻度。
5. **按压倾斜卡片 (`PressableTiltSpotlightCard`)**：做一张可按压的卡片，按下时朝触点方向轻微倾斜并缩小一点，背后的光斑向触点聚拢，松手弹性复原。
6. **长按变录音条 (`HoldToRecordBar`)**：底部做一个双按钮组，长按麦克风时按钮组横向拉长成录音条并显示实时波形，加号旋转成取消，松手复原。

### B. 10 大自然连贯转场组件（`ux_continuity_transitions.dart`）
1. **搜索框展开 (`MorphingSearchBar`)**：图标直接形变输入框，光标自动定位，不跳页。
2. **加号展开面板 (`ExpandableFabPanel`)**：悬浮按钮原位展开成操作面板，关闭时收回。
3. **提交状态反馈 (`TriStateSubmitButton`)**：按钮同位置依次显示加载、完成、结果。
4. **图标变形 (`MorphingMenuPlayIcon`)**：菜单三线用路径插值变成关闭图标，播放键同理变暂停。
5. **标签指示条 (`OvershootTabIndicatorBar`)**：切换时底部指示条先拉向新位置再收回，带超调。
6. **按钮变步进器 (`MorphingStepperButton`)**：加入按钮展开成数量选择器，归零自动复原。
7. **列表展开详情 (`InPlaceExpandableList`)**：选中项就地展开，其余内容顺势下移，不跳页。
8. **列表切换网格 (`FlipListGridSwitcher`)**：用 FLIP 布局形变，各项沿弧线缓动到新位置，不重载。
9. **顶栏随滚动收起 (`ScrollCollapsingHeaderBar`)**：滚动进度驱动搜索栏收成图标，反向滚动还原。
10. **菜单铺满全屏 (`CircularRevealFullscreenMenu`)**：以按钮为中心圆形揭示转场，菜单项交错进入。

### C. 10 大图表交互控件（`ux_interactive_charts.dart`）
1. **活动圆环 Rings (`ActivityRingsChart`)**：同心圆从零画到当前值，超标部分叠加深色。
2. **打卡热力格 Heatmap (`CheckinHeatmapGrid`)**：按次数分五档深浅，点击某天弹详情。
3. **仪表盘 Gauge (`SpringGaugeChart`)**：半圆刻度三段颜色，指针扫入带轻微回弹。
4. **柱状图 Bars (`InteractivePeriodBarChart`)**：周月切换柱高平滑过渡，按住某根其余变暗。
5. **专注分段图 Focus (`FocusTimelineChart`)**：时间轴按阶段分高低和颜色，滑动显示当前阶段。
6. **范围折线图 Range (`MorphingRangeLineChart`)**：切换一周一月一年，线条平滑变形、面积渐变。
7. **目标线 Goal (`DraggableGoalBarChart`)**：可拖动目标线，过线柱子高亮，实时显示达标天数。
8. **分段占比条 Stacked (`SegmentedStackedBar`)**：一整条分段留细缝，点击抽出显示名称和百分比。
9. **环形占比图 Share (`InteractiveDonutShareChart`)**：各段顺时针画出，点击外移高亮，中心显示占比。
10. **气泡图 Bubble (`PhysicsBubbleClusterChart`)**：圆大小表示数量，点击放大居中，其余被推开。
