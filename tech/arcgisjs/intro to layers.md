# 图层简介（翻译 Intro to layers）
> 这是一篇英文翻译，原文：https://developers.arcgis.com/javascript/latest/sample-code/intro-layers/index.html


图层是地图最基本的组成部分。图层是以图形或影像表示现实世界的空间数据的集合。在矢量数据中，图层包含很多离散的要素，而在栅格数据中图层包含很多连续的单元/像素。

地图可能包含不同类型的图层。有关 API 中可用的图层类型，请查阅关于 [Layer 类型的描述](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-Layer.html#layer-types)。

所有的图层类型都继承了 Layer 类的属性、方法、事件。本教程会讨论一些常用的属性。想了解更多关于不同图层的属性，可以在[这里](https://developers.arcgis.com/javascript/latest/sample-code/intro-layers/index.html?search=*layer) 搜索示范程序（如：输入 SceneLayer,单击搜索）。

在查看下列步骤之前，你应该对 [View](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-View.html) 和 [Map](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html) 熟悉。建议先查看以下教程：

- [Intro to MapView](Intro to MapView-Create a 2D map.md)
- [Intro to SceneView](Intro to SceneView-Create a 3D map.md)

1. 创建地图、场景和复选框

创建地图和场景的 JS 代码如下：

```javascript
require([
  "esri/Map",
  "esri/views/SceneView",
  "dojo/domReady!"
], function(
  Map,
  SceneView
) {

  // 创建地图
  var map = new Map({
    basemap: "oceans"
  });

  // 创建场景
  var view = new SceneView({
      container: "viewDiv",
      map: map
  });
});
```

创建复选框的代码如下。关于目的下文会讨论到。

```html
<body>
  <div id="viewDiv"></div>
  <span id="layerToggle">
    <input type="checkbox" id="streetsLyr" checked> Transportation
  </span>
</body>
```

2. 使用 TileLayer 创建两个图层
在创建 map 和 view 之前，应当创建两个 [TileLayer](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-TileLayer.html) 的实例。。创建 TileLayer 的实例需要加载 `esri/TileLayer` 模块并为实例指定 [url](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-TileLayer.html#url) 属性值。`url` 属性值必须指向一个地图缓存服务（托管在 ArcGIS Server 或者 Portal for ArcGIS）。

在这个例子中，我们为街道和高速公路分别创建了图层。

```javascript
require([
    "esri/Map",
    "esri/views/SceneView",
    "esri/layers/TileLayer",  // Require the TileLayer module
    "dojo/domReady!"
  ],
  function(
    Map, SceneView, TileLayer
  ) {

    var transportationLyr = new TileLayer({
      url: "https://server.arcgisonline.com/ArcGIS/rest/services/Reference/World_Transportation/MapServer"
    });

    var housingLyr = new TileLayer({
      url: "https://tiles.arcgis.com/tiles/nGt4QxSblgDfeJn9/arcgis/rest/services/New_York_Housing_Density/MapServer"
    });

    /*****************************************************************
    * 前面的步骤创建 map 和 view 的代码应该放在这里
    *****************************************************************/
});
```

3. 在图层中设置其它属性
你可以在图层上设置其它属性，例如 [id](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-Layer.html#id) 、 [minScale](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-Layer.html#minScale) 、 [maxScale](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-Layer.html#maxScale) 、 [opacity](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-Layer.html#opacity) 、 [visible](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-Layer.html#visible) 。可以在构造函数中设置，也可以在程序中的其它位置设置。

```javascript
var transportationLyr = new TileLayer({
  url: "https://server.arcgisonline.com/ArcGIS/rest/services/Reference/World_Transportation/MapServer",
  id: "streets",
  opacity: 0.7
});

var housingLyr = new TileLayer({
  url: "https://tiles.arcgis.com/tiles/nGt4QxSblgDfeJn9/arcgis/rest/services/New_York_Housing_Density/MapServer",
  id: "ny-housing"
});
```

`id` 唯一标识图层，使得我们可以在程序其他的其它引用图层。如果开发者没有设置，id 会自动生成。

4. 添加图层到地图
图层可以通过多种方式添加到地图中。关于添加图层方式，可查看 [Map.layers](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html#layers) 。在这个例子中，我们使用不同的方式来添加图层。

使用构造函数添加图层：
```javascript
// 创建图层的代码在此之前
var map = new Map({
  basemap: "oceans",
  layers: [housingLyr]  // 使用构造函数添加图层
});
```

使用 [map.layers.add()](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html#add) 添加图层：
```javascript
map.layers.add(transportationLyr);
```

5. 处理图层的可见性。

加载 `dojo/on` 和 `dojo/dom` 模块以实现监听第 1 步创建的复选框的改变事件。


---
[//]: # (内嵌 html)
<footer style="background:#000;color:white;border-radius:5px;padding:5px;">
  对我来说，这是翻译，也是学习笔记，主要是为了学习。如果有哪里不对并希望帮助我改进，可邮件：hgy9473@foxmail.com
</footer>