# 技术 SEO 配置文件

将这些文件放入网站根目录 (public/)，替换 YOUR_SITE 为你的域名。

---

## 1. robots.txt

```txt
User-agent: *
Allow: /
Sitemap: https://YOUR_SITE.com/sitemap-index.xml

# 禁止爬取后台路径
Disallow: /admin/
Disallow: /api/
Disallow: /_astro/

# 爬取延迟（对服务器友好）
Crawl-delay: 10
```

## 2. Sitemap 配置

Astro 项目在 `astro.config.mjs` 中启用 sitemap：

```js
import sitemap from '@astrojs/sitemap'

export default defineConfig({
  site: 'https://YOUR_SITE.com',
  integrations: [sitemap()],
})
```

安装: `npx astro add sitemap`

## 3. 结构化数据 (JSON-LD)

在页面 `<head>` 中添加：

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "BlogPosting",
  "headline": "文章标题",
  "description": "文章描述",
  "author": {
    "@type": "Person",
    "name": "作者名"
  },
  "datePublished": "2026-05-10",
  "dateModified": "2026-05-10",
  "mainEntityOfPage": {
    "@type": "WebPage",
    "@id": "https://YOUR_SITE.com/blog/post-slug"
  }
}
</script>
```

其他常用 Schema 类型：
- 首页 → `WebSite` + `Organization`
- 文章 → `BlogPosting` 或 `Article`
- 教程 → `HowTo`
- 常见问题 → `FAQPage`
- 商品 → `Product`
- 个人 → `Person`

## 4. Open Graph / Twitter Card

```html
<!-- Facebook / LinkedIn -->
<meta property="og:title" content="页面标题">
<meta property="og:description" content="页面描述">
<meta property="og:image" content="https://YOUR_SITE.com/og-image.png">
<meta property="og:url" content="https://YOUR_SITE.com/page-url">
<meta property="og:type" content="website">

<!-- Twitter -->
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="页面标题">
<meta name="twitter:description" content="页面描述">
<meta name="twitter:image" content="https://YOUR_SITE.com/og-image.png">
```

## 5. 性能优化清单

- [ ] 图片使用 WebP/AVIF 格式
- [ ] 启用懒加载: `<img loading="lazy">`
- [ ] 压缩 CSS/JS (Astro 自动)
- [ ] 添加 preload 关键资源: `<link rel="preload">`
- [ ] 移除阻塞渲染的 JS
- [ ] 使用 CDN (Vercel 自动)
- [ ] 启用 Brotli 压缩 (Vercel 自动)
- [ ] 减少第三方脚本

## 6. HTML Meta 基础模板

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>页面标题 | 网站名称</title>
  <meta name="description" content="页面描述，含关键词，不超过160字">
  <meta name="keywords" content="关键词1, 关键词2, 关键词3">
  <link rel="canonical" href="https://YOUR_SITE.com/current-url">
  <meta name="robots" content="index, follow">
</head>
```

## 7. 验证站点所有权

```html
<!-- Google Search Console -->
<meta name="google-site-verification" content="YOUR_VERIFICATION_CODE">

<!-- Bing Webmaster Tools -->
<meta name="msvalidate.01" content="YOUR_VERIFICATION_CODE">
```
