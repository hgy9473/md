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