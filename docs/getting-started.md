# 快速开始

跟着本文，5 分钟内跑起一个可用的网站。

## 环境要求

- **Node.js 22.12.0 或更高版本**（Astro 6.x 不再支持 Node 18/20）
- npm、pnpm 或 yarn 任意包管理器

## 安装

```bash
# 1. 克隆示例网站仓库
git clone https://github.com/OrO-c/astgov-demo-site my-site
cd my-site

# 2. 安装依赖
npm install
```

## 启动

```bash
npm run dev
```

浏览器打开 `http://localhost:4321`。

你会看到一个大学风格的示例网站，包含：
- 顶部工具栏（快捷入口、字号切换）
- 导航菜单
- 轮播图
- 文章列表（按分类展示）
- 底部信息（联系方式、备案号）

## 修改配置

打开 `src/config/site.config.ts`，搜索 `// TODO` 找到所有需要替换的示例数据：

```typescript
siteName: '王俊伊大学',              // ← 改成你的网站名称
siteSubtitle: 'Wangjunyi University', // ← 改成你的副标题
```

改完后保存，浏览器会自动刷新。

## 写第一篇文章

在 `src/content/posts/news/` 下新建一个 `.md` 文件：

```markdown
---
title: 我的第一篇文章
date: "2026-07-01"
author: 你的名字
tags: ["新闻"]
summary: 这是文章摘要
---

这是正文，支持 **Markdown** 语法。

## 二级标题

正文内容……
```

保存后，文章会自动出现在首页和 `/news/` 列表页。

**注意：** 文章内容由 Astro 6 Content Layer API 管理，配置文件在 `src/content.config.ts`（不是 `src/content/config.ts`），无需手动编辑。

## 构建与预览

```bash
npm run build     # 生成静态文件到 dist/
npm run preview   # 预览构建结果
```

构建完成后，把 `dist/` 目录上传到服务器即可上线。

## 下一步

- [目录结构](./directory-structure.md) — 了解每个文件的作用
- [配置详解](./configuration.md) — 查看所有可配置项
- [内容管理](./content-management.md) — 学习如何管理文章
- [部署指南](./deployment.md) — 发布到线上
