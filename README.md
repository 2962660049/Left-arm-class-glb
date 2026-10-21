# 左前臂与左手 · 解剖模型（three.js 用）

课堂用模型包：把 BodyParts3D 完整的左前臂 + 左手合并成一个 glb，直接用 three.js 加载即可，不需要 Blender。

!\[预览](preview.png)

## 文件

|文件|大小|内容|
|-|-|-|
|`glb/hand\_elbow\_left.glb`|4.74 MB|123 个部件 / 22.7 万三角 / 肌肉 35、骨 29、动脉 28、静脉 23、其他 8（含 5 个区域包络）|
|`preview.png`|—|模型预览图|
|`example/index.html`|5.6 MB|单文件示例页（模型已内嵌，双击即开）|
|`AGENT.md`|—|给 AI 助手看的规格说明（尺寸、坐标、陷阱）|

模型世界尺寸 **0.218 × 0.673 × 0.153 米**（218 × 673 × 153 mm，长轴沿 Y），Y 轴向上、站立姿态。

坐标系沿用数据集的整体人体坐标（**原点不在模型中心**，模型位于 x≈0.21、y≈0.99、z≈0.12 米处），这样 12 个部位文件可以叠在一起拼成人体；取景请用包围盒算，不要写死相机位置。

## 让 AI 助手直接帮你做

把下面这段连仓库地址一起发给 DSH：

```
从这个仓库拉取左前臂与左手的解剖模型，做一个可以用鼠标旋转缩放的三维网页：
https://github.com/2962660049/Left-arm-class-glb
先读仓库里的 AGENT.md，再下载 glb/hand_elbow_left.glb（4.7 MB），不要克隆整个仓库。
```

模型直链（写进自己的页面用这个）：

```
https://raw.githubusercontent.com/2962660049/Left-arm-class-glb/main/glb/hand_elbow_left.glb
```

> 如果 `raw.githubusercontent.com` 打不开（国内常见），改用 `git clone https://github.com/2962660049/Left-arm-class-glb.git`，
> 或者下载仓库的 `example/index.html`——它是单文件版，模型已经内嵌，双击就能看。


## 三个必踩的坑

1. **双击打开 html 会加载失败**（`file://` 下浏览器禁止读取本地 glb）。三种解法：

   * 把 html 和 glb 放到同一个本地服务器：`python -m http.server 8000`，浏览器开 `http://localhost:8000`；
   * 或者把模型换成线上地址 `https://raw.githubusercontent.com/2962660049/Left-arm-class-glb/main/glb/hand_elbow_left.glb`（带 CORS，`file://` 下也能读）；
   * 或者用 VS Code 的 Live Server 插件。
2. **模型很大**：长轴 0.67 m。相机别用默认的 `z = 5`，先算包围盒再取景，否则要么看不见要么贴脸。
3. **`bp3d\_group` 是 `other` 的 8 个部件是区域包络**（`userData.bp3d\_envelope === '1'`），不是真实器官，画面上会像半透明的壳，建议默认隐藏。

## 许可

数据来自 **BodyParts3D**（Database Center for Life Science, Japan），采用 **CC BY 4.0**。

页面里请保留这句署名：

> BodyParts3D © ライフサイエンス統合データベースセンター licensed under CC BY 4.0

详见 [NOTICE.md](NOTICE.md)。

