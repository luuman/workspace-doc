# Submodule 管理

## 当前注册的 Submodule

| 路径 | 远端仓库 | 分支 |
|------|----------|------|
| content/moltbot | git@github-luuman:luuman/moltbot-doc.git | docs |
| content/claude | git@github-luuman:luuman/claude-doc.git | docs |
| content/electron | git@github-luuman:luuman/electron-doc.git | docs |
| content/tauri | git@github-luuman:luuman/tauri-doc.git | docs |
| content/matrx | git@github-luuman:luuman/matrx-doc.git | docs |
| content/brainboard-doc | git@github-luuman:luuman/brainboard-doc.git | docs |
| content/synology | git@github-luuman:luuman/synology-doc.git | docs |

> SSH remote 使用本机 `git@github-luuman:` 别名。CI 中通过 `git config url.insteadOf` 重写为 HTTPS token URL，本地开发不受影响。

## 常用操作

```bash
# 初始化并拉取所有 submodule
git submodule update --init --remote

# 拉取单个 submodule 到分支最新
git submodule update --init --remote --depth 1 content/<site>

# 新增 submodule
git submodule add -b docs git@github-luuman:<owner>/<repo>.git content/<site>

# 移动 submodule 路径
git mv <old-path> <new-path>
# 同步 .git/config 中的 section 名称
git config --file .git/config --rename-section \
  'submodule.<old-name>' 'submodule.<new-name>'
```

## .gitmodules 格式

```ini
[submodule "content/<site>"]
    path = content/<site>
    url = git@github-luuman:luuman/<repo>.git
    branch = docs
    # shallow = true  # 可选，加速 CI 拉取
```
