# CHANGELOG · asc-watcher

## 2026-10-30

- 使用 `/Users/blake/Agy/Code/app-ai-creator` 企业级「Playground-First」官网脚手架全面升级站点视觉与交互：
  - 升级 `style.css` 为科技蓝 (`#3b82f6`) + 赛博青 (`#06b6d4`) + 翡翠绿 (`#10b981`) 明暗双生设计系统；
  - 重构 `index.html` 与 `index.en.html`：新增 `#heroCanvas` 动态粒子星网、发光旋转 App 徽章、首屏双舞台实机预览、3 模式可交互演练舱（`#playground`：柱状图单日构成点选拆解、用户评论直接回复模拟、过审版本一键发布 + 实时端侧遥测与白盒执行链）、实机真容截图灯箱画廊（`#shots`）、7 维对比矩阵（`#compare`）、100 次免弹窗 + Pro 透明定价矩阵（`#pricing`）与 FAQ 手风琴（`#faq`）；
  - 更新 `site.config.json` 并通过 `make legal-huawei && make build && make geo-check` 零错误校验。
- 初始化 ASC Watcher 官方网站（`https://ai-chen2050.github.io/asc-watcher`）：
  - 编写 `site.config.json` 唯一真相源；
  - 完成中英双语首页（`index.html` / `index.en.html`）、功能机制与问答（`geo.html` / `geo.en.html`）、技术支持（`support.html` / `support.en.html`）、隐私政策（`privacy.html` / `privacy.en.html` 及 `.huawei` 渠道版）、用户协议（`terms.html` / `terms.en.html` 及 `.huawei` 渠道版）与首篇开发日记；
  - 生成 `llms.txt`、`llms-full.txt`、`sitemap.xml`、`robots.txt` 并通过 `make geo-check` 一致性体检。
