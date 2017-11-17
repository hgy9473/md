




接下来，我们来设置邻域多边形的样式。就像设置 billboard 样式一样，当数据源加载完成之后，我们遍历领域多边形实体，检查多边形是否已经被定义：
```javascript
var neighborhoods;
neighborhoodsPromise.then(function(dataSource) {
    // 添加新的实体数据到 viewer
    viewer.dataSources.add(dataSource);
    neighborhoods = dataSource.entities;

    // 获取实体数组
    var neighborhoodEntities = dataSource.entities.values;
    for (var i = 0; i < neighborhoodEntities.length; i++) {
        var entity = neighborhoodEntities[i];

        if (Cesium.defined(entity.polygon)) {
            // 在这里设置实体样式
        }
    }
});
```

我们可以使用邻域作为实体名称重命名实体。原始的 GeoJson 文件有一个 neighborhood 属性。Cesium 把 GeoJson 属性存在 `entity.properties` 中,所以我们可以像下面这样设置名称：
```javascript
// 设置实体样式

// 使用 geojson neighborhood 作为实体名称
entity.name = entity.properties.neighborhood;
```

我们可以将所有邻域设置为同一种颜色，也可以通过[ColorMaterialProperty](http://cesiumjs.org/Cesium/Build/Documentation/ColorMaterialProperty.html?classFilter=material) 来设置随机[颜色](http://cesiumjs.org/Cesium/Build/Documentation/Color.html?classFilter=color)。

```javascript
// 设置实体样式

// 给多边形设置随机的半透明颜色
entity.polygon.material = Cesium.Color.fromRandom({
    red : 0.1,
    maximumGreen : 0.5,
    minimumBlue : 0.5,
    alpha : 0.6
});
```
最后，我们使用基础的样式选项为每一个实体生成一个[标签](http://cesiumjs.org/Cesium/Build/Documentation/LabelGraphics.html?classFilter=label)。为了保持整洁，我们可以设置 [disableDepthTestDistance](http://cesiumjs.org/Cesium/Build/Documentation/LabelGraphics.html?classFilter=label#disableDepthTestDistance) 来使 Cesium 只渲染场景中前面的没有被遮住的标签。

但是请注意，标签应该放置在 `Entity.position` 上，应该有一个位置。多边形没有 `position` 属性，因为它是由很多遍组成的。我们将多边形的中心作为多边形的 `position`（位置）属性。
```javascript
// 设置实体样式

// 生成多边形位置
// 获取多边形点集
var polyPositions = entity.polygon.hierarchy.getValue(Cesium.JulianDate.now()).positions;
// 计算点集中心
var polyCenter = Cesium.BoundingSphere.fromPoints(polyPositions).center;
// 转换坐标
polyCenter = Cesium.Ellipsoid.WGS84.scaleToGeodeticSurface(polyCenter);
// 设置 position 属性
entity.position = polyCenter;
// 生成标签
entity.label = {
    text : entity.name,
    showBackground : true,
    scale : 0.6,
    horizontalOrigin : Cesium.HorizontalOrigin.CENTER,
    verticalOrigin : Cesium.VerticalOrigin.BOTTOM,
    distanceDisplayCondition : new Cesium.DistanceDisplayCondition(10.0, 8000.0),
    disableDepthTestDistance : Number.POSITIVE_INFINITY
};
```
添加了标签的多边形像这样：
![添加了标签的多边形](./images/cesium07.jpg)

尽管我们设置了标签的多边形很好看，但是整个场景已经变得有点混乱了。所以我们要创建一种隐藏多边形的方式。一般来说，我们可以使用 [Entity.show](http://cesiumjs.org/Cesium/Build/Documentation/Entity.html?classFilter=entity#show) 设置可见性来隐藏实体。但是，它只能设置单个实体的可见性，我们想一次隐藏或显示所有相邻的实体。

我们可以通过添加相邻实体到父实体来实现（[示例](https://cesiumjs.org/Cesium/Build/Apps/Sandcastle/index.html?src=Show%20or%20Hide%20Entities.html&label=Showcases)），或者也可以使用 [Entity Collection](http://cesiumjs.org/Cesium/Build/Documentation/EntityCollection.html) 的 `show` 属性来实现。我们通过改变 `neighborhood.show` 的值来设置所有子实体的可见性。
```javascript
neighborhoods.show = false;
```
最后，我们来给城市上空添加一辆无人机。

因为飞行路径是随着时间变化的一些点，所以可以这个数据保存在[ CZML 文件](https://github.com/AnalyticalGraphicsInc/czml-writer/wiki/CZML-Guide) 中。CZML 是一种描述事件动态图形场景的格式，主要显示在运行 Cesium 的浏览器中。它描述线、点、 billboard 、模型以及其他图形元件，并指定他们随着时间改变而变化的方式。CZML 之于 Cesium 犹如 KML 之于 Google Earth ，它是一种标准格式，它使得大多数 Cesium 功能可以通过声明样式型语言来使用。

我们的 CZML 文件定义了一个实体（一个点），它的位置是在不同时间的一系列点位。这里有一个 Entity API 存储动态值的 Property 系统的[例子](https://cesiumjs.org/Cesium/Build/Apps/Sandcastle/index.html?src=Hello%20World.html&label=Showcases&gist=c1b1ef84af7dd0b40b3102ffed164a3d)。

```javascript
// 从 CZML 文件加载无人机路径
var dronePromise = Cesium.CzmlDataSource.load('./Source/SampleData/SampleFlight.czml');

dronePromise.then(function(dataSource) {
    viewer.dataSources.add(dataSource);
});
```
在这里，Cesium 使用 `PathGraphics` （显示实体位置随时间变化的一个属性）显示无人机飞行。通过使用插值，由离散样本组成的路径可以被可视化为连续的线路。

接下来，我们可以加载一个三维模型并将其附加到实体上来表示无人机。

三维示例：[示例1](https://cesiumjs.org/Cesium/Build/Apps/Sandcastle/index.html?src=3D%20Models.html&label=Showcases) [示例2](https://cesiumjs.org/Cesium/Build/Apps/Sandcastle/index.html?src=3D%20Models%20Coloring.html&label=Showcases)

Cesium 支持加载基于 glTF(GL 传输格式，一种免税规范，用于高效率传输，通过使用最小化文件大小和处理时间的程序来加载三维场景和模型) 的三维模型。我们提供一个[在线转换器](http://cesiumjs.org/convertmodel.html) 来转换 COLLADA 和 OBJ 文件为 glTF 格式。

我们加载一个无人机模型，并添加物理阴影和一些动画：
```javascript
var drone;
dronePromise.then(function(dataSource) {
    viewer.dataSources.add(dataSource);
    drone = dataSource.entities.values[0];
    // 添加三维模型
    drone.model = {
        uri : './Source/SampleData/Models/CesiumDrone.gltf',
        minimumPixelSize : 128,
        maximumScale : 2000
    };
});
```
现在我们加入的模型很棒，但是和原来的点不同，无人机是有方向的，如果移动的时候无人机没有转动会很奇怪。幸好，Cesium 提供了一个 [VelocityOrientationProperty 属性](http://cesiumjs.org/Cesium/Build/Documentation/VelocityOrientationProperty.html),它可以根据实体的前后采样位置自动计算方向。
```javascript
// 基于采样位置计算方向
drone.orientation = new Cesium.VelocityOrientationProperty(drone.position);
```
为了提升无人机飞行的外观，还有一件事可以做。无人机的飞行路线是线段，看起来不自然，这是因为 Cesium 默认使用线型插值来构建路径。不过还好，插值选项可以修改。
[插值示例](https://cesiumjs.org/Cesium/Build/Apps/Sandcastle/index.html?src=Interpolation.html&label=All)

为了得到更平滑的飞行路线，我们可以修改插值插值选项：
// 平滑路径插值
```javascript
drone.position.setInterpolationOptions({
    interpolationDegree : 3,
    interpolationAlgorithm : Cesium.HermitePolynomialApproximation
});
```
![平滑飞行路径](./images/cesium08.jpg)

## 三维切片


- 原文 https://cesiumjs.org/tutorials/Cesium-Workshop/#loading-and-styling-entities



