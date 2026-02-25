# docs-workspace

多站点 Docusaurus v3 文档构建中心。本仓库不存放文档内容，只负责构建和部署。

## 文档目录

- [站点注册表](./sites.md) — 所有站点的配置清单
- [开发指南](./development.md) — 本地预览、构建命令
- [CI 部署](./deployment.md) — GitHub Actions 部署流程
- [Submodule 管理](./submodules.md) — submodule 注册与更新
- [新增站点](./add-site.md) — 完整的新站点接入流程

## 架构概览

```
内容仓库（各自独立 git 仓库）
    ↓ git submodule
docs-workspace/content/<site>/
    ↓ Docusaurus 构建
build/<site>/
    ↓ force push
目标仓库 gh-pages 分支（GitHub Pages）
```

内容仓库推送后，通过 `trigger-deploy.yml` 自动调用本仓库的 `deploy.yml`，实现内容更新即自动发布。

## 技术栈

| 组件 | 版本 |
|------|------|
| Docusaurus | 3.9.2 |
| React | 19.2.1 |
| TypeScript | ~5.9.3 |
| 搜索 | @easyops-cn/docusaurus-search-local |
| 图表 | @docusaurus/theme-mermaid |
| 包管理 | pnpm |
| Node.js | ≥ 18.0 |
