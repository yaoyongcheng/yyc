# 工地数字地图管理云平台 — 外网部署说明

本项目是一个纯静态 HTML 单文件页面，已配置为可直接部署到 Vercel / Netlify。

## 🚀 一键部署到 Vercel

### 方式 A：Vercel CLI（推荐）

```bash
cd "C:/Users/11423/Desktop/site-deploy"
npm install -g vercel
vercel login              # 浏览器登录 Vercel
vercel --prod             # 部署到生产环境
```

部署成功后，Vercel 会输出类似：
```
✅ Production: https://construction-site-map-xxx.vercel.app [copied to clipboard]
```

### 方式 B：网页拖拽部署（无需 CLI）

1. 打开 https://app.netlify.com/drop
2. 把 `C:/Users/11423/Desktop/site-deploy` 文件夹拖到网页
3. 30 秒后获得 URL

### 方式 C：GitHub + Vercel 自动部署

```bash
git init
git add .
git commit -m "init"
# 在 GitHub 创建空仓库后：
git remote add origin https://github.com/你的用户名/your-repo.git
git push -u origin main
# 然后在 vercel.com/new 导入这个仓库即可
```

## 🔒 安全说明

- 项目纯前端、无后端、无密钥，已扫描确认无敏感内容
- Vercel/Netlify 自动 HTTPS
- 默认公开访问，如需鉴权见 Vercel Password Protection（Pro 计划）

## 🌐 中国大陆访问优化

页面依赖 Google Fonts（fonts.googleapis.com），在中国大陆可能加载较慢。
如需优化，可替换为国内 CDN（如 https://fonts.font.im 或 https://fonts.loli.net）。