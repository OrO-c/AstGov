# 配置详解

所有配置集中在 `src/config/site.config.ts`。下面逐一说明每个字段的用途和写法。

---

## 站点基本信息

```typescript
siteName: '王俊伊大学',              // 类型: string — 网站标题，显示在浏览器标签和页面顶部
siteSubtitle: 'Wangjunyi University', // 类型: string — 副标题，显示在站名下方
siteLogo: '/images/logo.png',        // 类型: string — Logo 图片（放 public/images/ 下）
siteIcon: '/favicon.svg',            // 类型: string — 浏览器标签图标
siteType: 'university',              // 类型: string — 仅供参考，不影响功能
defaultFontSize: 'medium',           // 类型: 'small' | 'medium' | 'large' — 默认字号
```

## 配色

```typescript
colors: ColorPresets.university,
```

使用预设配色：

| 预设名 | 效果 | 适合 |
|--------|------|------|
| `ColorPresets.university` | 蓝白配 | 大学官网 |
| `ColorPresets.government` | 深蓝配 | 政府网站 |
| `ColorPresets.committee` | 红蓝配 | 赛事委员会 |

也可以直接写色值：

```typescript
colors: {
  primary: '#003366',        // 主色 — 顶栏背景、底部背景
  primaryLight: '#184390',   // 主色浅变体
  secondary: '#CC0000',      // 辅色 — 导航背景、高亮
  accent: '#FF6600',         // 强调色
  background: '#F5F5F5',     // 页面背景
  text: '#333333',           // 正文颜色
  link: '#0000FF',           // 链接颜色
  linkVisited: '#800080',    // 已访问链接颜色
  border: '#CCCCCC',         // 边框颜色
  divider: '#EEEEEE',        // 分割线颜色
  fontFamily: '"宋体", "SimSun", serif',  // 正文字体
}
```

所有色值格式为十六进制（`#RRGGBB`）。

## 顶栏

```typescript
topBar: {
  enabled: true,                           // 类型: boolean — 是否显示顶栏
  quickLinks: [                            // 类型: 数组 — 左侧快捷入口
    { text: '学生', url: '/students' },
    { text: 'English', url: '/en' },
  ],
  rightLinks: [                            // 类型: 数组 — 右侧链接
    { text: '校长信箱', url: 'mailto:admin@example.com' },
  ],
  searchEnabled: false,                    // 类型: boolean — 是否显示搜索框（仅 UI）
  searchPlaceholder: '请输入关键字',        // 类型: string — 搜索框占位文字
}
```

每个链接对象：`{ text: string, url: string, target?: string }`。

## 导航

```typescript
nav: [
  { text: '首页', url: '/' },
  {
    text: '关于我们',
    url: '/about',
    dropdown: [                          // 二级菜单，可选
      { text: '学校简介', url: '/about' },
      { text: '联系我们', url: '/contact' },
    ],
  },
  { text: '报名入口', url: '/signup', highlight: true },  // 橙色高亮背景
]
```

导航项字段：

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `text` | string | 是 | 菜单显示文字 |
| `url` | string | 是 | 链接地址 |
| `target` | string | 否 | `'_blank'` 新窗口打开 |
| `highlight` | boolean | 否 | 橙色高亮背景 |
| `dropdown` | 数组 | 否 | 二级子菜单 |

## 模块开关

```typescript
modules: {
  banner: true,         // 轮播图区域
  headline: true,       // 头条图片卡片
  countdown: false,     // 倒计时
  quickLinks: true,     // 快速通道
  friendLinks: true,    // 友情链接
}
```

设为 `false` 可以隐藏对应的区域。

## 类别定义

类别是 AstGov 的核心概念。每个类别对应 `src/content/pages/` 下的一个子目录。

```typescript
categories: [
  { title: '校园资讯', slug: 'news',     limit: 8, side: 'main' },
  { title: '通知公告', slug: 'announce',  limit: 5, side: 'side' },
  { title: '学术动态', slug: 'academics', limit: 5, side: 'side' },
]
```

每个类别字段：

| 字段 | 类型 | 说明 |
|------|------|------|
| `title` | string | 区块标题，显示在首页 |
| `slug` | string | 标识符，对应子目录名和文章的 `category` 字段 |
| `limit` | number | 首页最多显示多少条 |
| `side` | 'main' \| 'side' | `'main'` = 左栏 70%，`'side'` = 右栏 30% |

想添加新类别，加一行就行：

```typescript
{ title: '媒体关注', slug: 'media', limit: 5, side: 'side' },
```

然后在 `src/content/pages/media/001.md` 里写文章。

## 轮播

```typescript
banner: {
  images: [                             // 图片列表
    { src: '/images/banner1.jpg', alt: '校园风光', link: '/about' },
  ],
  height: 360,                          // 图片高度（像素），默认 360
  autoplayInterval: 4000,               // 自动切换间隔（毫秒），默认 4000
  prevText: '‹',                        // 上一张按钮文字
  nextText: '›',                        // 下一张按钮文字
}
```

每张轮播图：`{ src: string, alt: string, link?: string }`。
- `link` 可选，点击图片跳转地址。

## 头条卡片

```typescript
headlineCards: [
  {
    src: '/images/card1.jpg',    // 图片路径
    alt: '招生简章',              // 替代文本
    title: '2026 年本科招生简章发布',  // 标题
    date: '2026-06-01',          // 日期（YYYY-MM-DD 格式）
    link: '/admission',          // 点击跳转地址（可选）
  },
]
```

## 快速通道

```typescript
quickLinks: [
  { text: '招生信息', url: '/admission' },
  { text: '信息门户', url: 'https://portal.example.com' },
  { text: '图书馆', url: '/library' },
]
```

每个链接：`{ text: string, url: string }`。支持外部链接。

## 友情链接

```typescript
friendLinks: [
  { text: '中华人民共和国教育部', url: 'http://www.moe.gov.cn' },
  { text: '吉林省教育厅', url: 'http://www.jledu.gov.cn' },
]
```

每个链接：`{ text: string, url: string }`。

## 底部

```typescript
footer: {
  organization: '王俊伊大学',                   // 机构名称
  footerLogo: '/images/logo.png',              // 底部 Logo（可选）
  contacts: [                                   // 联系方式
    { label: '地址', value: '吉林省长春市 XX 路 123 号' },
    { label: '电话', value: '0431-88888888' },
  ],
  records: [                                    // 备案信息
    { label: 'ICP备案', value: '吉ICP备 xxxxxx 号', link: 'https://beian.miit.gov.cn' },
  ],
  policeRecord: '吉公网安备 xxxxxxxxx 号',       // 公安备案号（可选）
  links: [                                      // 底部导航
    { text: '关于我们', url: '/about' },
    { text: '网站地图', url: '/sitemap' },
  ],
  extraLinks: [                                 // 底部第二行链接（可选）
    { text: '校长信箱', url: 'mailto:president@example.com' },
  ],
  qrCodes: [                                    // 二维码（可选）
    { src: '/images/qrcode.png', alt: '官方微信' },
  ],
}
```

`records` 中的 `link` 字段可选。有 `link` 则显示为可点击的链接。

## 界面文字

所有界面上固定显示的文字都在这里改：

```typescript
ui: {
  moreText: '更多 →',                      // 列表和区块的"更多"链接文字
  dropdownArrow: '▼',                      // 导航下拉箭头
  copyright: '版权所有 © {year} {org}',    // 底部版权（{year} {org} 会被自动替换）

  quickLinks: {
    title: '快速通道',                     // 快速通道区块标题
    expandText: '展开更多',                // 展开按钮文字
    collapseText: '收起',                  // 收起按钮文字
  },

  friendLinks: {
    title: '友情链接',                     // 友情链接区块标题
  },

  fontSizeLabels: {
    small: '小',                           // 小字号按钮
    medium: '中',                          // 中字号按钮
    large: '大',                           // 大字号按钮
  },

  countdown: {
    labels: {
      days: '天',                          // 天数标签
      hours: '时',                         // 小时标签
      minutes: '分',                       // 分钟标签
      seconds: '秒',                       // 秒钟标签
    },
  },

  dateFormat: '{month}月',                 // 列表日期格式。可用占位符：{year} {month} {day}

  article: {
    authorLabel: '作者：',                 // 文章页面作者标签
    tagLabel: '标签：',                    // 文章页面标签标签
    backLinkText: '← 返回列表',           // 文章底部返回链接文字
  },
}
```

## 元数据

```typescript
meta: {
  keywords: '大学,教育,招生',               // SEO 关键词，逗号分隔
  description: '王俊伊大学官方网站',        // SEO 描述
}
```

## 快速查找表

| 想改什么 | 配置字段 |
|----------|---------|
| 网站标题 | `siteName` |
| 网站副标题 | `siteSubtitle` |
| 网站 Logo | `siteLogo` |
| 浏览器标签图标 | `siteIcon` |
| 整体颜色 | `colors` |
| 顶栏链接 | `topBar.quickLinks` / `topBar.rightLinks` |
| 顶栏搜索框 | `topBar.searchEnabled` |
| 导航菜单 | `nav` |
| 导航高亮项 | `nav[].highlight` |
| 导航下拉菜单 | `nav[].dropdown` |
| 首页显示哪些区域 | `modules` |
| 首页分区 | `categories` |
| 轮播图片 | `banner.images` |
| 轮播高度 | `banner.height` |
| 轮播切换速度 | `banner.autoplayInterval` |
| 头条卡片 | `headlineCards` |
| 快速通道链接 | `quickLinks` |
| 友情链接 | `friendLinks` |
| 底部机构名称 | `footer.organization` |
| 联系方式 | `footer.contacts` |
| 备案号 | `footer.records` / `footer.policeRecord` |
| 底部导航链接 | `footer.links` |
| 底部额外链接 | `footer.extraLinks` |
| 二维码 | `footer.qrCodes` |
| "更多 →" 文字 | `ui.moreText` |
| "快速通道" 标题 | `ui.quickLinks.title` |
| "友情链接" 标题 | `ui.friendLinks.title` |
| 版权文字 | `ui.copyright` |
| 字号按钮文字 | `ui.fontSizeLabels` |
| 倒计时标签 | `ui.countdown.labels` |
| 日期格式 | `ui.dateFormat` |
| 文章页面标签 | `ui.article` |
| 页面关键词/描述 | `meta` |
