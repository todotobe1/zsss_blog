---
title: "Hello World：用 Hugo 搭建博客"
date: 2026-04-28T13:00:00+08:00
draft: false
tags: ["Hugo", "博客", "建站"]
categories: ["技术"]
series: ["博客搭建"]
summary: "用 Hugo 搭建个人博客的起点——记录这个站点是如何诞生的。"
cover:
  image: ""
  alt: ""
  relative: false
  hidden: true
---

## 为什么选 Hugo

- **极快的构建速度**：毫秒级生成静态页面
- **纯静态**：托管成本几乎为零，可以部署在 GitHub Pages / Cloudflare Pages / Vercel
- **Markdown 驱动**：专注写作，无需折腾 CMS
- **PaperMod 主题**：简洁、响应式、支持暗色模式

## 快速上手

```bash
# 安装 Hugo（macOS）
brew install hugo

# 新建站点
hugo new site my-blog

# 安装主题
git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod

# 启动本地预览
hugo server -D
```

## 目录结构

```
zsss_blog/
├── content/
│   ├── posts/        ← 博客文章
│   └── about/        ← 关于页面
├── static/           ← 静态资源（图片、favicon 等）
├── themes/PaperMod/  ← 主题
└── hugo.toml         ← 站点配置
```

## 下一步

- [ ] 配置自定义域名
- [ ] 接入 Giscus 评论系统
- [ ] 添加 Google Analytics
- [ ] 部署到 Cloudflare Pages

---

> 好的开始是成功的一半。🐾
