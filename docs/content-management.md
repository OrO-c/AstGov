# 文章与页面管理

## 内容结构概览

AstGov 使用 Astro 6 的 **Content Layer API** 管理三类内容：

```
src/content/
├── posts/                      ← 文章（Markdown，按分类子目录组织）
│   ├── news/001.md
│   └── announce/001.md
├── banner/                     ← 轮播图（每张图一个 md 文件）
│   └── slide-1.md
└── headline/                   ← 头条卡片（每张卡片一个 md 文件）
    └── card-1.md
```

内容集合的配置位于项目根目录下的 `src/content.config.ts`，无需手动编辑。

---

## 文章（posts）

### 文章放在哪里

所有文章文件放在 `src/content/posts/` 下，按分类放在子目录中：

```
src/content/posts/
├── news/001.md         →  /news/001
├── news/002.md         →  /news/002
├── announce/001.md     →  /announce/001
└── (其他分类/...)
```

- 子目录名 = 分类标识（slug），与 `site.config.ts` 中 `categories` 的 `slug` 对应
- 文件名 = URL 最后一段（不含 `.md`）
- 每个文章条目的 `id` 即路径本身，例如 `news/001`

### 文章格式

每篇文章是一个 Markdown 文件，包含 frontmatter 和正文：

```markdown
---
title: 文章标题              # 必填，string
date: "2026-07-01"          # 必填，string，格式 YYYY-MM-DD，引号不能少
category: news               # 选填，string。不填则从目录名自动推断
author: 张三                 # 选填，string，默认"本站"
tags: ["新闻", "通知"]       # 选填，string 数组
summary: 文章摘要            # 选填，string，显示在文章详情页顶部
draft: false                 # 选填，boolean。true 表示草稿，不会发布
image: /images/cover.jpg     # 选填，string，头图路径
---

正文内容，支持完整 Markdown 语法。
```

### frontmatter 字段一览

| 字段 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| `title` | string | 是 | — | 文章标题 |
| `date` | string | 是 | — | 发布日期，格式 `"YYYY-MM-DD"`，**必须加引号** |
| `category` | string | 否 | 从目录名推断 | 文章分类，需与配置中 `categories[].slug` 一致 |
| `author` | string | 否 | `"本站"` | 作者名 |
| `tags` | string[] | 否 | `[]` | 标签数组 |
| `summary` | string | 否 | `""` | 文章摘要，显示在详情页顶部 |
| `draft` | boolean | 否 | `false` | 草稿模式。设为 `true` 则构建时不会输出 |
| `image` | string | 否 | — | 头图路径 |

### 分类规则

文章的 `category` 字段决定它属于哪个分类。如果不填，则从文件所在的子目录名推断：

```
src/content/posts/news/001.md     →  category = "news"
src/content/posts/announce/001.md →  category = "announce"
```

首页按 `categories` 配置中的 `slug` 匹配文章，显示在对应的分区中。

### URL 规则

```
src/content/posts/news/001.md     →  /news/001
src/content/posts/news/002.md     →  /news/002
src/content/posts/announce/001.md →  /announce/001
```

### 草稿模式

在 frontmatter 中设置 `draft: true`，草稿在 dev 中可见但 `npm run build` 时不会输出。

### 文章排序

按 `date` 字段倒序排列，最新的在最前面。

---

## 轮播图（banner）

轮播图不再写在 `site.config.ts` 中，而是以独立的 Markdown 文件管理。每张轮播图对应 `src/content/banner/` 下的一个 `.md` 文件。

```markdown
---
src: /images/banner1.jpg     # 图片路径（public/ 下，以 / 开头）
alt: 校园风光                # 替代文本
link: /about                 # 点击跳转地址（可选）
width: 1200                  # 图片宽度（可选，推荐提供以避免 CLS）
height: 400                  # 图片高度（可选）
order: 1                     # 排序序号（数字越小越靠前）
---
```

增加轮播图：在 `src/content/banner/` 下新建 `.md` 文件即可。删除文件即移除该图。

---

## 头条卡片（headline）

头条卡片同样以独立 Markdown 文件管理，位于 `src/content/headline/`：

```markdown
---
src: /images/card1.jpg       # 图片路径
alt: 招生简章                # 替代文本
title: 2026 年本科招生简章发布  # 卡片标题
date: "2026-06-01"          # 日期（必须加引号）
link: /admission             # 点击跳转地址（可选）
width: 400                   # 图片宽度（可选）
height: 250                  # 图片高度（可选）
order: 1                     # 排序序号
---
```

---

## 文章内图片

图片放在 `public/images/` 目录下，正文中引用：

```markdown
![图片说明](/images/photo.jpg)
```

**路径规则**：以 `/` 开头，不加 `public`。

> 主题组件（轮播图、头条卡片、页脚二维码）中的图片已全部使用 `astro:assets` 的 `<Image />` 组件渲染，提供自动尺寸推断和懒加载优化。

---

## 页面管理

### 首页
`src/pages/index.astro` — 从 content 集合和配置自动生成。不需要手动修改。

### 文章详情页
`src/pages/[category]/[slug].astro` — 自动处理所有文章。不需要手动修改。

### 分类列表页
`src/pages/news/index.astro` — 显示指定分类的所有文章。如需其他分类的列表页，复制此文件并修改过滤条件。

### 独立页面
`src/pages/about.astro` 和 `src/pages/contact.astro` 是独立页面，内容直接写在页面文件中。

## 添加新页面

```astro
---
import { BaseLayout } from '@astgov/theme';
import { siteConfig } from '../config/site.config';
---

<BaseLayout config={siteConfig} title="新页面标题">
  <h1>新页面</h1>
  <p>页面内容……</p>
</BaseLayout>
```

> `BaseLayout` 支持通过 `config` 属性一次性传入整个站点配置，无需逐个列出 props。

## 删除不需要的页面
直接删除 `src/pages/` 下对应的文件，同时移除 `nav` 配置中的菜单项。
