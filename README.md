# 紫薯蜀黍的技术博客

基于 [Hugo](https://gohugo.io/) + [PaperMod](https://github.com/adityatelange/hugo-PaperMod) 主题构建的个人技术博客。

## 关注领域

- 🤖 **AI Coding**：AI 编程工具的使用与探索
- 🦾 **具身智能**：机器人与 AI 的交叉前沿
- 🛠️ **独立开发**：用 AI 工具构建有用的东西

## 技术栈

- **框架**：Hugo（静态站点生成器）
- **主题**：PaperMod
- **部署**：GitHub Pages
- **评论**：Giscus（基于 GitHub Discussions）

## 快速开始

### 本地预览

```bash
# 安装 Hugo（macOS）
brew install hugo

# 克隆项目
git clone https://github.com/todotobe1/zsss_blog.git
cd zsss_blog

# 安装主题 submodule
git submodule update --init --recursive

# 启动本地服务器
hugo server
```

访问 http://localhost:1313 查看博客。

### 新建文章

```bash
hugo new posts/my-new-post.md
```

然后在 `content/posts/my-new-post.md` 编写内容。

## 文章结构

```yaml
---
title: "文章标题"
date: 2026-04-28T13:00:00+08:00
draft: false
tags: ["标签1", "标签2"]
categories: ["分类"]
series: ["系列名称"]
summary: "文章摘要"
---
```

## 部署

博客通过 GitHub Actions 自动部署到 GitHub Pages。每次推送到 `main` 分支都会触发构建。

## 目录结构

```
zsss_blog/
├── archetypes/      # 文章模板
├── assets/         # 资源文件
├── config/        # 站点配置
│   └── _default/
│       ├── hugo.toml      # 主配置
│       ├── params.toml    # 主题参数
│       └── menu.toml     # 菜单配置
├── content/       # 内容目录
│   ├── posts/     # 博客文章
│   └── about/     # 关于页面
├── layouts/       # 自定义布局
├── public/        # 生成的静态文件
├── static/        # 静态资源
├── themes/        # 主题（submodule）
├── hugo.toml      # Hugo 配置
└── README.md
```

## 写作规范

- 使用 Markdown 编写
- 中英文混排时中文与英文之间加空格
- 代码块注明语言
- 文章末尾添加分隔线 `---`
- 使用中文标点符号

## License

本博客内容采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) 许可证。
