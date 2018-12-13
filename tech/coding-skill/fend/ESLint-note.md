# ESLint 使用笔记

> 环境： ESLint@5.6.1 Node@8.12.0   |   Visual Studio Code

## 安装 ESLint

```c
// 安装 
$ npm install eslint --save-dev

```

## 配置

* 在项目根目录添加文件 `.eslintrc.js`

```js
// 文件：.eslintrc.js

module.exports = {
  env: {
    browser: true,
    commonjs: true,
    es6: true,
    node: true
  },
  extends: 'eslint:recommended',
  parserOptions: {
    sourceType: 'module'
  },
  rules: {
    'comma-dangle': ['error', 'always-multiline'],
    indent: ['error', 2],
    quotes: ['error', 'single'],
    semi: ['error', 'always'],
    'no-unused-vars':['warn'],
    'no-console': 0
  }

};

```

## 检查代码：命令行

```c
// 检查文件夹 app 下的文件
$ node_modules/.bin/eslint app/
```

## 检查代码：快捷方式

* 先在 `package.json` 中的 `scripts` 下添加 ` "lintjs": "eslint app/ "`，然后在命令行执行：

```c
// 调用
$ npm run lintjs
// 在这种配置下可以用：
// npm run lintjs -- --fix 实现自动修复代码

```

> Webpack 要实现编译时检查代码可以使用 `eslint-loader` 。这里不多做解释。
