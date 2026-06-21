# 文章与页面管理

## 文章放在哪里

所有文章文件放在 `src/content/pages/` 下，按分类放在子目录中：

```
src/content/pages/
├── news/001.md         →  /news/001
├── news/002.md         →  /news/002
├── announce/001.md     →  /announce/001
└── (其他分类/...)
```

- 子目录名 = 分类标识（slug），与 `site.config.ts` 中 `categories` 的 `slug` 对应
- 文件名 = URL 最后一段（不含 `.md`）

## Content Layer API（Astro 6）

本主题使用 Astro 6 的 **Content Layer API** 管理内容。内容集合的配置位于项目根目录下的 `src/content.config.ts`（注意：**不是** `src/content/config.ts`）。

无需手动编辑此文件，主题包已自动配置好 `glob()` loader 读取 `src/content/pages/` 下的 Markdown 文件。

配置内容如下：

```typescript
// src/content.config.ts（不需要改）
import { defineCollection } from 'astro:content';
import { z } from 'astro/zod';
import { glob } from 'astro/loaders';

const pagesCollection = defineCollection({
  loader: glob({ pattern: '**/pages/**/[^_]*.{md,mdx}', base: './src/content' }),
  schema: z.object({
    title: z.string(),
    date: z.string(),
    // ...
  }),
});
```

## 文章格式

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

## 二级标题

- 列表
- 列表

**加粗** *斜体* `行内代码`

> 引用

![图片说明](/images/photo.jpg)

| 表格 | 列2 |
|------|-----|
| 数据 | 数据 |
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

## 分类规则

文章的 `category` 字段决定它属于哪个分类。如果不填，则从文件所在的目录名推断：

```
src/content/pages/news/001.md     →  category = "news"（从目录名自动推断）
src/content/pages/announce/001.md →  category = "announce"
```

首页按 `categories` 配置中的 `slug` 匹配文章，显示在对应的分区中。

## URL 规则

```
src/content/pages/news/001.md     →  /news/001
src/content/pages/news/002.md     →  /news/002
src/content/pages/announce/001.md →  /announce/001
```

文件名（不含扩展名）就是 URL 的最后一段。

> **关于内容标识：** Astro 6 Content Layer API 使用 `id` 而非旧版的 `slug` 作为条目标识符。在本主题中，`id` 格式为 `pages/news/001`，其中 `pages/` 前缀会被自动剥离生成干净 URL。如果您自定义页面逻辑，请使用 `entry.id` 而非 `entry.slug`（后者在 Astro 6 中已移除）。

## 草稿模式

在 frontmatter 中设置：

```markdown
---
title: 还没写完的文章
draft: true
---
```

草稿文章在开发服务器上可见，但执行 `npm run build` 时不会输出到 `dist/`。

## 文章排序

文章按 `date` 字段倒序排列，最新的在最前面。

## 文章内图片

图片放在 `public/images/` 目录下，正文中引用：

```markdown
![图片说明](/images/photo.jpg)
```

**路径规则**：以 `/` 开头，不加 `public`。

支持格式：jpg、png、svg、webp、gif。

图片会自动适应屏幕宽度，带 1px 边框和内边距。

> 主题组件（如轮播图、头条卡片、页脚二维码）中的图片已全部使用 `astro:assets` 的 `<Image />` 组件渲染，提供自动尺寸推断和懒加载优化。

## 页面管理

### 首页

`src/pages/index.astro` — 从配置和文章自动生成。不需要手动修改。

### 文章详情页

`src/pages/[category]/[slug].astro` — 自动处理所有文章。不需要手动修改。

### 分类列表页

`src/pages/news/index.astro` — 显示指定分类的所有文章。如果需要其他分类的列表页，复制这个文件，把分类过滤条件改成对应的 slug。

### 独立页面

`src/pages/about.astro` 和 `src/pages/contact.astro` 是独立页面，内容写在页面文件中。

## 添加新页面

在 `src/pages/` 下创建 `.astro` 文件：

```astro
---
import { BaseLayout } from '@astgov/theme';
import { siteConfig } from '../config/site.config';
---

<BaseLayout
  siteName={siteConfig.siteName}
  title="新页面标题"
  topBar={siteConfig.topBar}
  navItems={siteConfig.nav}
  footer={siteConfig.footer}
  colors={siteConfig.colors}
>
  <h1>新页面</h1>
  <p>页面内容……</p>
</BaseLayout>
```

然后在 `nav` 配置中添加菜单项。

## 删除不需要的页面

直接删除 `src/pages/` 下对应的文件即可。同时记得在 `nav` 配置中移除对应的菜单项。
