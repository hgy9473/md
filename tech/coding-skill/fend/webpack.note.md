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


