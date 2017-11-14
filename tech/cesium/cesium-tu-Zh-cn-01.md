




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





