# Contributing

感谢你愿意改进 NEURO // LIVE。

## 开发环境

- Node.js 18+
- 现代 Chromium / Firefox（WebGL）
- DSH Desktop 2.0.x（仅在测试真实插件时需要）
- Python 3（仅用于本地静态预览服务器）

仓库不需要 `npm install`；插件构建只使用 Node 内置模块。

## 构建与预览

```powershell
# 插件
cd plugin
node build.mjs
node --check lib/client.js

# 预览（在仓库根目录或 plugin 目录启动均可）
python -m http.server 8766
# http://127.0.0.1:8766/plugin/preview/panel.html
```

`plugin/preview/panel.html` 使用 CDN React，只有预览需要联网；正式插件由 DSH 提供 React。

## 修改原则

1. **不伪造数据**
   - 不要把随机动画、预设文本或 mock 的父/子 Agent 说成真实运行时数据。
   - 基础神经场可以存在，但必须在 UI/README 中声明它是视觉基质。
   - 子 Agent 节点只能来自真实的 `subagent` / `subagent_fork` 工具调用与 `callId`。
2. **保持可验证**
   - 新增运行时字段时，在 `plugin/src/client.js` 的 `extractLive()` 中集中处理，并说明数据来源。
   - 不要默默改变“父 Agent = 当前会话”的定义。
3. **许可证兼容**
   - 代码贡献按 MIT。
   - 脑网格或其它 CC BY-SA 4.0 数据的衍生修改继续按 CC BY-SA 4.0。
   - 新增第三方资产时，更新 `THIRD_PARTY_NOTICES.md`。
4. **不要提交隐私数据**
   - 不要提交 `.dsh` profile、`cordis.patch.yml` 的个人配置、token、日志、绝对用户路径或聊天记录。
   - 预览截图请使用 mock 数据，并清除个人信息。

## 测试清单

- [ ] `node build.mjs` 成功
- [ ] `node --check lib/client.js` 成功
- [ ] 悬浮窗可拖动、固定、收起、关闭、恢复
- [ ] 运行中自动展开，空闲自动收起
- [ ] 推理/工具/token 数据更新
- [ ] 没有子 Agent 调用时 `SUBAGENT == 0` 且没有紫色节点
- [ ] 页面隐藏时 RAF 停止，恢复时继续
- [ ] 移动端/窄屏不溢出
- [ ] 没有新增外部网络依赖（预览 CDN 除外）

## Pull Request

- 一个 PR 聚焦一个主题。
- 说明改动、验证方式、是否影响许可/署名。
- UI 改动请附截图或短视频。
- 不要求更新版本号，维护者会在发布时处理。

## 行为准则

保持技术讨论友善、具体、可复现。不要提交他人的隐私数据、未授权资产或误导性的“真实模型读数”。
