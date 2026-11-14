# Kova 网站 / Kova site

扣瓦（Kova）的中英双语介绍站，基于 [Mintlify](https://mintlify.com) 构建。

The bilingual (Chinese / English) site for Kova, built on [Mintlify](https://mintlify.com).

## 本地预览 / Preview locally

```bash
npm i -g mintlify        # 或 npx mintlify
mintlify dev             # 在仓库根目录执行
```

## 结构 / Layout

```
docs.json          Mintlify 配置：暗色主题、品牌色、双语导航、页脚
landing.css        落地页专用视觉系统（仅 index.mdx / en/index.mdx 引用）
favicon.png        站点图标（应用吉祥物）
logo.png           导航栏 logo（同上，自带底色，明暗主题通用）
index.mdx          落地页（默认语言：简体中文，URL 为 /）
quickstart.mdx     快速开始
features/          功能详解（17 页：对话 / Agent 引擎 / 循环视图 / 模型服务 / 队列 / 缓存 /
                   模式 / 沙箱 / goal / 子代理 / 能力模型 / MCP / 插件 / 自动化 /
                   IM 通道 / 远程移动 / 安全密钥）
developers/        开发者文档（3 页）
reference/         路线图、开源许可
en/                English（mirror of the root pages, URL 为 /en/...）
```

**目录约定不能改**：Mintlify 的多语言约定是「默认语言放根目录，其他语言放以语言代码
命名的子目录」，且**非默认语言的导航页面路径必须带目录前缀**（`"en/quickstart"`），
否则该语言的页面不进自己的导航，界面语言也不会随 URL 切换。
`docs.json` 里 `languages[].language` 只声明语言代码，没有单独指定路径的字段
（`directory` 是导航显示方式，取值 `none/accordion/card`）。

**全屏落地页**：落地页通过 CSS 隐藏文档 chrome（左侧栏、右侧目录、页头）并清空文章列的
内边距，实现通栏。所有规则都用 `body:has(.kov)` 限定作用域——只有带 `.kov` 根节点的
落地页命中，文档页不受影响。

**MDX 标签白名单**：Mintlify 的 MDX 管线会丢弃 `<header>`、`<article>` 等语义标签
（DOM 里直接消失），也会把 `<a>` 的子元素包进 `<span data-as="p">`。
落地页里只使用 `div / section / span / p / a / h1-h3 / pre / code`；
按钮内部的包装层用 `display: contents` 处理（见 landing.css）。

两种语言的页面一一对应。改动一侧时请同步另一侧——
导航是按语言分别声明的，不会自动互链。

The two languages mirror each other page for page. Navigation is declared per language
and does not cross-link automatically, so update both sides when you change a page.

## 发布 / Deploy

Mintlify 托管需要把本仓库（根目录即站点根目录）推送到 GitHub，然后用 Mintlify CLI 连接仓库：

```bash
mintlify connect
```

若要托管到其他静态平台，`docs.json` 里的 Mintlify 专有组件（`<Hero>`、`<Card>` 等）
需要先经过 Mintlify 的构建步骤才能得到纯静态 HTML。