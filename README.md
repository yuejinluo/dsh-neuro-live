# NEURO // LIVE

[English](./README.en.md) · [在线 Demo](https://slianhamess.github.io/dsh-neuro-live/) · [v0.2.0 Release](https://github.com/Slianhamess/dsh-neuro-live/releases/tag/v0.2.0)

把 AI Agent 的**真实运行时轨迹**映射到一颗 3D 半透明人类大脑上的开源项目。

这个仓库包含两个交付物：

| 目录 | 是什么 | 是否读取真实数据 |
| --- | --- | --- |
| [`plugin/`](./plugin/) | DSH Desktop 悬浮窗插件：实时读取推理流、工具调用、子 Agent、步骤与 token 用量 | ✅ 真实 DSH 运行时事件 |
| [`standalone/`](./standalone/) | 纯前端 Three.js 演示页：可调参数的 3D 脑与思考动画 | ❌ 预设演示，不连接模型 |

![DSH 悬浮脑插件](docs/panel-final.png)

![独立 Three.js 演示页](docs/standalone.png)

> 两个版本共享解剖脑网格与视觉语言；`plugin/` 是真正接入 DSH 的版本，`standalone/` 是不需要 DSH 的视觉研究/展示页。

---

## 功能

### DSH 插件

- **真实运行时事件图**：推理片段、工具调用、子 Agent、步骤和 token 用量变成脑内事件节点。
- **覆盖全脑的密集神经场**：约 374 个基础神经元（160 个内部节点 + 190 个皮质表面节点 + 24 个小脑节点），500 余条连接。
- **流星信号**：信号沿连接高速穿行，带 12 段渐隐尾迹，而不是单个移动光点。
- **皮层透明度 48%**：半透明皮质，保留解剖结构的同时可以看到内部神经场。
- **功能区映射**：读取类工具偏颞叶、写入类偏前额叶、终端偏运动区、搜索偏顶叶，真实子 Agent 调用走跨半球连接。
- **实时仪表**：当前动作、推理尾部、工具链、步骤、工具数、真实子 Agent 调用数、tokens、字符数、时长。
- **可核验的父/子 Agent**：父 Agent 就是当前 DSH 会话，页脚显示 `sessionId` 后 8 位；子 Agent 节点只来自轨迹中真实出现的 `subagent` / `subagent_fork` 工具调用。没有调用就没有节点。
- **悬浮窗交互**：拖动、收起为悬浮球、关闭到下一轮；默认常驻显示，空闲时也保持 60fps 旋转，不会自动收起。
- **本地资源**：Three.js 与脑网格都已打包，运行时不需要网络。

### 独立演示页

- 真实解剖脑网格、半透明皮层、小脑与脑干
- 140–520 个可调神经元、连接网络、跨半球连接、移动脉冲
- 自动旋转、缩放、悬停查看脑区、点击注入信号
- Agent 思考流程演示（解析输入 → 记忆 → 关联 → 推演 → 校验 → 输出）
- 参数面板：旋转速度、神经元密度、脉冲频率、皮层透明度、扫描速度、图层开关
- 桌面/平板/手机响应式 HUD

---

## 真实数据边界（重要）

语言模型内部的**权重、注意力矩阵和单个神经元**不会通过 DSH 的公开接口暴露。

因此：

- 插件展示的是 **DSH 运行时事件的神经场隐喻**：推理流、工具调用、子 Agent、步骤、token。
- 它**不伪造**模型内部激活，也不声称自己能看到“真正的神经元放电”。
- 暗色基础神经场是视觉基质，不是实时事件；真实事件节点会在轨迹到来时亮起。
- 父 Agent 不是神秘实体，就是当前 DSH 会话；子 Agent 必须对应真实 `callId`，否则不绘制。
- 独立演示页完全不连接模型，它的“思考链路”是预设演示。

`plugin/` 内的 `i` 信息浮层会再次说明这一点。

---

## 快速开始

### 1. DSH Desktop 插件

适用于 **DSH Desktop 2.0.x**，需要启用客户端插件支持。

**安装到 profile：**

```powershell
$src = ".\plugin"
$dst = "$env:USERPROFILE\.dsh\profiles\desktop\node_modules\dsh-neuro-live"

if (Test-Path $dst) { Remove-Item $dst -Recurse -Force }
New-Item -ItemType Directory -Force $dst | Out-Null
Copy-Item "$src\*" $dst -Recurse -Force
```

**在 profile 覆盖层中加入插件：**

编辑 `~/.dsh/profiles/desktop/cordis.patch.yml`，在末尾追加：

```yaml
- insert:
    - id: dsh-neuro-live
      name: 'dsh-neuro-live'
```

**完全退出并重新打开 DSH Desktop**（托盘也要退出）。客户端 bundle 在应用启动时扫描；当前正在运行的窗口不会自动加载新插件。会话不会丢失。

重启后，在任意对话里发一条消息，右下角会出现悬浮脑；空闲时收成悬浮球。

> 详细说明、构建方式、卸载步骤见 [`plugin/README.md`](./plugin/README.md)。

### 2. 独立 Three.js 演示页

```powershell
cd standalone
python -m http.server 8766
# 浏览器打开 http://127.0.0.1:8766/
```

也可以直接双击 `standalone/index.html`；页面不依赖服务器或网络。

在线演示：**https://slianhamess.github.io/dsh-neuro-live/**

---

## 开发

### 插件构建

```powershell
cd plugin
node build.mjs
node --check lib/client.js
```

构建会生成 `lib/client.js`：

- Three.js r155
- 量化皮质网格
- `src/engine.js` 3D 脑场引擎
- `src/client.js` React 悬浮窗客户端

预览（React 由 CDN 提供，仅预览用）：

```powershell
python -m http.server 8766
# http://127.0.0.1:8766/plugin/preview/panel.html
```

### 独立页

无构建步骤。直接修改 `standalone/index.html`、`styles.css`、`main.js`。

### 验证清单

- [ ] 悬浮窗能拖动、收起、关闭、恢复
- [ ] 空闲时不自动收起，并保持 60fps 旋转
- [ ] 推理流、工具调用、token 数字会更新
- [ ] 无 `subagent` 调用时 `SUBAGENT` 为 0，且没有紫色子 Agent 节点
- [ ] 页面隐藏时 RAF 暂停，重新可见后恢复
- [ ] 不提交任何 profile 配置、token、日志或用户路径

---

## 交互

| 操作 | 插件 | 独立页 |
| --- | --- | --- |
| 拖动 | 移动悬浮窗 | 旋转视角 |
| 滚轮 / 双指 | — | 缩放 |
| 悬停节点 | 查看真实事件 | 查看脑区信息 |
| 点击 | 收起 / 关闭等按钮 | 注入聚焦信号 |
| `Space` | — | 演示一次思考 |
| `A` / `R` / `H` | — | 自动旋转 / 重置 / 帮助 |

插件窗口位置保存在 `localStorage` 的 `dsh-neuro-live.settings.v1`。

---

## 目录结构

```text
dsh-neuro-live/
├── plugin/                  # DSH Desktop 插件
│   ├── src/
│   │   ├── client.js        # slot 注册、轨迹提取、React 悬浮窗
│   │   ├── engine.js        # Three.js 脑场与事件图
│   │   └── brain-cortex.js  # 量化皮质网格（CC BY-SA 4.0）
│   ├── lib/client.js        # 构建产物
│   ├── preview/             # 独立预览与 mock 数据
│   ├── vendor/three.min.js  # Three.js r155
│   ├── build.mjs
│   ├── cordis.patch.yml
│   └── README.md
├── standalone/              # 纯前端 Three.js 演示页
│   ├── index.html
│   ├── styles.css
│   ├── main.js
│   ├── brain-asset.js       # 量化脑网格（CC BY-SA 4.0）
│   ├── vendor/three.min.js
│   └── README.md
├── docs/                    # README 截图
├── .github/workflows/       # GitHub Pages 部署
├── THIRD_PARTY_NOTICES.md
├── CONTRIBUTING.md
├── CHANGELOG.md
├── PUBLISHING.md
├── README.en.md             # English README
├── LICENSE                  # MIT（代码）
└── README.md
```

---

## 已知限制

- 需要 WebGL；极旧显卡或禁用硬件加速的环境可能无法显示。
- 插件客户端 bundle 约 1.5 MB，首次加载有一次解析开销。
- 插件只作用于当前会话的对话视图，不读取其他会话的全局状态。
- 长会话中真实事件节点只保留最近 28 个。
- 独立演示页不连接任何模型，所有“思考”是预设演示。
- 脑网格只用于视觉展示，**不适用于临床诊断、手术规划或任何医疗用途**。

---

## 贡献

欢迎 issue 和 PR。请先阅读 [`CONTRIBUTING.md`](./CONTRIBUTING.md)。

特别欢迎：

- 更好的运行时事件映射（而不是伪造数据）
- 性能优化、低配 WebGL 兼容
- 无障碍与键盘操作
- 脑网格/材质/光照改进
- 文档、截图、英文翻译

---

## 许可与署名

- **项目代码**（HTML/CSS/JS/React 客户端）：MIT，见 [`LICENSE`](./LICENSE)。
- **3D 解剖脑网格衍生数据**（`brain-asset.js`、`src/brain-cortex.js` 及打包产物中的网格）：**CC BY-SA 4.0**。来源与修改说明见 [`THIRD_PARTY_NOTICES.md`](./THIRD_PARTY_NOTICES.md)。
- **Three.js r155**：MIT。
- 预览页通过 CDN 使用 React 18.3.1（MIT），正式插件由 DSH 提供 React。

> `plugin/lib/client.js` 是把 MIT 代码、Three.js 和 CC BY-SA 4.0 脑网格打包在一起的产物。再分发时请同时遵守这些许可，并保留署名。

本项目与 DeepSeek 官方无隶属关系；DSH Desktop 是独立产品，本仓库只包含插件代码。

---

## 状态

- 当前版本：**v0.2.0**
- 插件 UI 与 3D 引擎已在独立预览中验证
- DSH profile 组合配置与 host/client 模块解析已验证
- 实机挂载需要重启 DSH；不同 DSH 版本的 slot/API 可能存在差异

如果这个项目对你有帮助，欢迎 star、提 issue，或者把它接到你自己的 Agent 运行时。
