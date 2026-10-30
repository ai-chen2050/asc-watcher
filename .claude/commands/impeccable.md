---
description: 执行 Impeccable UI 审美审计（audit 打分挑刺）或像素级收尾打磨（polish）。用法：/impeccable audit [文件/页面] 或 /impeccable polish [问题序号]
---

请读取并严格遵循 `.claude/skills/impeccable/SKILL.md` 技能规范，对当前目标页面或最近修改的 UI 代码执行收尾检查或打磨：

用户输入参数：`$ARGUMENTS`

- 如果参数以 `audit` 开头（或为空）：
  执行 **模式一（`/impeccable audit`）** —— 按 8 大审美与工程维度打分（满分 100），列出带精确 `文件路径:行号` 与修复方案的编号问题清单，供用户挑选。
- 如果参数以 `polish` 开头（或指定了要修复的条目）：
  执行 **模式二（`/impeccable polish`）** —— 针对选定条目或全部待修问题进行像素级打磨，完成后运行 `analyze` 与 `test` 验证零报错全通过。
