# 目录结构

理解项目里每个文件的作用。

## 整体结构

```
my-site/                          ← 你的网站根目录
├── src/
│   ├── config/
│   │   ├── site.config.ts        ← 站点配置（你主要改这个文件）
│   │   └── types.ts              ← 类型定义（不需要动）
│   ├── content/
│   │   └── pages/                ← 文章放在这里
│   │       ├── news/
│   │       │   ├── 001.md        ← 一篇新闻
│   │       │   └── 002.md        ← 另一篇新闻
│   │       └── announce/
│   │           └── 001.md        ← 一篇公告
│   ├── content.config.ts         ← Content Layer API 配置（Astro 6 新增）
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

| 文件 | 作用 | 改动频率 |
|------|------|---------|
| `src/config/site.config.ts` | 所有配置 | 每次改网站都改 |
| `src/content/pages/` | 文章内容 | 每次发文都改 |
| `public/images/` | 图片资源 | 换图时改 |

## 你不需要碰这些

| 文件/目录 | 为什么 |
|-----------|--------|
| `src/config/types.ts` | 类型定义，主题包提供 |
| `src/content.config.ts` | Content Layer API 配置，已设定好（Astro 6 自动读取） |
| `src/layouts/` | 布局文件，转发给主题包 |
| `astro.config.mjs` | 已配置好，除非你要改构建选项 |
| `node_modules/` | 依赖包，不要手动修改 |
| `dist/` | 构建产物，每次构建自动生成 |

> **注意：** 相比 Astro 4.x 时代，内容集合配置文件从 `src/content/config.ts` **迁移到了根目录的 `src/content.config.ts`**，并使用 Content Layer API 的 `glob()` loader。这是 Astro 6 的强制要求。旧文件 `src/content/config.ts` 已不存在。

## 主题包提供了什么

`@astgov/theme` 这个 npm 包提供了：
- 布局（BaseLayout、BlogLayout）
- 组件（Header、Nav、Footer、Banner、CategorySection 等）
- 全局样式

这些都在 `node_modules/@astgov/theme/` 下，**不要手动修改**。升级主题包版本后会自动更新。
