# 站点注册表

`sites.json` 是所有站点的单一事实来源。

## 当前站点

| 站点 | 内容仓库 | 内容分支 | submodule 路径 | 部署仓库 |
|------|----------|----------|----------------|----------|
| moltbot | luuman/moltbot | main | content/moltbot | luuman/moltbot-doc |
| claude | luuman/claude-doc | docs | content/claude | luuman/claude-doc |
| electron | luuman/electron-doc | docs | content/electron | luuman/electron-doc |
| tauri | luuman/tauri-doc | docs | content/tauri | luuman/tauri-doc |
| matrx | luuman/matrx-doc | docs | content/matrx | luuman/matrx-doc |

> `content/brainboard-doc`、`content/synology` 已注册 submodule，尚未添加到 `sites.json`。

## sites.json 字段说明

```json
{
  "<site>": {
    "contentRepo": "owner/repo",       // 内容来源仓库
    "contentBranch": "docs",           // 拉取的分支
    "config": "configs/<site>.config.ts", // Docusaurus 配置文件路径
    "deployRepo": "owner/repo",        // 部署目标仓库（gh-pages 分支）
    "submodule": "content/<site>"      // 本地 submodule 路径
  }
}
```

## 配置文件约定（configs/\<site\>.config.ts）

- **docs 路径**：`./content/<site>/docs`
- **静态资源**：`staticDirectories` 同时包含 `"static"` 和内容仓库静态目录
- **Sidebar**：优先加载 `./content/<site>/sidebars.ts`，不存在则 fallback 到 `./sidebars/default.ts`
- **rehype 插件**：启用 `src/plugins/rehype-fix-style`（将 HTML `style` 字符串转为 React 对象，兼容 GitBook 导出文档）
- **语言**：`defaultLocale: "zh"`，暗色主题
