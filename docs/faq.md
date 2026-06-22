# 常见问题

## 文章不显示

检查以下几点：

**1. 文件位置对吗？**

文章必须放在 `src/content/posts/` 下的分类子目录中：

```
src/content/posts/news/001.md    ✅ 正确
src/content/posts/001.md         ❌ 错误，缺少分类子目录
```

**2. `date` 字段加引号了吗？**

```markdown
date: "2026-07-01"    ✅ 正确
date: 2026-07-01      ❌ 错误，不加引号会被解析为算术表达式
```

**3. `draft` 是 `true` 吗？**

```markdown
draft: true    ← 草稿模式，构建时不会输出
```

设为 `draft: false` 或直接删掉这行。

## 文章分类不对

文章的分类由两个因素决定：

1. **文件所在的子目录名**（优先级高）
2. **frontmatter 的 `category` 字段**（如果没有子目录）

```
src/content/posts/news/001.md   →  自动归类到 news
```

如果你手动设置了 `category` 字段，确保拼写与配置中的 `slug` 一致。

## 图片不显示

**1. 图片放在哪？**

放在 `public/images/` 目录下。

**2. 路径怎么写？**

```markdown
![说明](/images/photo.jpg)    ✅ 正确，以 / 开头
![说明](photo.jpg)            ❌ 错误，缺少路径
![说明](public/images/photo.jpg)  ❌ 错误，不要加 public
```

**3. 文件名大小写对吗？**

服务器区分大小写。`Photo.jpg` 和 `photo.jpg` 是不同的文件。

## 部署后样式丢失

**1. 检查 `site` 配置**

在 `astro.config.mjs` 中：

```javascript
site: 'https://your-domain.com',  // 改成你的实际域名
```

如果部署到子路径（如 `https://user.github.io/repo/`），需要加 `base`：

```javascript
base: '/repo',
```

**2. 清空浏览器缓存**

按 `Ctrl+Shift+Delete`（Windows）或 `Cmd+Shift+Delete`（Mac），选择清除缓存。

## Astro 6 升级相关

### 项目之前用的是 Astro 4/5，要怎么升级？

参见 `feed-ai/guides/upgrade-to/v6.mdx`。主要变更：

| 变更项 | 旧（4.x） | 新（6.x） |
|--------|-----------|-----------|
| Node.js | 18/20 | **22.12.0+** |
| Astro 版本 | `^4.0.0` | **`^6.4.8`** |
| 内容集合配置 | `src/content/config.ts` | **`src/content.config.ts`** |
| 内容加载方式 | `type: 'content'` | **`glob()` loader**（Content Layer API） |
| 条目渲染 | `entry.render()` | **`render(entry)`** |
| 条目标识符 | `entry.slug` | **`entry.id`** |
| Zod 导入 | `import { z } from 'astro:content'` | **`import { z } from 'astro/zod'`** |
| CSS 变量注入 | `<style is:global set:html={...}>` | **`<Fragment set:html=...>`** |
| 图片组件 | `<img>` | **`<Image />` from `astro:assets`** |
| 实验性配置 | `experimental.globalRoutePriority` | **已移除** |

### `src/content/config.ts` 去哪了？

Astro 6 使用 Content Layer API，内容集合配置文件已移至项目根目录下的 `src/content.config.ts`。旧版 `src/content/config.ts` 已不再使用。

### 还想用 `entry.slug` 怎么办？

Astro 6 的 Content Layer API 使用 `entry.id` 替代了 `entry.slug`。对于存放在 `src/content/posts/news/001.md` 的文件，`id` 值为 `news/001`，可直接用于 URL 路径。

## 如何更新主题包版本

```bash
npm update @astgov/theme
```

更新后检查是否有破坏性变更（查看主题包的发布说明）。

## 如何添加新页面

见 [文章与页面管理](./content-management.md#添加新页面)。

## 如何创建新的文章分类

1. 在 `site.config.ts` 的 `categories` 数组中添加一项
2. 在 `src/content/posts/` 下创建对应的子目录
3. 在子目录中放入 `.md` 文章文件
4. （可选）在 `src/pages/` 下创建该分类的列表页

示例：

```typescript
// site.config.ts 中
categories: [
  // ...现有分类
  { title: '媒体关注', slug: 'media', limit: 5, side: 'side' },
]
```

```bash
# 创建目录
mkdir src/content/posts/media
# 创建文章
```

## 搜索框不能用？

搜索框目前是 UI 展示，没有接入后端搜索服务。你需要自行对接搜索接口，或使用第三方搜索服务（如 Algolia）。

## 如何备份网站

整个项目文件夹就是全部内容。重点备份：

```
src/config/site.config.ts    ← 网站配置
src/content/posts/           ← 所有文章
public/images/               ← 图片资源
src/pages/                   ← 自定义页面（如果有）
```

## 如何贡献代码

主题包仓库：[astgov-theme](https://github.com/your-org/astgov-theme)

示例网站仓库：[astgov-demo-site](https://github.com/OrO-c/astgov-demo-site)

欢迎提交 Issue 和 Pull Request。
