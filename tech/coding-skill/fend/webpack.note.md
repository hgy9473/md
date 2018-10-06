# Webpack 笔记

* 用 Webpack 处理你的应用时，它会递归的构建每个模块的依赖关系图,然后把所有的这些依赖模块打包到一个由数个小块组成的文件中--通常只有一个（将被浏览器加载）

* Webpack 有四个核心概念：入口（entry），输出(output)，加载器(loader)，插件(plugins)。这四个概念对应 webpack.config.js 中的四个配置项。

```js
// file: webpack.config.js
var path = require('path');

module.exports = {
    entry: { },
    module: {
        rules:[ ]
    },
    output: {
        path: '',
        filename: ''
    },
    plugins: [ ],
}

```

* 入口：告诉 webpack 从哪里开始打包。Webpack 将构建整个项目的依赖树，entry 用来指定打包的起始节点（依赖树的根节点），Webpack 会从这个节点出发将所有依赖的模块打包进来。



