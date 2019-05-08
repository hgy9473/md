# 前端工程化笔记

## 传统的代码依赖管理的问题

> 1. It is not immediately apparent that the script depends on an external library.
> 2. If a dependency is missing, or included in the wrong order, the application will not function properly.
> 3. If a dependency is included but not used, the browser will be forced to download unnecessary code.


## 工程目录结构解释

```c
  webpack-demo
  |- package.json
  |- /dist
    |- index.html
  |- /src
    |- index.js
```

### src 和 dist

> First we'll tweak our directory structure slightly, separating the "source" code (/src) from our "distribution" code (/dist). The "source" code is the code that we'll write and edit. The "distribution" code is the minimized and optimized output of our build process that will eventually be loaded in the browser.
> 
> 来源：https://webpack.js.org/guides/getting-started/

### --save 和 --save-dev

```javascript
// package.json

// --save-dev
  "devDependencies": {
    "webpack": "^4.29.0",
    "webpack-cli": "^3.2.1"
  },

  // --save
  "dependencies": {
    "loadsh": "0.0.3",
    "lodash": "^4.17.11"
  }

```

> When installing a package that will be bundled into your production bundle, you should use npm install --save. If you're installing a package for development purposes (e.g. a linter, testing libraries, etc.) then you should use npm install --save-dev. 
> 
> 来源：https://webpack.js.org/guides/getting-started/