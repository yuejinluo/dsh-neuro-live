# 发布到 GitHub

这个目录已经是一个完整的开源项目，但本机当前没有安装 Git，所以没有自动初始化仓库或推送。

## 方式 A：GitHub 网页上传

1. 在 GitHub 新建公开仓库，例如 `dsh-neuro-live`。
2. 不要勾选自动生成 README / .gitignore / LICENSE。
3. 把本目录全部文件上传。
4. 确认 `LICENSE`、`THIRD_PARTY_NOTICES.md` 和 `plugin/ATTRIBUTION.md` 都在。

## 方式 B：安装 Git 后推送

```powershell
# 安装 Git 后，在项目目录执行
git init -b main
git add .
git commit -m "feat: open-source NEURO // LIVE"
git remote add origin https://github.com/<your-name>/dsh-neuro-live.git
git push -u origin main
```

如果使用 GitHub CLI：

```powershell
gh repo create dsh-neuro-live --public --source=. --remote=origin --push
```

## 发布前检查

- [ ] 没有 `node_modules`、`.dsh` profile、token、日志或绝对用户路径
- [ ] `LICENSE` 是 MIT，`THIRD_PARTY_NOTICES.md` 保留脑网格 CC BY-SA 4.0 署名
- [ ] README 截图不含个人信息
- [ ] `plugin/lib/client.js` 是执行 `cd plugin && node build.mjs` 后的最新产物
- [ ] 仓库描述/标签包含 `threejs`、`dsh`、`ai-agent`、`visualization`
- [ ] 不声称插件能读取模型权重/注意力/单个神经元

## 建议的仓库描述

> A floating 3D brain for DSH Desktop that maps real agent runtime events — reasoning stream, tool calls, subagents, steps and token usage — onto an anatomical cortical field. Includes a standalone Three.js demo.

## 建议的 Topics

`threejs` `dsh` `deepseek-harness` `ai-agent` `visualization` `webgl` `brain` `plugin`
