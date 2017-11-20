




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

## 三维切片(3D Tiles)
我们团队有时候把 Cesium 称为现实世界数据的三维游戏引擎。不过，处理现实世界的数据比处理一般的游戏视频资源要困难的得多，因为现实世界的数据分辨率极高，可视化精度要求高。幸运的是，Cesium 与开源社区合作开发了 [3D Tiles](http://cesiumjs.org/2015/08/10/Introducing-3D-Tiles/)，这是一个用于流式传输大量异构三维地理空间数据集的开放式规范。

使用类似 Cesium 的地形和图像流技术， 3D Tiles 使得我们可以查看巨大的模型，包括建筑数据集、CAD (或者 BIM )模型、点云、摄影测量模型。

[三维切片调试工具](https://cesiumjs.org/Cesium/Build/Apps/Sandcastle/index.html?src=3D%20Tiles%20Inspector.html&label=3D%20Tiles)

以下是一些不同格式的 3D Tiles 示例:
- [摄影测量模型](https://cesiumjs.org/Cesium/Build/Apps/Sandcastle/index.html?src=3D%20Tiles%20Photogrammetry.html&label=3D%20Tiles)
- [BIM](https://cesiumjs.org/Cesium/Build/Apps/Sandcastle/index.html?src=3D%20Tiles%20BIM.html&label=3D%20Tiles)
- [点云模型](https://cesiumjs.org/Cesium/Build/Apps/Sandcastle/index.html?src=3D%20Tiles%20Point%20Cloud.html&label=3D%20Tiles)
- [所有格式](https://cesiumjs.org/Cesium/Build/Apps/Sandcastle/index.html?src=3D%20Tiles%20Formats.html&label=3D%20Tiles)

在我们的程序中，我们使用 [Cesium3DTileset](http://cesiumjs.org/Cesium/Build/Documentation/Cesium3DTileset.html) 添加完整的纽约建筑的三维模型来增加真实感。
```javascript
// 添加建筑模型
var city = viewer.scene.primitives.add(new Cesium.Cesium3DTileset({
    url: 'https://beta.cesium.com/api/assets/1461?access_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiJkYWJmM2MzNS02OWM5LTQ3OWItYjEyYS0xZmNlODM5ZDNkMTYiLCJpZCI6NDQsImFzc2V0cyI6WzE0NjFdLCJpYXQiOjE0OTkyNjQ3NDN9.vuR75SqPDKcggvUrG_vpx0Av02jdiAxnnB1fNf-9f7s',
    maximumScreenSpaceError: 16 
}));
```
在这里，我们添加 `tileset` 到 `scene.primitives` 而不是 `scene.entities`，因为 3D Tiles 不是 Entity API。`maximumScreenSpaceError` 指定 Cesium 在渲染一个具体场景显示多少细节，数字越小，视觉效果越好。高度精细的视觉效果必然会带来性能成本，所以改变设置时要小心。

你可能注意到了建筑在地面上的位置不正确。这是使用三维切片时常见的问题，很容易修复。我们可以通过修改 [modelMatrix](http://cesiumjs.org/Cesium/Build/Documentation/Cesium3DTileset.html?classFilter=3dtil#modelMatrix) 属性来调整模型的位置。

我们可以通过转换模型的 boundingSphere 为 [Cartographic](http://cesiumjs.org/Cesium/Build/Documentation/Cartographic.html?classFilter=cartographic)来获取当前模型的偏移，然后添加所需的偏移量并重新设置 modelMatrix 属性。
```javascript
var heightOffset = -32;
city.readyPromise.then(function(tileset) {
    // 调整模型位置
    var boundingSphere = tileset.boundingSphere;
    var cartographic = Cesium.Cartographic.fromCartesian(boundingSphere.center);
    var surface = Cesium.Cartesian3.fromRadians(cartographic.longitude, cartographic.latitude, 0.0);
    var offset = Cesium.Cartesian3.fromRadians(cartographic.longitude, cartographic.latitude, heightOffset);
    var translation = Cesium.Cartesian3.subtract(offset, surface, new Cesium.Cartesian3());
    tileset.modelMatrix = Cesium.Matrix4.fromTranslation(translation);
})

```

三维切片允许我们设置模型部件的样式。
```javascript
var defaultStyle = new Cesium.Cesium3DTileStyle({
    color : "color('white')",
    show : true
});
city.style = defaultStyle;
```
![添加三维切片的效果](./images/cesium09.jpg)

给所有模型设置为同样的颜色只算是用到了皮毛。我们可以根据模型的属性为模型设置不同的样式。以下是根据建筑高度设置颜色的示例：
```javascript
var heightStyle = new Cesium.Cesium3DTileStyle({
    color : {
        conditions : [
            ["${height} >= 300", "rgba(45, 0, 75, 0.5)"],
            ["${height} >= 200", "rgb(102, 71, 151)"],
            ["${height} >= 100", "rgb(170, 162, 204)"],
            ["${height} >= 50", "rgb(224, 226, 238)"],
            ["${height} >= 25", "rgb(252, 230, 200)"],
            ["${height} >= 10", "rgb(248, 176, 87)"],
            ["${height} >= 5", "rgb(198, 106, 11)"],
            ["true", "rgb(127, 59, 8)"]
        ]
    }
});

```

![根据高度为建筑设置不同颜色](./images/cesium10.jpg)


更多的三维切片使用和设置样式的应用请查看 [the 3D sandcastle demos](https://cesiumjs.org/Cesium/Build/Apps/Sandcastle/index.html?src=Hello%20World.html&label=3D%20Tiles)

三维切片示例：
- [格式](https://cesiumjs.org/Cesium/Build/Apps/Sandcastle/index.html?src=3D%20Tiles%20Formats.html&label=3D%20Tiles)
- [摄影测量模型](https://cesiumjs.org/Cesium/Build/Apps/Sandcastle/index.html?src=3D%20Tiles%20Photogrammetry.html&label=3D%20Tiles)
- [设置样式](https://cesiumjs.org/Cesium/Build/Apps/Sandcastle/index.html?src=3D%20Tiles%20Feature%20Styling.html&label=3D%20Tiles)

如果你对三维切片的生成过程好奇或者有一些数据要转换，可以看看 [read more here](https://groups.google.com/d/msg/cesium-dev/xLkYIuku9hA/t_AxUBepAgAJ)。


## 交互













- 原文 https://cesiumjs.org/tutorials/Cesium-Workshop/#loading-and-styling-entities

