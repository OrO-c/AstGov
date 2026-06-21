# 部署指南

构建产出 `dist/` 目录是纯静态文件，可以部署到任何 Web 服务器或托管平台。

## 构建

```bash
npm run build
```

产出在 `dist/` 目录中。

## Cloudflare Pages（推荐，免费）

1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com/)
2. 进入 **Pages** → **创建项目** → **连接到 Git**
3. 选择你的仓库
4. 构建设置：
   - **构建命令**: `npm run build`
   - **构建输出目录**: `dist`
5. 点击 **保存并部署**

每次推送代码到仓库，Cloudflare Pages 会自动重新构建和部署。

## Vercel

1. 登录 [Vercel](https://vercel.com/)
2. 点击 **Add New** → **Project**
3. 导入你的 Git 仓库
4. 框架选择 **Astro**（Vercel 会自动识别）
5. 点击 **Deploy**

## Netlify

1. 登录 [Netlify](https://app.netlify.com/)
2. 点击 **Add new site** → **Import an existing project**
3. 连接你的 Git 仓库
4. 构建设置：
   - **Build command**: `npm run build`
   - **Publish directory**: `dist`
5. 点击 **Deploy site**

## GitHub Pages

1. 在仓库设置中启用 GitHub Pages
2. 在项目根目录创建 `.github/workflows/deploy.yml`：

```yaml
name: Deploy to GitHub Pages
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 22
      - run: npm ci
      - run: npm run build
      - uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```

3. 推送代码到 `main` 分支，GitHub Actions 会自动构建并部署到 GitHub Pages。

## 自有服务器（Nginx）

```bash
# 构建
npm run build

# 将 dist/ 复制到服务器
scp -r dist/* user@your-server:/var/www/my-site/
```

Nginx 配置：

```nginx
server {
    listen 80;
    server_name your-domain.com;
    root /var/www/my-site;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

## 部署前检查清单

- [ ] 修改了 `site.config.ts` 中的所有示例数据
- [ ] 替换了 `public/images/` 中的示例图片
- [ ] 文章 frontmatter 的 `date` 字段加了引号（`"2026-07-01"`）
- [ ] 运行 `npm run build` 没有报错
- [ ] 运行 `npm run preview` 预览效果正常
