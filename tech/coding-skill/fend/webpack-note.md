# Webpack 笔记

* 用 Webpack 处理你的应用时，它会递归的构建每个模块的依赖关系图，然后把所有的这些依赖模块打包到一个由数个小块组成的文件中 —— 通常只有一个（将被浏览器加载）

* Webpack 有四个核心概念：入口（entry），输出(output)，加载器(loader)，插件(plugins)。这四个概念对应 webpack.config.js 中的四个配置项。

```js
// file: webpack.config.js
var path = require('path');

module.exports = {
    entry: { },
    module: {
        rules:[
          {
            loader:''
          }
        ]
    },
    output: {
        path: '',
        filename: ''
    },
    plugins: [ ],
}

```

* 入口（entry）：告诉 webpack 从哪里开始打包。Webpack 将构建整个项目的依赖树，entry 用来指定打包的起始节点（依赖树的根节点），Webpack 会从这个节点出发将所有依赖的模块打包进来。

* 输出（output）: 告诉 Webpack 怎么处理打包的代码。output 指定了打包后的文件放置的位置，以及命名等信息。

* 加载器（loader）: Webpack 把所有的资源（css\js\image等）都看作模块。Webpack 只能处理 JS，借助 loader ,Webpack 能将其他资源也转为模块。加载器可以预处理（如压缩、检查）文件。加载器的很多功能由外部插件完成。

* 插件（plugins）:用于解决加载器无法处理的事情。loader 只能针对某种特定类型的文件进行处理，而 plugin 的功能则更为强大，能够介入到整个 Webpack 编译的生命周期

## 命令笔记

> webpack版本：2.3.2  
> 项目结构  
> ├── app  
> │   └── index.js  
> ├── node_modules  
> ├── package-lock.json  
> ├── package.json  
> └── webpack.config.js

```js
// file: webpack.config.js

const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

const PATHS = {
  app: path.join(__dirname, 'app'),
  build: path.join(__dirname, 'build')
}

module.exports = {
  entry: {
    app: PATHS.app
  },
  output:{
    path: PATHS.build,
    filename: '[name].js'
  },
  plugins:[
    new HtmlWebpackPlugin({
      title:'webpack demo',
    })
  ]
}

```

* 命令行编译

```c
// 项目根目录执行
$ node_modules/.bin/webpack

/* 执行完之后项目根目录多了一个文件夹build,
   build 下面有两个文件：index.html app.js
 */

```

* 快捷编译

在 `package.json` 文件中 `scripts` 值中添加 `"build": "webpack"`。然后调用：

> webpack 命令后面还可加参数。

```c
// npm 调用编译命令
$ npm run build

```

* 实现实时刷新

安装 webpack-dev-server(简称 WDS):`npm install webpack-dev-server --save-dev`

在`package.json` 文件中 `scripts` 值中添加 `"start": "webpack-dev-server --env development"` 然后调用：

```c
// npm 调用
$ npm run start
/*
  这种情况下编译结果是放在内存中的。
  当项目文件被修改的时候，项目会立即被编译并且浏览器内容被更新。
  这种方式有利于快速开发。

  如果要将打包结果保存下来，参考前面的方式。
*/


```

## 配置 js 检查（eslint-loader）

* 在 webpack.config.js 中添加：

```js

  module:{
    rules:[
      {
        test: /\.js$/,
        enforce: 'pre',
        loader:'eslint-loader',
        options: {
          emitWarning: true,
        },
      },
    ],
  },

```

* 如果代码有错误，用上面的方式去编译就可以看到错误提示，编译会因为有错误而停止。

* 可以在 webpack.config.js 中添加配置来实现将编译错误显示到浏览器（WDS 运行时）

```js
  // file: webpack.config.js
  // 将错误显示到浏览器
  devServer:{
    overlay:{
      errors:true,
      warnings:true,
    },
  },
```

## CSS 处理

* css-loader 处理 `@import` `url()` 相关的资源。

* style-loader 处理 style 标签

1. 安装加载器

```c
// 安装 css 处理插件
$ npm install css-loader style-loader --save-dev

```

2. 配置加载器

在 webpack.config.js 中增加：

```js
// 配置css处理器
  module:{
    rules:[
      {
        test:/\.css$/,
        use:['style-loader','css-loader'],
      },
    ],
  },

// 注意 css-loader 和 style-loader 的先后顺序不能换
```

3. 添加 common.js 文件

```js
// 文件结构如下：

├── app
│   ├── common.css
│   └── index.js
├── build
├── package-lock.json
├── package.json
└── webpack.config.js

// file: common.css
body{
  background-color:red;
}

// file: index.js
import '../app/common.css';
//...
//...

// 构建后可以得到 index.html 和 app.js ,运行 index.html 可以看到 css 的效果。

```

## 代码压缩

* 安装配置加载器 `babili-webpack-plugin`

## Source Map

打包后的文件不方便找到出错的源代码位置，Source Map可以帮忙处理这个问题。

通过 Source Map 相关的配置，webpack 可以再打包时生成 Source Map，这为我们提供了一种源文件和编译成果对应关系图，使得编译后的代码方便调试。

配置示例：
```js

// source map 配置示例
// webpack.config.js

devtool:'source-map',

```

## 代码分类打包

* 之前的配置，webpack 打包后所有 js 代码都在 app.js 里面。

* 通过配置 vendor 和 webpack.optimize.CommonsChunkPlugin, webpack 可将项目中的第三方库以及其他公共代码打包到 vendor.js 中。

```js

// 代码分类打包配置示例
// file: webpack.config.js
  entry: {
    app: PATHS.app,
    vendor: ['jquery'],
  },

  plugins:[
    new HtmlWebpackPlugin({
      title:'webpack demo',
    }),
    plugin,
    new BabiliPlugin(),
    new webpack.optimize.CommonsChunkPlugin({
      name: 'vendor',
    }),
  ],

```

## 打包结果统计分析

* 使用可视化工具可以更加直观的查看项目中相关资源的依赖关系、使用情况。

* 配置：

```js
// file：package.json

  "scripts": {
    "stats": "webpack --env production --profile --json > stats.json"
  },

```

* 运行 `npm run stats`，将会在项目根目录生成 stats.json 文件，这个文件中存放了统计结果，使用相关网站可以生成可视化图表。

* 可视化网站：
  * [Webpack 官方分析工具](http://webpack.github.io/analyse/)
  * [webpack chart](http://alexkuz.github.io/webpack-chart/)
  * [http://blog.parryqiu.com/2017/06/16/webpack2-Statistics/](http://blog.parryqiu.com/2017/06/16/webpack2-Statistics/)


## 多页面编译

> 以两个页面为例

1. 移除 webpack.config.js 中插件 HtmlWebpackPlugin。
2. 在项目根目录新建 index.html about.html

```html
<!-- file : index.html -->

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>index.html</title>
  <link rel="stylesheet" href="./build/app.css">
</head>
<body>
  <p>This is index page</p>

  <script src="./build/vendor.js"></script>
  <script src="./build/index.js"></script>
</body>
</html>

```

```html
<!-- file : about.html -->

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>index.html</title>
  <link rel="stylesheet" href="./build/app.css">
</head>
<body>
  <p>This is about page</p>

  <script src="./build/vendor.js"></script>
  <script src="./build/about.js"></script>
</body>
</html>

```

3. 修改 webpack.config.js 中 entry 配置

```js

// file: webpack.config.js

const path = require('path');
// const HtmlWebpackPlugin = require('html-webpack-plugin');
const ExtractTextPlugin = require('extract-text-webpack-plugin');
const BabiliPlugin = require('babili-webpack-plugin');
const webpack = require('webpack');

const PATHS = {
  app: path.join(__dirname, 'app'),
  build: path.join(__dirname, 'build'),
};

const plugin = new ExtractTextPlugin({
  filename: '[name].css',
  ignoreOrder: true,
});

module.exports = {
  devServer:{
    overlay:{
      errors: true,
      warnings: true,
    },
  },
  
  entry: {
    index: './app/index.js',
    about: './app/about.js',
    vendor: ['jquery'],
  },
  module:{
    rules:[
      {
        test: /\.js$/,
        enforce: 'pre',
        loader:'eslint-loader',
        options: {
          emitWarning: true,
        },
      },
      {
        test:/\.css$/,
        exclude: /node_modules/,
        use: plugin.extract({
          use: {
            loader: 'css-loader',
            options: {
              modules: true,
            },
          },
          fallback: 'style-loader',
        }),
      },
    ],
  },
  output:{
    path: PATHS.build,
    filename: '[name].js',
  },
  plugins:[
    // new HtmlWebpackPlugin({
    //   title:'webpack demo',
    // }),
    plugin,
    new BabiliPlugin(),
    new webpack.optimize.CommonsChunkPlugin({
      name: 'vendor',
    }),
  ],
};

```

> 最终目录结构(build 是编译之后生成的)
```c
├── about.html
├── app
│   ├── about.js
│   ├── component.js
│   ├── index.js
│   ├── style1.css
│   └── style2.css
├── build
│   ├── about.js
│   ├── app.css
│   ├── app.js
│   ├── index.css
│   ├── index.js
│   └── vendor.js
├── index.html
├── package.json
└── webpack.config.js
```
