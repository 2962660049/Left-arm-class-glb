# AGENT.md — 给 AI 助手的模型规格

本文件写给阅读本仓库的编码助手。读完这一页可以不用再看别的文件。

## 一句话

仓库里只有**一个**模型文件，直接加载它即可。

```
glb/hand_elbow_left.glb     4.74 MB
```

**不要 `git clone` 整个仓库，也不要下载其他文件。** 只需要这一个 glb（外加可选的 `preview.png`）。

远程直链（若页面用 `file://` 打开，必须用这个而不是相对路径）：

```
https://raw.githubusercontent.com/2962660049/Left-arm-class-glb/main/glb/hand_elbow_left.glb
```

## 规格

| 项 | 值 |
|---|---|
| 格式 | glTF 2.0 二进制（GLB），未压缩，无 Draco、无贴图 |
| 单位 | **世界坐标是米**；每个 mesh 节点自带 `scale: [0.001, 0.001, 0.001]`，accessor 里的顶点值仍是毫米 |
| 坐标系 | Y 轴向上，右手系，模型正面朝 +Z |
| 世界包围盒 | 0.107–0.324 (X) · 0.653–1.327 (Y) · 0.042–0.195 (Z)，单位米 |
| 世界尺寸 | 0.218 × 0.673 × 0.153 m（长边沿 **Y**） |
| 原点 | **不在模型中心**（沿用数据集的人体全局坐标，便于与其他部位文件叠加对齐） |
| 部件数 | 123 个 mesh 节点（全部挂在 scene 下，无嵌套层级） |
| 三角面 | 227,028 |
| 材质 | 5 个（MAT_muscle / MAT_bone / MAT_artery / MAT_vein / MAT_other），只有 baseColorFactor，无贴图 |
| 姿态 | 解剖学站立位，前臂与手，手指向下 |

> 注意两点：① 模型是世界空间里**竖着**的（长边沿 Y，约 0.67 m）且**不在原点**；② 顶点数据是毫米、靠节点上的 0.001 缩放变成米。所以**不要**直接读 `accessor.min/max` 当世界坐标，一律用 `Box3.setFromObject(scene)` 取尺寸和中心，再按 fov 反推相机距离。

## 每个节点的 extras

```json
{
  "bp3d_id": "FJ1469",
  "bp3d_group": "muscle",     // muscle | bone | artery | vein | other
  "bp3d_fma": "FMA:37369",    // 可能为空
  "bp3d_cats": "hand_elbow_left",
  "bp3d_envelope": "1"        // 仅区域包络节点有
}
```

在 three.js 里是 `mesh.userData`。

- `bp3d_group` 是给组织分色的唯一可靠依据（不要靠节点名正则）。
- `bp3d_envelope === '1'` 的 5 个节点是**区域包络**（原始数据集里的 "organ zone / anatomical set / musculature"），是空腔或区域外壳，不是实体器官。**默认隐藏**，否则会包住整条手臂。
- 节点名是英文解剖名（如 `abductor digiti minimi of left hand`），可用于检索，但同一名字可能带 `_1` 后缀去重。

## 组织构成

| group | 部件数 |
|---|---|
| muscle | 35 |
| bone | 29 |
| artery | 28 |
| vein | 23 |
| other（含 5 个区域包络） | 8 |

## 最小可用加载代码

```js
import * as THREE from 'three';
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js';

const gltf = await new GLTFLoader().loadAsync(url);
const root = gltf.scene;

const COL = { muscle: 0xc4574f, bone: 0xe8e2d6, artery: 0xb43c34, vein: 0x5b7fa8, other: 0xb9b3aa };
root.traverse((o) => {
  if (!o.isMesh) return;
  const g = o.userData.bp3d_group || 'other';
  if (o.userData.bp3d_envelope === '1') { o.visible = false; return; }
  o.material = new THREE.MeshStandardMaterial({
    color: COL[g], roughness: g === 'bone' ? 0.55 : 0.75,
    metalness: 0, side: THREE.DoubleSide,
  });
});

// 取景
const box = new THREE.Box3().setFromObject(root);
const size = box.getSize(new THREE.Vector3());
const center = box.getCenter(new THREE.Vector3());
const maxY = size.y;
const dist = (maxY / 2) / Math.tan((camera.fov * Math.PI) / 360) * 1.25;
camera.position.copy(center).add(new THREE.Vector3(0.35, 0.15, 1).normalize().multiplyScalar(dist));
camera.lookAt(center);
controls.target.copy(center);
```

## 分发与加载的坑

1. `file://` 下 `GLTFLoader` 读**相对路径**会被 CORS 拦（浏览器禁止 XHR 读本地文件）。要么起本地服务器（`python -m http.server`），要么加载上面的 raw 直链（`raw.githubusercontent.com` 带 `Access-Control-Allow-Origin: *`）。
2. 模型只有 4.7 MB，不要为了"优化"去做 Draco 压缩——会让页面额外依赖解码器，课堂环境容易失败。
3. 不要开 `castShadow`/`receiveShadow` 整片打光，123 个部件做实时阴影在弱显卡上会掉帧；用两盏方向光 + 环境光即可。
4. 材质的 `baseColorFactor` 已是线性值，直接当 `MeshStandardMaterial.color` 用会略偏亮，需要精确配色时按 sRGB 重新乘一次。

## 许可（必须保留）

数据来自 BodyParts3D, © データベースセンター for Life Science，**CC BY 4.0**。
生成的页面/文档里请保留：

```
BodyParts3D © ライフサイエンス統合データベースセンター licensed under CC BY 4.0
```
