#  一个简单的 Cesium 应用的开发（翻译）

[TOC]

## 简介
欢迎加入 Cesium 家族！为了使你能够尽快学会使用 Cesium 开发网络地图，这个教程将带领你体验一个完整的 Cesium 应用的开发过程。

我们将创建一个简单的应用来可视化纽约市的示例藏宝点。我们将加载多种二三维数据并设置样式，并创建相机和显示选项以方便用户和程序交互。我们将加载一个三维无人机模型来侦察藏宝点。

看完这个教程之后，你将对 Cesium 有一个大致的了解并学会配置 Cesium 的查看器（viewer）、加载数据集、创建几何图形并设置样式、使用三维切片、控制相机、给程序添加鼠标交互功能。

## 安装
只需要几步我们就可以开始开发了
1. 访问 [Hello World](http://cesiumjs.org/Cesium/Apps/HelloWorld.html) 看看你的系统是否支持 Cesium。看不到地球？看看[Troubleshooting](http://cesiumjs.org/troubleshooting.html) 。
2. 安装 [Node.js](https://nodejs.org/en/)。
3. 获取 [workshop code](https://github.com/AnalyticalGraphicsInc/cesium-workshop) ，解压。
4. 打开命令行窗口，进入 `cesium-workshop` 目录。
5. 运行 `npm install` 。
6. 运行 `npm start` 。

运行 `npm start` 之后，将会输出“Cesium development server running locally. Connect to http://localhost:8080/” 。请不要关闭命令行！这个程序需要一直运行。

在浏览器中打开 `localhost:8080` ，你应该可以看到程序运行的效果。
![程序最终效果](./images/cesium01.jpg)

###关于程序目录
请看看程序目录！这个程序目录已经尽可能设计得很简单，忽略了很多现在 JS 框架。
- Source : 应用程序的代码和数据。
- ThirdParty : 外部库，在这个程序里面只有 Cesium。
- LICENSE.md : 这个程序的使用条款。
- index.html : 这个程序主要的网页。
- server.js : 这个程序的服务器。

查看 `index.html` 文件可以发现它创建了一个 `div` (用于 Cesium 组件) 和一些基本的输入元素。Cesium 组件只是一个普通的 `div`。

`index.html` 文件里有几行代码很重要：
在 `HTML` 头部的引入 `Cesium.js` 的代码。 `Cesium.js` 定义了 `Cesium` 对象，`Ceisum` 对象包含整个 Cesium 库。
```html
<script src="ThirdParty/Cesium/Cesium.js"></script>
```

`Cesium` 附带了一些组件，他们需要 CSS。
```html
<style>@import url(ThirdParty/Cesium/Widgets/widgets.css);</style>
```

在 `HTML body` 中，需要为 Cesium 查看器组件创建一个 `div `。
```html
<div id="cesiumContainer"></div>
```

最后，需要在 `HTML body` 的结束标签之前添加 `script` 标签引入这个应用程序需要的 JavaScript 脚本。
```html
<script src="Source/App.js"></script>
```

就是这些了。整个 HTML 文件剩余的内容用于收集用户输入，我们后面会用到。

### 开发资料
遇到问题的时候，在这些资料里也许能找到答案。
- [Reference Documentation](http://cesiumjs.org/refdoc.html) : 关于 Cesium API 的一份完整的指南，包含很多代码片段。
- [Sandcastle](https://cesiumjs.org/Cesium/Build/Apps/Sandcastle/index.html) : 一个实时编程环境，附带很多示例。
- [Tutorials](http://cesiumjs.org/tutorials.html) : 关于 Cesium 开始的详细介绍。
- [Cesium Forum](http://cesiumjs.org/forum.html) : Cesium 相关的问答资源。

### 开发流程

在之后的学习中，我们的开发工作基本分为以下 5 个步骤：
1. 打开 `Source/App.js` 并删除里面的内容。
2. 复制 `Source/AppSkeleton` 中的内容到 `Source/App.js`。
3. 确保你的 `cesium-workshop` 下的服务还在运行。
4. 用浏览器打开 `localhost:8080` ，应该可以看到一个黑色的界面。
5. 去掉代码注释并保存文件，刷新浏览器查看效果。


## 创建查看器（Viewer）

查看器是 Cesium 应用程序的基础，是一个带有很多现成功能的可交互的三维地球。通过取消注释第一行代码即可添加查看器到名为 cesiumContainer 的 `div`。
```javascript
var viewer = new Cesium.Viewer('cesiumContainer');
```

这一行代码实现了很多内容！你应该可以在执行程序后看到一个简单的地球：
![](./images/cesium02.jpg)

默认情况下，这个程序能处理鼠标输入和触摸输入。你可以试着尝试以下操作（这些操作功能由 camera 控件提供）：
- 按住鼠标左键拖动 ：在地球表面平移（实质是沿着地球表面平移相机）。
- 按住鼠标右键拖动 ：缩放地球。
- 滚动鼠标滚轮 ：也是缩放地球。
- 按住鼠标中键拖动 ：沿着地球表面的点旋转地球（实质是旋转相机）。

除了地球之外，查看器还默认附带一些有用的小部件，上图中用数字标出的就是。
1. Geocoder : 地理编码器，用于搜索位置的工具，它还能飞行到搜索到的位置。
2. Home Button : 主页按钮，用于返回到默认视图。
3. Scene Mode Picker : 场景模式选择器，用于在二维视图、三维视图、哥伦布视图之间切换。
4. Base Layer Picker : 地图选择器，用于选择显示在地球上影像和地形。
5. Navigation Help Button : 导航辅助按钮，用于显示默认的相机控件。
6. Animation : 动画控件，用于控制动画播放速度。
7. Timeline : 时间线控件，用于显示当前的时间位置，可以跳转到时间线上其他位置。
8. Credits Display : 用于显示数据归属，这个是必须有的。
9. 全屏按钮 ： 用于全屏显示查看器。

当我们创建查看器的时候，可以通关传递给构造函数的参数来决定包不包含一些默认的功能。在这个程序中，我们可以删除第一行代码，然后取消注释以下几行代码来配置默认功能。
```javascript
var viewer = new Cesium.Viewer('cesiumContainer', {
    scene3DOnly: true,
    selectionIndicator: false,
    baseLayerPicker: false
});
```

## 添加影像




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
最后，我们给场景添加一些鼠标交互。为了提升地理藏宝标记的视觉效果，我们可以设置当鼠标滑过标记的时候高亮它。

为了实现这个，我们需要使用 picking ,它是 Cesium 的一个功能，能根据鼠标在 查看器中的位置返回三维场景中的数据。

以下是几种不同的类型的 picking 。
- [Scene.pick](http://cesiumjs.org/Cesium/Build/Documentation/Scene.html#pick) 根据给定的窗口位置返回一个包含元件的对象。
- [Scene.drillPick](http://cesiumjs.org/Cesium/Build/Documentation/Scene.html#drillPick) 根据给定的窗口位置，返回包含所有元件的对象列表。
- [Globe.pick](http://cesiumjs.org/Cesium/Build/Documentation/Globe.html?classFilter=globe#pick) 返回给定射线和地形的交点。

以下是一些应用 picking 的例子
- [Picking Demo](https://cesiumjs.org/Cesium/Build/Apps/Sandcastle/index.html?src=Picking.html&label=Showcases)
- [3D Tiles Feature Picking Demo](https://cesiumjs.org/Cesium/Build/Apps/Sandcastle/index.html?src=3D%20Tiles%20Feature%20Picking.html&label=3D%20Tiles)

为了实现鼠标滑过高亮效果，我们首先需要创建鼠标行为处理器。因此我们要使用 [ScreenSpaceEventHandler](http://cesiumjs.org/Cesium/Build/Documentation/ScreenSpaceEventHandler.html) ，它是一组处理器，当接收到用户输入时调用指定的函数。 [ScreenSpaceEventHandler.setInputAction()](http://cesiumjs.org/Cesium/Build/Documentation/ScreenSpaceEventHandler.html#setInputAction) 监听用户的行为类型（[ScreenSpaceEventType](http://cesiumjs.org/Cesium/Build/Documentation/ScreenSpaceEventType.html)） 并调用指定函数（用户行为将作为参数）。

```javascript
var handler = new Cesium.ScreenSpaceEventHandler(viewer.scene.canvas);
handler.setInputAction(
    function(movement) {},   
    Cesium.ScreenSpaceEventType.MOUSE_MOVE
);
```
接下来，我们看看高亮的实现。处理器将传递鼠标事件作为参数，从这个参数能获取到鼠标单击窗口的位置。我们将位置用到 `pick()` 中，如果 `pick()` 返回了一个 `billboard` 对象，说明鼠标滑过了标记。然后，使用之前学到的关于实体样式设置的知识，我们可以为标记设置高亮样式。
```javascript
handler.setInputAction(
    function(movement) {
        var pickedPrimitive = viewer.scene.pick(movement.endPosition);
        var pickedEntity = (Cesium.defined(pickedPrimitive)) ? pickedPrimitive.id : undefined;
    // 高亮实体
        if (Cesium.defined(pickedEntity) && Cesium.defined(pickedEntity.billboard)) {
            pickedEntity.billboard.scale = 2.0;
            pickedEntity.billboard.color = Cesium.Color.ORANGERED;
        }
    }, 
Cesium.ScreenSpaceEventType.MOUSE_MOVE

);

```
运行程序我们发现当鼠标移开之后标记还是高亮的样式。为了解决这个问题，我们可以跟踪最近高亮的标记并恢复样式。

以下是完整的代码：
```javascript
var previousPickedEntity = undefined;
handler.setInputAction(
    function(movement) {
        var pickedPrimitive = viewer.scene.pick(movement.endPosition);
        var pickedEntity = (Cesium.defined(pickedPrimitive)) ? pickedPrimitive.id : undefined;
        
        // 先取消之前高亮的实体
        if (Cesium.defined(previousPickedEntity)) {
            previousPickedEntity.billboard.scale = 1.0;
            previousPickedEntity.billboard.color = Cesium.Color.WHITE;
        }
    
        // 高亮当前实体
        if (Cesium.defined(pickedEntity) && Cesium.defined(pickedEntity.billboard)) {
            pickedEntity.billboard.scale = 2.0;
            pickedEntity.billboard.color = Cesium.Color.ORANGERED;
            previousPickedEntity = pickedEntity;
        }
    }, 
    Cesium.ScreenSpaceEventType.MOUSE_MOVE
);

```
至此，我们完成了鼠标滑过时高亮标记的功能。
![鼠标滑过高亮效果](./images/cesium11.jpg)


## 相机模式
对于无人飞行，我们在这里可以设置两种简单的模式。
- 自由模式：默认的相机控件
- 无人机模式：相机跟随飞行中的无人机并和无人机保持一段距离

自由模式不需要再写代码，因为他是默认模式。对于无人机模式，我们可以使用查看器内置的实体跟踪功能，将相机定位在无人机上并设置一点偏移。要跟踪实体，我们只需设置 [viewer.trackedEntity](http://cesiumjs.org/Cesium/Build/Documentation/Viewer.html?classFilter=viewer#trackedEntity) 。

要切换回自由模式，我们只需要设置 `viewer.trackedEntity` 为 `undefined` ，然后使用 `camera.flyTo()` 返回到我们的主视图。
```javascript
// 创建一个跟随无人机的相机
function setViewMode() {
    if (droneModeElement.checked) {
        viewer.trackedEntity = drone;
    } else {
        viewer.trackedEntity = undefined;
        viewer.scene.camera.flyTo(homeCameraView);
    }
}
```
我们还需要将这个函数和 HTML 元素的 `change` 事件关联起来
```javascript
var freeModeElement = document.getElementById('freeMode');
var droneModeElement = document.getElementById('droneMode');

// 创建一个跟随无人机的相机
function setViewMode() {
    if (droneModeElement.checked) {
        viewer.trackedEntity = drone;
    } else {
        viewer.trackedEntity = undefined;
        viewer.scene.camera.flyTo(homeCameraView);
    }
}

freeModeElement.addEventListener('change', setCameraMode);
droneModeElement.addEventListener('change', setCameraMode);

```
最后，可以设置当双击实体的时候开始跟踪，并更新界面样式。
```javascript
viewer.trackedEntityChanged.addEventListener(function() {
    if (viewer.trackedEntity === drone) {
        freeModeElement.checked = false;
        droneModeElement.checked = true;
    }
});
```
![相机模式效果](./images/cesium12.jpg)

## 其他
剩下的代码只是添加了一点可视化选项。类似于之前的交互，我们添加了切换阴影和邻接多边形的功能。
```javascript
var shadowsElement = document.getElementById('shadows');
var neighborhoodsElement =  document.getElementById('neighborhoods');

shadowsElement.addEventListener('change', function (e) {
    viewer.shadows = e.target.checked;
});

neighborhoodsElement.addEventListener('change', function (e) {
    neighborhoods.show = e.target.checked;
    tileStyle.value = 'transparent';
    city.style = transparentStyle;
});

```
因为三维切片可能不会瞬间加载完，所以我们可以添加一个加载提示并在加载完成之后移除这个提示。
```javascript
// 模型加载完成时移除提示
var loadingIndicator = document.getElementById('loadingIndicator');
loadingIndicator.style.display = 'block';
city.readyPromise.then(function () {
    loadingIndicator.style.display = 'none';
});
```

恭喜！我们应用完成了。你现在已经完成了一个完整 Cesium 应用的开发。


















- 原文 https://cesiumjs.org/tutorials/Cesium-Workshop/#loading-and-styling-entities

