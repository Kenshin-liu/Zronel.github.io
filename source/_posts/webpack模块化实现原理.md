---
layout: post
title:  webpack模块化实现原理
date: 2018/05/06
---

## 前言
  如果你已经非常的熟知了解webpack模块化的实现原理，请移步右上角，本篇文章可能对你帮助并不大。
webpack在我们前端开发过程中可以说的上是起了非常大的作用，作为一名前端er，我觉得非常有必要了解一下其中的一些实现原理。

## 什么是webpack
WebPack可以看做是模块打包机：它做的事情是，分析你的项目结构，找到JavaScript模块以及其它的一些浏览器不能直接运行的拓展语言（Scss，TypeScript等），并将其转换和打包为合适的格式供浏览器使用。

## 为什么要使用webpack
当今的网页应用越来越复杂，假如单凭人为的管理依赖，很有可能出错，并且依赖十分复杂不好管理，举个栗子，在前几年的jQuery时代，假如我们想要引用一些jQuery的包，我们就必须先引入jQuery，要是没有事先引入jQuery就会报错，提示jQuery未初始化，一个两个还好，当网页应用日渐复杂，就不好管理了，虽然说js社区也出现过一些前端模块化的解决方案，例如：require.js、sea.js、commonjs等解决方案，但这样各管各家，每种都有自己的一套实现方案，技术迁移日后不方便。使用webpack就不用太过担心以上的这些事情，他会把各种模块化的兼容过来。

## 看一个简单的例子

工程文件

```javascript
// commonModule.js
var counter = 3
function incCounter() {
  counter++
}
module.exports = {
  counter: counter,
  incCounter: incCounter,
}

// esModule.js
export let counter = 3
export function incCounter() {
  counter++
}

// index.js 入口文件
var commonModule = require('./commonModule')
import esModule from './esModule'

```

打包后的文件,这里是极简版的
这里要使用webapck4.0一下的版本，4.0以上的他会自动压缩代码，不好分析

```javascript
(function (modules) {/* webpackBootstrap xxx */})
([
  (function(module, __webpack_exports__, __webpack_require__){
    /* 模块index.js的代码 */
  }),
  (function(module, exports) {
    /* 模块commonModule的代码 */
  }),
  (function(module, __webpack_exports__, __webpack_require__){
     /* 模块esModule的代码 */
  })
]);
```
看着这里我们可以看到，打包出来的整个文件本质上是一个IIFE(立即执行函数),modules是入参，各个文件被打包成了一个个函数，仔细观察，有人会发现commonModule与esModule打包出来后的函数参数不一致，这个待会再解释。不过随着浏览器对ES模块的支持，这种通过哦IIFE方式hack模块化的方式在日后我想会慢慢退出舞台的，目前大多数浏览器还是不支持原生的ES模块机制，故得通过该方式来实现模块化的实现。

接下来分析下各个参数的含义以及用处，modules：就是打包后的各个文件的函数被（）包裹起来。它是一个数组。
module、__webpack_exports__、__webpack_require__这些参数先看看webpackBootstrap里头的代码再说。
同样这里只是摘取了一些关键代码：

```javascript
 (function(modules) { // webpackBootstrap
   // 模块缓存对象
   var installedModules = {};

   // __webpack_require__函数定义
   function __webpack_require__(moduleId) {

     // 检查是否有缓存
     if (installedModules[moduleId]) {
       return installedModules[moduleId].exports;
     }
     // 创建个新的模块对象，并且把它放进缓存数组中
     var module = installedModules[moduleId] = {
       i: moduleId,
       l: false,
       exports: {}
     };

     // 调用模块函数
     modules[moduleId].call(module.exports, module, module.exports, __webpack_require__);

     // 标记该模块已加载
     module.l = true;

     // 返回exports对象
     return module.exports;
   }

   return __webpack_require__(__webpack_require__.s = 0);
 })
```

可以看到我们打包后的module是在
> modules[moduleId].call(module.exports, module, module.exports, __webpack_require__);

这一句中调用的，一共是有3个参数，这里做了一个动态绑定，将模块函数的调用对象绑定为module.exports，这是为了保证在模块中的this指向当前模块。
这里的第一个参数module是当前缓存的模块，包含当前模块的信息和exports；第二个参数exports是module.exports的引用，这也符合commonjs的规范（所以我们平常在写代码时，exports={}与module.exports是等效的）；第三个__webpack_require__ 则是require的实现。
在这个函数内的最后一个就是加载我们在webpack.config.js中定义的入口文件

接下看看我们的入口文件被编译成怎样了：

```javascript
(function(module, __webpack_exports__, __webpack_require__) {

  "use strict";
  Object.defineProperty(__webpack_exports__, "__esModule", { value: true });
  var __WEBPACK_IMPORTED_MODULE_0__esModule__ = __webpack_require__(2);
  var commonModule = __webpack_require__(1);
}),
```

三个参数，module, __webpack_exports__, __webpack_require__

在第二行，看到给__webpack_exports__加了个__esModule的属性，告诉webapck这是个esModule
在webpack定义函数内

```javascript
// define getter function for harmony exports
 	__webpack_require__.d = function(exports, name, getter) {
 		if(!__webpack_require__.o(exports, name)) {
 			Object.defineProperty(exports, name, {
 				configurable: false,
 				enumerable: true,
 				get: getter
 			});
 		}
 	};

 	// getDefaultExport function for compatibility with non-harmony modules
 	__webpack_require__.n = function(module) {
 		var getter = module && module.__esModule ?
 			function getDefault() { return module['default']; } :
 			function getModuleExports() { return module; };
 		__webpack_require__.d(getter, 'a', getter);
 		return getter;
 	};
```

在这里__webpack_require__.n会判断module是否为es模块，当__esModule为true的时候，标识module为es模块，那么module.a默认返回module.default，否则返回module。

具体实现则是通过 __webpack_require__.d将具体操作绑定到属性a的getter方法上的。

一个奇怪的想法，我要是commonModule通过import引入、esModule通过require引入，webapck又是怎样打包的，说动手就动手，稍稍的修改下index.js入口文件：

```javascript

import commonModule from './commonModule'
let esModule = require('./esModule')

```

看下有什么不同的：

```javascript

(function(module, __webpack_exports__, __webpack_require__) {

"use strict";
Object.defineProperty(__webpack_exports__, "__esModule", { value: true }); // 1
var __WEBPACK_IMPORTED_MODULE_0__commonModule__ = __webpack_require__(1);  // 2
var __WEBPACK_IMPORTED_MODULE_0__commonModule___default = __webpack_require__.n(__WEBPACK_IMPORTED_MODULE_0__commonModule__);   // 3

var esModule = __webpack_require__(2);   //4 

}),

```

其他的没什么变化，主要就是index.js内的一些变化,commonModule通过import引入变化还挺大的，看看都变了什么，同样在开头添加__esModule标志，主要就是第2、3句的变化了。在这里会通过__webpack_require__.n去取模块的内容，然后var getter = module && module.__esModule ？function getDefault() { return module['default']; } : function getModuleExports() { return module; }; 判断是es模块还是commonjs模块。就是多了一层包装，


**至此webapck模块化的定义以及基本实现就浮出水面了，本质原理是通过一个IIFE函数，把exports和require定义先包揽在里头，然后把各个模块当成参数传递过去。在IIFE函数内里调用整个项目的入口文件，再通过入口文件一一关联到其他的模块，从而形成一个庞大的树形结构。**




