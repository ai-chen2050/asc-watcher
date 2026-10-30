---
name: impeccable
description: 页面与交互完成后的收尾审查（audit）与像素级打磨（polish）。当用户输入 `/impeccable audit`、`/impeccable polish` 或要求“做完收尾检查、打分挑问题、把细节打磨干净”时触发。
---

# Impeccable — 严苛审美审计与像素级收尾打磨

> **万能公式**：做完页面后，先输 `/impeccable audit` 打分挑问题，挑几条后再输 `/impeccable polish` 把细节打磨干净。

## 两种运行模式

### 模式一：`/impeccable audit [目标页面或文件]`（打分挑刺）
对目标页面或当前改动区域执行 **8 大维度严苛审查（满分 100 分）**，绝不客套，直接指出具体到文件名、行号与视觉症状的问题清单：

1. **排版与中日韩单行纪律（Typography & CJK Discipline，15分）**
   - 中文标题/标签/胶囊是否漏加 `maxLines: 1, overflow: TextOverflow.ellipsis` 或 `FittedBox` 导致两字折行？
   - 数字变化（倒计时、金额、进度、步进器）是否启用了等宽数字（`FontFeature.tabularFigures()`）防止左右抖动？
   - 字号、字重、行高是否拉开了清晰的 3 级层次（主标题 `w800~w900`、正文 `w500~w600`、辅助说明 `textSub/textFaint`）？

2. **表面材质与明暗双生对比度（Surface & Adaptive Contrast，15分）**
   - 是否有写死的 `Colors.white` / `Colors.black` 导致深色模式下惨白刺眼或浅色模式下发灰发脏？
   - 浅色模式下卡片是否有细腻边框或柔和投影？深色模式下是否用微光描边（`white.withValues(alpha: 0.08~0.12)`）替代无效黑影？
   - 弹窗是否出现了重复的两个顶部拖拽横条（Dual Drag Handles）或标题重复 Emoji？

3. **动效连贯性与物理曲线方向（Motion Continuity & Physics，15分）**
   - 入场是否用了 `easeOut*` / `easeOutBack`？退场是否用了 `easeIn*`？（严禁入场用 `easeIn`！）
   - 高频点击动效是否超过 300ms 显得拖沓？
   - 是否有无限 `.repeat()` 动画未做生命周期管控或导致 Widget 测试 `pumpAndSettle()` 死锁？
   - 滚动视差或动画包装器是否在切换状态时改变了 Widget 树结构导致子组件 State 重置？

4. **触控热区与手势竞技场零延迟（Touch Target & Gesture Arena，15分）**
   - 所有可点击元素热区是否 `>= 44×44dp`？
   - 嵌套在 2D 拖拽画布或缩放容器内的点击事件是否被 `GestureDetector` 竞技场吞噬？（复杂画布应使用底层 `Listener` + 位移阈值判定）。
   - 点击回调是否挂在了真正接收事件的层级上，且配有触感反馈（`Haptics` / `HapticFeedback`）？

5. **空间呼吸感与小屏/多语言防溢出（Spatial Rhythm & 375px Safety，15分）**
   - 在 `375px` 宽度小屏及切换为英文长文案时，Row 内是否漏加 `Expanded` / `Flexible` 导致 `RenderFlex overflowed`？
   - 底部操作栏是否包裹了 `SafeArea(top: false)` 避开手势横条，且未贴边误触系统返回手势？
   - 页面内容较少（`maxScrollExtent <= 80`）时，右下角悬浮按钮是否误触发了滚动收缩？

6. **信息密度与渐进式披露（Information Density，10分）**
   - 首屏是否堆砌了过多低频输入框或冗长解释文案？能否折叠为紧凑胶囊摘要？
   - 相邻页面是否存在重复的功能入口？

7. **分享海报与离屏截图完整性（Snapshot & Poster Integrity，10分）**
   - `RepaintBoundary` 导出的海报最外层是否误加了 `ClipRRect` / `BorderRadius` 导致保存在相册或微信分享时出现“四个顶角白弧/黑弧”？（导出画布内层必须是 100% 满版直角矩形底色）。

8. **无障碍与减少动态效果（Accessibility & Reduced Motion，5分）**
   - 是否尊重 `MediaQuery.disableAnimationsOf(context)`？

#### Audit 输出格式
```markdown
## 🔍 Impeccable Audit 报告 · 总分：XX / 100

| 维度 | 得分 | 核心诊断 |
|---|---|---|
| 1. 排版与单行纪律 | X/15 | ... |
| ... | ... | ... |

### 🎯 挑出的具体问题清单（按优先级排序，回复序号即可一键 `/impeccable polish`）：
1. **[P0 · 溢出/交互隐患]** `文件路径:行号` — 问题描述 → 修复方案
2. **[P1 · 质感/动效欠佳]** `文件路径:行号` — 问题描述 → 修复方案
3. **[P2 · 细节抛光]** `文件路径:行号` — 问题描述 → 修复方案
```

---

### 模式二：`/impeccable polish [序号或全部]`（像素级收尾打磨）
根据用户选定的问题序号（若未指定序号，默认修复全部挑出的问题）：
1. 逐条精准修改代码，不破坏任何既有功能；
2. 补齐必要的防溢出保护、等宽数字、微光描边、触感反馈与动画曲线；
3. 自动运行静态分析（`flutter analyze`）与单元测试（`flutter test`），确保 0 error、测试全绿。
