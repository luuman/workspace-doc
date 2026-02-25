# CI 部署

部署由 `.github/workflows/deploy.yml` 驱动。

## 触发方式

| 触发 | 行为 |
|------|------|
| push 到 main | 自动检测变更范围，按需部署 |
| workflow_dispatch | 手动选择部署指定站点或全部 |
| 内容仓库 push | 通过 `trigger-deploy.yml` 调用 workflow_dispatch |

## 变更检测逻辑

```
src/ / package.json / sites.json / .github/ / .gitmodules 有变更
    → 全量部署所有站点

configs/<site> 或 content/<site> 有变更
    → 只部署该站点
```

## 部署步骤

1. Checkout docs-workspace
2. 从 `sites.json` 读取站点配置
3. SSH URL 重写（`git@github-luuman:` → HTTPS token URL）
4. 拉取对应 submodule（`--depth 1`）
5. `pnpm install` + `pnpm build:<site>`
6. Force push 构建产物到目标仓库的 `gh-pages` 分支

## 所需配置

**本仓库 Secrets：**
- `DEPLOY_TOKEN`：需对所有部署目标仓库有写权限，且需有本仓库的 `workflow` 写权限

**内容仓库 Secrets：**
- `DEPLOY_TOKEN`：需有本仓库的 `workflow` 写权限（用于触发部署）

## 自动触发链路

内容仓库的 `.github/workflows/trigger-deploy.yml` 在分支 push 后，通过 workflow_dispatch API 调用本仓库的 `deploy.yml`，传入对应的 `site` 参数，实现内容更新即自动发布。

```yaml
# 内容仓库中的 trigger-deploy.yml 示例
on:
  push:
    branches: [docs]
jobs:
  trigger:
    runs-on: ubuntu-latest
    steps:
      - name: Trigger docs-workspace deploy
        run: |
          curl -X POST \
            -H "Authorization: token ${{ secrets.DEPLOY_TOKEN }}" \
            -H "Accept: application/vnd.github.v3+json" \
            https://api.github.com/repos/luuman/docs-workspace/actions/workflows/deploy.yml/dispatches \
            -d '{"ref":"main","inputs":{"site":"<site>"}}'
```
