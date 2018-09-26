# js 工具描述

[TOC]

## Babel

用途：把 JavaScript 中 ES2015/2016/2017/... 的新语法转化为 ES5，让低端运行环境(如浏览器和 Node )能够认识并执行。
Github:[The compiler for writing next generation JavaScript.](https://github.com/babel/babel)
中文资源：[Babel 中文网](https://www.babeljs.cn/)

> Babel 的自我定位是一个编译器
> Babel is a compiler for writing next generation JavaScript.

## Vue.js

Vue.js 是一个JavaScript MVVM 库，是一套用于构建用户界面的渐进式框架。它以数据驱动视图和组件化的思想构建，采用自底向上的增量开发设计。

* 数据驱动视图

* 双向数据绑定

* 虚拟DOM(Virtual Dom)
  * 当状态发生变化时，Virtual Dom 会进行 diff 运算，然后只更新需要更新的节点。