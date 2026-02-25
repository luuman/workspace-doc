# 开发指南

## 前置条件

- Node.js ≥ 18.0
- pnpm
- 需要访问的 submodule 已 checkout（`content/<site>/docs/` 目录存在）

## 安装依赖

```bash
pnpm install
```

## 本地预览

```bash
pnpm start:moltbot      # http://localhost:3001
pnpm start:claude       # http://localhost:3002
pnpm start:electron     # http://localhost:3003
pnpm start:tauri        # http://localhost:3004
pnpm start:matrx        # http://localhost:3005
```

## 生产构建

输出到 `build/<site>/`：

```bash
pnpm build:moltbot
pnpm build:claude
pnpm build:electron
pnpm build:tauri
pnpm build:matrx
```

## 其他命令

```bash
# 清理 Docusaurus 缓存
pnpm clear

# 拉取/更新所有 submodule 到分支最新
git submodule update --init --remote

# 仅更新单个 submodule
git submodule update --init --remote --depth 1 content/<site>
```

## 目录结构

```
docs-workspace/
├── sites.json              # 站点注册表
├── configs/                # 各站点 Docusaurus 配置
├── content/                # git submodule（文档内容）
├── src/
│   ├── css/                # 全局样式
│   ├── theme/              # swizzle 组件（SearchBar, Navbar, TOC 等）
│   ├── plugins/
│   │   └── rehype-fix-style.js
│   ├── prism-themes/       # 代码高亮主题（winter-light / winter-dark）
│   └── components/         # 共享 MDX 组件
├── sidebars/
│   └── default.ts          # 自动生成 sidebar 的 fallback
└── build/                  # 构建输出（gitignored）
```
