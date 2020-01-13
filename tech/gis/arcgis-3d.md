# ArcGIS 3D Note

## 三维数据种类

- 离散的三维模型
- 密集格网：倾斜摄影模型、3D Mesh
- 三维矢量点
- 点云

## 三维数据格式

### I3S 规范
- 全称为OGC Indexed 3D Scene Layer ，以及基于该规范的三维数据格式规范SceneLayer Package(SLPK)，专注于在互联网或离线环境中提供高性能三维可视化和空间分析。

- 已经率先被Esri的ARCGIS、Bentley的ContextCapture、Skyline的PhotoMesh等产品支持。

- ArcGIS基于I3S标准在桌面端的三维数据格式是multipatch，web端和移动端是slpk，其中在web端的slpk是用来发服务的交换缓存格式，在移动端的slpk是用于作为离线数据。

### 三维数据来源

- CityEngine -> multipatch格式和slpk格式

- Smart3D -> slpk格式

- Revit -> ArcGIS Pro直接读取




