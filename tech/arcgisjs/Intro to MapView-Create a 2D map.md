# 介绍 MapView ，创建一张二维地图

本教程将指导你在二维 MapView 中创建一张简单的地图。

1. 引入 ArcGIS API for JavaScript 库
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="initial-scale=1, maximum-scale=1, user-scalable=no">
<title>介绍 MapView ，创建一张二维地图</title>
<link rel="stylesheet" href="https://js.arcgis.com/4.5/esri/css/main.css">
<script src="https://js.arcgis.com/4.5/"></script>
</head>
</html>
```
`<script>` 标签从 CDN 加载 JS 库。`<link>` 引用了 `main.css` 样式表，这个样式表设置了 Esri 小部件和组件的样式。

2. 加载模块
再用一个 `<script>` 标签从 API 加载指定的模块。
- `esri/Map`  加载用于创建地图的代码
- `esri/views/MapView` 加载可以查看二维地图的代码
- `dojo/domReady!` 确保在执行代码之前 DOM 可用

> 感叹号表示 domReady 是一个 AMD 加载器插件。除了 `dojo/domReady` , Dojo 还提供了 `dojo/ready` 。区别是前者需等到 DOM 可用才调用回调函数，后者等待 DOM 准备好，等待所有请求调用完成。更多关于 `dodo/ready` 的信息请查看 [Dojo documentation for dojo/ready](http://dojotoolkit.org/reference-guide/1.10/dojo/ready.html) 。 在简单的情况下，应该使用 `dojo/domReady！`。如果应用程序使用 `parseOnLoad：true`，`Dojo Dijits`，Esri库中的小部件或自定义dijits，则应使用 `dojo/ready`。

```html
<script>
require([
  "esri/Map",
  "esri/views/MapView",
  "dojo/domReady!"
], function(Map, MapView) {
  // 创建地图的代码写在这里
});
</script>
```
> 在构建简单的网页或者做实验时，将 JavaScript 代码放到 script 标签中是非常合适的。但是当构建大型的应用程序时，JavaScript 代码应该放在在一些单独的 .js 文件中。

全局函数 `require()` （由 Dojo 提供）用于加载模块。为了充分利用 Dojo 的模块化代码库和跨浏览器差异的能力，Esri 的 JavaScript API 构建于 Dojo 的基础之上。如果想了解更多关于 Dojo 和 JavaScript API 之间的关系，可以查看 [Working with Dojo](https://developers.arcgis.com/javascript/jshelp/inside_dojo.html) 和 [Why Dojo](https://developers.arcgis.com/javascript/jshelp/why_dojo.html)。

3. 创建地图
创建地图使用 `Map` ,`Map` 它是 Map 类（从 `esri/Map` 加载的模块）的引用。 通过传递一个对象给 Map 构造函数可以设定地图属性，例如 `basemap`。
```javascript
require([
  "esri/Map",
  "esri/views/MapView",
  "dojo/domReady!"
], function(Map, MapView) {
  var map = new Map({
    basemap: "streets"
  });
});
```

`basemap` 选项的值还可以是：`satellite`、`hybrid`、`topo`、`dark-gray`、`oceans`、`osm`、`national-geographic`。可以在[沙箱](https://developers.arcgis.com/javascript/latest/sample-code/sandbox/index.html?sample=intro-mapview) 中试着修改 `basemap`。可以查看 [Map class](https://developers.arcgis.com/javascript/latest/api-reference/esri-Map.html) 以了解其他地图选项的详细信息。

4. 创建二维视图
作为 HTML 文件中的容器，视图参考节点允许用户在 HTML 页面内查看地图。
创建一个 `MapView`，通过它的构造函数设置它的属性:
```javascript
require([
  "esri/Map",
  "esri/views/MapView",
  "dojo/domReady!"
], function(Map, MapView) {
  var map = new Map({
    basemap: "streets"
  });

  var view = new MapView({
    container: "viewDiv",  // 引用视图中的 DOM 节点
    map: map               // 引用第 3 步创建的 map 对象。
  });
});
```
在这个代码片段中，`container` 属性被设置为用于显示地图的 DOM 节点的名称。`map` 属性引用了第 3 步创建的地图对象。如果想为 `view` 设置更多属性（例如 `center`、`zoom`， 他们将会设置地图的初始范围），可以参考 [MapView documentation](https://developers.arcgis.com/javascript/latest/api-reference/esri-views-MapView.html)。

> 有两种视图：MapView(用于创建二维地图) 和 SceneView(用于创建三维地图)。如果想了解更多关于创建三维地图的知识，可以查看[Intro to SceneView](https://developers.arcgis.com/javascript/latest/sample-code/intro-sceneview/index.html)。

5. 定义网页内容
创建地图和视图的 JavaScript 都已经编写完成了！接下来要做的是添加一些和查看地图相关的 HTML 代码。对于这个例子，HTML 代码很简单：添加一个 `body` 标签（定义在浏览器中可以看到内容）和一个 `div` 元素（用于显示创建的地图）。
```html
<body>
  <div id="viewDiv"></div>
</body>
```

这里的 `div` 的 id 和传递给 MapView 的构造函数的 `container` 属性的值一样。

5. 设置网页样式






























