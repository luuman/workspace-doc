# 新增站点

## 完整流程

### 1. 注册到 sites.json

```json
"<site>": {
  "contentRepo": "luuman/<repo>",
  "contentBranch": "docs",
  "config": "configs/<site>.config.ts",
  "deployRepo": "luuman/<deploy-repo>",
  "submodule": "content/<site>"
}
```

### 2. 添加 submodule

```bash
git submodule add -b docs git@github-luuman:luuman/<repo>.git content/<site>
```

在 `.gitmodules` 中确认格式：

```ini
[submodule "content/<site>"]
    path = content/<site>
    url = git@github-luuman:luuman/<repo>.git
    branch = docs
```

### 3. 新建配置文件

参考 `configs/moltbot.config.ts`，在 `configs/<site>.config.ts` 中设置：

```ts
const config: Config = {
  title: '<站点名>',
  // ...
  presets: [['classic', {
    docs: {
      path: './content/<site>/docs',
      routeBasePath: '/',
      sidebarPath: fs.existsSync('./content/<site>/sidebars.ts')
        ? './content/<site>/sidebars.ts'
        : './sidebars/default.ts',
    },
  }]],
  staticDirectories: ['static', './content/<site>/static'],
  // ...
};
```

### 4. 添加 npm scripts

在 `package.json` 中追加（端口递增）：

```json
"start:<site>": "docusaurus start --config configs/<site>.config.ts --port 300X",
"build:<site>": "docusaurus build --config configs/<site>.config.ts --out-dir build/<site>"
```

### 5. 内容仓库配置自动触发

在内容仓库根目录添加 `.github/workflows/trigger-deploy.yml`，参考现有内容仓库，指定正确的监听分支和 `site` 名称，并配置 `DEPLOY_TOKEN` secret。

## 注意事项

- 本地开发时需先 checkout submodule（`content/<site>/docs/` 目录存在）
- 修改 `src/` 下共享资源会触发 CI 全量部署所有站点
- `onBrokenLinks` 和 `onBrokenAnchors` 均设为 `warn`，不阻断构建
