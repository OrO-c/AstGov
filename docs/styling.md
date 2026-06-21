# 样式定制

AstGov 使用原生 CSS，没有使用任何 CSS 框架。所有样式均可覆盖。

## 全局样式

全局样式由主题包提供，文件在：

```
node_modules/@astgov/theme/src/styles/global.css
```

**不要直接修改这个文件**，升级主题包后你的修改会丢失。

## 覆盖主题包样式

在 `src/styles/overrides.css` 中添加你的样式：

```css
/* 例：修改页面背景色 */
body {
  background-color: #FAFAFA;
}

/* 例：修改模块标题样式 */
.module-title {
  font-size: 18px;
  color: #1A3C6E;
}
```

`overrides.css` 已经引入到项目中，你只需要往里面写 CSS 就行。

## 调整文章排版样式

文章正文的样式在 `src/pages/[category]/[slug].astro` 文件的 `<style>` 标签中。

你可以修改：

- `.article-content` — 正文字号、行高、颜色
- `.article-content h2` — 二级标题
- `.article-content img` — 图片样式
- `.article-content blockquote` — 引用样式
- `.article-content code` / `pre` — 代码样式
- `.article-content table` — 表格样式

示例：把正文字号从 16px 改为 18px：

```css
.article-content {
  font-size: 18px;
  line-height: 2;
}
```

## 换字体

方法一：在 `site.config.ts` 的 `colors.fontFamily` 中修改：

```typescript
colors: {
  // ...其他色值
  fontFamily: '"微软雅黑", "Microsoft YaHei", sans-serif',
}
```

这会改变全站的正文字体。

方法二：在 `overrides.css` 中覆盖：

```css
body {
  font-family: "思源宋体", "Source Han Serif", serif;
}
```

标题字体在 `--font-heading` CSS 变量中定义，你也可以覆盖它：

```css
:root {
  --font-heading: "思源黑体", "Source Han Sans", sans-serif;
}
```

## CSS 变量一览

| 变量名 | 作用 | 默认值 |
|--------|------|--------|
| `--color-primary` | 主色 | `#003366` |
| `--color-secondary` | 辅色 | `#CC0000` |
| `--color-bg` | 页面背景 | `#F5F5F5` |
| `--color-text` | 正文颜色 | `#333333` |
| `--color-link` | 链接颜色 | `#0000FF` |
| `--color-border` | 边框颜色 | `#CCCCCC` |
| `--color-divider` | 分割线颜色 | `#EEEEEE` |
| `--font-body` | 正文字体 | `"宋体", "SimSun", serif` |
| `--font-heading` | 标题字体 | `"黑体", "SimHei", sans-serif` |

在 `overrides.css` 中覆盖这些变量即可全局修改对应颜色和字体。

## 图片渲染（astro:assets）

主题组件中的图片已全部使用 Astro 6 内置的 `<Image />` 组件（来自 `astro:assets`）替代原生 `<img>` 标签。涉及组件：

- **Header** — 网站 Logo
- **Banner** — 轮播图
- **Headline** — 头条卡片
- **Footer** — 二维码

`<Image />` 自动提供以下优化：

- 自动添加 `alt`、`loading="lazy"`、`decoding="async"` 属性
- 自动推断图片尺寸，避免页面布局偏移（CLS）
- 构建时自动转换为 WebP 格式（如需）
- 支持 `class` 属性透传到底层 `<img>` 元素

文章正文中的 Markdown 图片（`![](/images/photo.jpg)`）不受影响，沿用原生 `<img>` 渲染。
