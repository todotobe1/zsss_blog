# 长期记忆

## Hugo 博客项目

- **路径**：`/Users/zsss/Documents/project/zsss_blog`
- **Hugo 版本**：v0.147.4+extended
- **主题**：PaperMod（git submodule，路径 themes/PaperMod）
- **配置文件**：hugo.toml，使用 `[pagination] pagerSize = 10`（注意：旧版 `paginate` 已废弃）
- **内容结构**：content/posts/（文章）、content/about/（关于）、content/search.md（搜索）
- **本地预览**：`hugo server -D`，访问 http://localhost:1313
- **文章模板**：`hugo new posts/文章名.md`（使用 archetypes/posts.md）
- **部署建议**：Cloudflare Pages 或 GitHub Pages（静态托管）
- **功能**：全文搜索（JSON 输出）、代码高亮（monokai）、标签/分类/系列、暗色模式
