# 第三方组件与资产署名

本仓库的原创代码采用 MIT（见 `LICENSE`）。但仓库中包含第三方 3D 数据和第三方库，尤其脑网格是 **CC BY-SA 4.0** 许可的衍生作品。再分发时请保留以下署名与许可信息。

## 1. 3D 脑网格

以下两个文件包含同一来源的量化脑网格衍生数据：

- `standalone/brain-asset.js`
- `plugin/src/brain-cortex.js`（以及 `plugin/lib/client.js` 中内联的网格）

来源：

- **BrainProject** — 3D brain atlas by Itay Inbar  
  https://github.com/itayinbarr/brainproject  
  源文件：`brain-atlas/models/brain.glb`
- **Z-Anatomy**  
  https://www.z-anatomy.com/  
  https://github.com/Z-Anatomy
- **BodyParts3D** — Database Center for Life Science (DBCLS)  
  https://lifesciencedb.jp/bp3d/

BrainProject 的 3D 解剖资产基于 Z-Anatomy，Z-Anatomy 又建立在 BodyParts3D / DBCLS 数据之上。原始 3D 资产采用：

**Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)**  
https://creativecommons.org/licenses/by-sa/4.0/

### 本项目所做的修改

1. 选取大脑皮层、小脑、脑干相关网格；
2. 解码 Draco 压缩几何；
3. 合并、清理无关的血管、神经、深部核团和骨骼；
4. 重新居中、统一缩放、旋转到本项目的坐标系统；
5. 将顶点位置量化为 16 位整数，索引使用 Uint16，并打包为网页可直接加载的 JavaScript 数据；
6. 在插件版本中进一步提取 cortex-only 网格，并合并左右半球。

上述衍生脑网格数据继续以 **CC BY-SA 4.0** 提供。再分发 `brain-asset.js`、`brain-cortex.js`、`lib/client.js` 或它们的修改版本时，请保留本署名、许可声明与许可链接。

## 2. Three.js

- 版本：r155
- 许可：MIT
- 网站：https://threejs.org/
- 版权：Copyright © 2010-2023 three.js authors
- 文件：`standalone/vendor/three.min.js`、`plugin/vendor/three.min.js`，并内联进 `plugin/lib/client.js`

## 3. React（仅预览页）

- 版本：18.3.1
- 许可：MIT
- 网站：https://react.dev/
- 使用方式：`plugin/preview/panel.html` 通过 unpkg CDN 加载，**不随插件打包**；正式插件由 DSH 运行时提供 React。

## 4. DSH Desktop

本仓库只包含 DSH 客户端插件代码与视觉资产，不包含 DSH Desktop 本体。DSH Desktop 是独立产品，本项目与 DeepSeek 官方无隶属关系。

## 5. 字体

界面使用系统字体栈（Bahnschrift / Segoe UI / Cascadia Mono / Consolas 等），不包含外部字体文件。

## 6. 医疗免责声明

3D 脑网格与所有视觉映射仅用于视觉展示、交互演示和技术研究，**不适用于临床诊断、手术规划、医学训练或任何医疗用途**。源解剖数据的准确性请以原始项目说明为准。

## 7. 打包产物说明

`plugin/lib/client.js` 是以下内容的组合：

- 本项目 MIT 代码（`src/client.js`、`src/engine.js`）
- Three.js（MIT）
- CC BY-SA 4.0 的量化脑网格
- 由 DSH 在运行时提供的 React（不打包）

因此该文件不能简单地整体视为单一 MIT 作品；再分发时请同时遵守 MIT 与 CC BY-SA 4.0。
