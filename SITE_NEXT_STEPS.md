# SITE_NEXT_STEPS · ASC Watcher 官网

生成于 2026-10-30。做完划掉，全清空后删掉本文件。

## 1. 配置（先做这个，其它都从它生成）

- [ ] `site.config.json` → `site.domain` 换成真实域名
      （**还是 example.com 的话 canonical / sitemap / llms.txt 全指向错地址**）
- [ ] `site.config.json` → `site.ga4`（如 `G-1CQNQNPK1Y`）与 `site.baidu`（如 `b3a7b6fedcba22d39897693d6f716035`）确认已配置
      （**⚠️ 挂在 GitHub Pages 子路径如 `xxx.github.io/app/` 时，主站统计代码不会自动继承到子仓，必须配置并在 `make build` 后注入全站每个 HTML 页面 + SPA 路由监听**）
- [ ] `site.type`：应用填 `SoftwareApplication`，游戏填 `VideoGame`
- [ ] `site.description` —— 两三句，写给人也写给 AI，要和商店描述口径一致
- [ ] `features` —— 每条都要有 `detail`，空条目会进 llms-full.txt
- [ ] `faq` —— **每个问题都要有完整答案**，空答案是明确的负面信号
- [ ] `differentiators` —— 对比表，AI 回答"X 和 Y 的区别"时直接引用
- [ ] `citationTriggers` —— 你希望在哪些提问下被推荐（写用户真会问的原话）
- [ ] `links` —— 上架后填各渠道地址，没有的留空字符串

## 2. 页面文本

- [ ] `index.html` 的 TODO 全部换掉
- [ ] `geo.html` —— 这是 GEO 主战场，把机制写具体（步骤、优先级、数字）
- [ ] `privacy.html` —— 必须与代码事实一致，商店会核对；AI 相关段落包进 `<!-- AI-ONLY -->`
- [ ] `terms.html` —— 使用条款（App 内「关于」页和付费半屏底部链到它；国内商店当必填）
- [ ] 出国内渠道包的话：`make legal-huawei` 生成无 AI 版，商店页填 `privacy.huawei.html`
- [ ] 产品仓：`lib/services/legal_links.dart` 的 `host` 填成本站域名，`make legal` 抓离线全文
- [ ] `support.html` —— 邮箱、常见问题
- [ ] 英文版 `index.en.html` / `geo.en.html` —— **重写而非直译**
- [ ] 各页 `pages[].description` 各写一段（不写会全站共用 `site.description`）
- [ ] **多语站：`site.config.json` 里的文本字段写成 `{"zh-CN": "…", "en": "…"}`**
      —— 只翻页面正文的话，英文页的 meta description 和 FAQ 结构化数据还是中文。
      跑完 `make geo` 检查一下 `llms.en.txt` 出来的是不是英文

## 3. 资源

- [ ] `icon.png`（512×512 起）
- [ ] 截图放 `shots/`，在首页引用
- [ ] og 图（社交分享缩略图，建议 1200×630）

## 4. 生成与体检

```bash
make build        # devlog + geo
make geo-check    # 必须全绿
make serve        # http://localhost:4000 看一眼
```

- [ ] `geo-check` 无 error
- [ ] 核心能力具备「交互演练舱（Playground）」：至少 1~3 个可把玩功能演示（双链联动/星系网状/思考流/公式），杜绝死板截图
- [ ] Canvas 动效具备完整的生命周期与防死锁闭环（滑到底部再滑回顶部，动效与鼠标连线持续灵动，不冻结）
- [ ] 明暗两色都严格校验：浅色模式绝无发白渐变文字（对比度超 10:1），Canvas 粒子在白底上浓郁清晰
- [ ] 手机窄屏（360px~412px）下无横向滚动，顶栏文字绝对不竖向挤压折行（强制 `white-space: nowrap; flex-shrink: 0;`），次要链接 `.hide-sm` 折叠并在首屏按钮组中快捷补位

## 5. 部署

- [ ] 推到 GitHub
- [ ] 仓库 Settings → Pages → Source 选 **GitHub Actions**
- [ ] 自定义域名：Settings → Pages → Custom domain，并在 DNS 加 CNAME
- [ ] 部署后线上验：
      ```bash
      curl -s https://你的域名/llms.txt | head -20
      curl -sI https://你的域名/ | head -5
      ```

## 6. 接进发版流程

- [ ] 在产品仓库的发版清单里确认有"同步官网"这一步
- [ ] 每次发版跑 `/geo-refresh <产品仓路径>`

## 7. 持续

- [ ] 每月在 ChatGPT / Claude / Perplexity 里问一遍 `citationTriggers` 里的问题，
      记录会不会提到你、信息对不对
- [ ] 每次有值得讲的技术决策就写一篇开发日记（`/devlog-new`）——
      长内容才是被 AI 引用的主体
