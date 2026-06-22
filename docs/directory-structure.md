# 目录结构

理解项目里每个文件的作用。

## 整体结构

```
my-site/                          ← 你的网站根目录
├── src/
│   ├── config/
│   │   ├── site.config.ts        ← 站点配置（你主要改这个文件）
│   │   ├── types.ts              ← 类型定义（不需要动）
│   │   └── page.ts              ← 页面通用导入（BaseLayout 从主题包具体路径引入）
│   ├── content/
│   │   ├── posts/                ← 文章放在这里（按分类子目录）
│   │   │   ├── news/
│   │   │   │   └── 001.md        ← 一篇新闻
│   │   │   └── announce/
│   │   │       └── 001.md        ← 一篇公告
│   │   ├── banner/               ← 轮播图（每张图一个 md）
│   │   │   └── slide-1.md
│   │   └── headline/             ← 头条卡片（每张卡片一个 md）
│   │       └── card-1.md
│   ├── content.config.ts         ← Content Layer API 配置（无需手动编辑）
│   ├── pages/
│   │   ├── index.astro           ← 首页
│   │   ├── [category]/[slug].astro  ← 文章详情页（自动处理所有文章）
│   │   ├── news/index.astro      ← 新闻列表页
│   │   ├── about.astro           ← 关于我们
│   │   └── contact.astro         ← 联系我们
│   ├── layouts/
│   │   └── BaseLayout.astro      ← 布局转发文件（不需要动）
│   └── styles/
│       └── overrides.css         ← 自定义样式（可选）
├── public/
│   └── images/                   ← 图片放这里
├── astro.config.mjs              ← Astro 配置
├── package.json                  ← 项目依赖
└── tsconfig.json                 ← TypeScript 配置
```

## 你只需要改这些

| 文件/目录 | 作用 | 改动频率 |
|-----------|------|---------|
| `src/config/site.config.ts` | 网站名称、导航、配色等配置 | 每次改网站都改 |
| `src/content/posts/` | 文章内容 | 每次发文都改 |
| `src/content/banner/` | 轮播图 | 换图时改 |
| `src/content/headline/` | 头条卡片 | 换内容时改 |
| `public/images/` | 图片资源 | 换图时改 |

## 你不需要碰这些

| 文件/目录 | 为什么 |
|-----------|--------|
| `src/config/types.ts` | 类型定义，主题包提供 |
| `src/content.config.ts` | Content Layer API 配置，已设定好 |
| `src/layouts/` | 布局文件，转发给主题包 |
| `astro.config.mjs` | 已配置好，除非你要改构建选项 |
| `node_modules/` | 依赖包，不要手动修改 |
| `dist/` | 构建产物，每次构建自动生成 |

## 主题包提供了什么

`@astgov/theme` 这个 npm 包提供了：
- 布局（BaseLayout、BlogLayout）
- 组件（Header、Nav、Footer、Banner、CategorySection 等）
- 全局样式

这些都在 `node_modules/@astgov/theme/` 下，**不要手动修改**。升级主题包版本后会自动更新。
