---
layout: post
title: JavaScript中this的4种用法
date: 2016/12/12
---


之前对JavaScript中的this的理解只是一知半解,趁着最近比较闲把this的用法给总结下.
一般的来说JavaScript中的this有四种用法,下面按使用方法按照调用方式的不同，分别讨论 this 的含义。

# 作为对象方法调用

在这个时候this就是指的这个方法的上级对象;

```javascript
function Person() {
    this.say = function() {
        console.log(this)
    }
}

var people = new Person();

people.say();

```
这个时候控制台输出的是Person,this指向这个方法的上级对象.

# 作为函数调用

这个时候函数是全局调用的,this就代表全局对象Window。

```javascript
function test() {
    console.log(this);
}

```
控制台输出的是Window,此时this指的是Window 这个全局对象,属于全局性调用的.

# 作为构造函数调用

所谓构造函数，就是通过这个函数生成一个新对象,如果调用正确，this绑定到新创建的对象上.

```javascript
function Person() {
     this.a = 1;
}

var people = new Person();

console.log(people.a)

```
控制台输出的是1.

# apply/call调用

这两个方法异常强大，他们允许切换函数执行的上下文环境（context），即 this 绑定的对象。这个时候this指的是apply或call方法中的第一个参数.

```javascript
function A() {
    this.say = function () {
    	console.log(this.string)
    }
}

function B() {
	this.string = '我是B';
    this.say = function () {
    	console.log(this.string)
    }
}

var a = new A();
var b = new B();

a.say.call(b);
```

控制台输出的是'我是B',此时注意到在A的构造函数中其实是没有string这个变量的,这能更好的说明this绑定的是b的上下文.

在这特别的说明下ES6中箭头函数的this,就是定义时所在的对象，而不是使用时所在的对象。

```javascript
function Timer() {
  this.s1 = 0;
  this.s2 = 0;
  // 箭头函数
  setInterval(() => this.s1++, 1000);
  // 普通函数
  setInterval(function () {
    this.s2++;
  }, 1000);
}

var timer = new Timer();

setTimeout(() => console.log('s1: ', timer.s1), 3100);
setTimeout(() => console.log('s2: ', timer.s2), 3100);
// s1: 3
// s2: 0
```


