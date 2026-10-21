# 署名与来源（Attribution & Source）

本仓库的 3D 模型是从 **BodyParts3D** 数据集转换而来，不是原创作品。
按 **CC BY 4.0** 的要求，任何使用（页面、课件、论文、视频、二次分发）都必须保留下列署名。

## 必须保留的署名文本

```
BodyParts3D, (c) The Database Center for Life Science licensed under CC Attribution 4.0 International
```

日文原写法（与官方一致）：

```
BodyParts3D © ライフサイエンス統合データベースセンター licensed under CC表示 4.0 国際
```

中文页面可直接写：

> 模型数据来自 BodyParts3D（© ライフサイエンス統合データベースセンター），依 CC BY 4.0 许可使用。

## 来源

- 数据集主页：https://dbarchive.biosciencedbc.jp/en/bodyparts3d/
- 下载归档：https://dbarchive.biosciencedbc.jp/data/bodyparts3d/LATEST/
- 本仓库使用的文件：`isa_BP3D_4.0_obj_99.zip`（ISA 子集，99% 减面，2234 个网格，136.3 MB）
- 许可文本：https://creativecommons.org/licenses/by/4.0/
- 许可说明（数据集方）：https://dbarchive.biosciencedbc.jp/en/bodyparts3d/lic.html

## 本项目做了什么（衍生说明）

1. 从 ISA 包中解析全部 2234 个 OBJ（顶点 + 三角面），未改动几何形状；
2. 按解剖区域重新分类为 11 个部位（多标签，允许同一结构出现在多个部位）；
3. 在 Blender 5.2 中导入、按组织分配纯色材质、平滑着色，导出为 glTF 2.0 / GLB；
4. 坐标从毫米 / Z-up 转为米 / Y-up。

几何数据未经减面或重拓扑，仍为数据集原始精度。

## 引用建议

学术使用时可引用数据集原始文献：

> Mitsuhashi N, Fujieda K, Tamura T, Kawamoto S, Takagi T, Okubo K.
> BodyParts3D: 3D structure database for anatomical concepts.
> Nucleic Acids Research, 2009; 37(Database issue):D782-5.
