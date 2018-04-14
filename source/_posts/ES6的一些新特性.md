---
layout: post
title: ES6的一些新特性
date: 2016/12/06
---


### 一些赋值命令

>let:

用来声明变量，但是声明的变量只在let命令所在的代码块内有效

不存在变量提升

>const:

声明一个只读变量，声明之后则不能改变

*let,const,class 声明的变量不属于全局对象属性*

### 变量的解构赋值

* 交换变量的值

```JavaScript
[x,y] = [y,x]
```

* 从函数返回多个值
函数只能返回一个值，如果要返回多个值，只能将它们放在数组或对象里返回。有了解构赋值，取出这些值就非常方便

```JavaScript
// 返回一个数组

function example() {
  return [1, 2, 3];
}
var [a, b, c] = example();

// 返回一个对象

function example() {
  return {
    foo: 1,
    bar: 2
  };
}
var { foo, bar } = example();
```

* 提取JSON数据

```JavaScript
var jsonData = {

  id: 42,

  status: “OK”,

  data: [867, 5309]

};

let { id, status, data: number } = jsonData;
```

* 遍历Map结构

```JavaScript
for (let [key, value] of map)

for (let [, value] of map) //只取value值
```

* 输入模块的指定方法

```javascript
const {
  SourceMapConsumer,SourceNode 
  } = require(“source-map”);
```


### 数值的扩展

Number.isNaN()  ： 检查一个值是否为NaN

Math对象的扩展

Math.trunc  去除一个数的小数部分，返回整数部分

Math.sign   判断一个数到底是正数、负数、还是零(+1,-1,0)

### 数组的扩展

find()  找出第一个符合条件的数组成员  否 undefined

```JavaScript
[1, 5, 10, 15].find(function(value, index, arr) {

  return value > 9;

}) // 10
```

接受三个参数，依次为当前的值、当前的位置和原数组

findIndex()  返回第一个符合条件的数组成员的位置  否 -1

```JavaScript
[1, 5, 10, 15].findIndex(function(value, index, arr) {

  return value > 9;

}) // 2
```

fill()  填充一个数组  会覆盖原来的值

entries()  对键值对的遍历

keys()  对键名的遍历

values()  对键值的遍历

```JavaScript
for (let [index, elem] of [‘a’, ‘b’].entries()) {

  console.log(index, elem);

}
```

### 函数的扩展

* 函数参数的默认值

```JavaScript
function Point(x = 0, y = 0)
```

参数变量是默认声明的，不能用let或const再次声明

函数的length属性  返回没有指定默认值的参数个数

* rest参数

代替arguments参数

* 扩展运算符

扩展运算符（spread）是三个点（…）。它好比rest参数的逆运算，将一个数组转为用逗号分隔的参数序列

用法

1.函数调用

```JavaScript
function push(array, …items) {

  array.push(…items);

}

function add(x, y) {

  return x + y;

}

var numbers = [4, 38];

add(…numbers) // 42
```

2.替代数组的apply方法

由于扩展运算符可以展开数组，所以不再需要apply方法，将数组转为函数的参数了

```JavaScript
// ES5的写法

Math.max.apply(null, [14, 3, 77])

// ES6的写法

Math.max(…[14, 3, 77])

// 等同于

Math.max(14, 3, 77);
```

3.扩展运算符的应用

合并数组

```JavaScript
var newArr = […arr1, …arr2, …arr3]
```

4.箭头函数

如果箭头函数的代码块部分多于一条语句，就要使用大括号将它们括起来，并且使用return语句返回

如果箭头函数直接返回一个对象，必须在对象外面加上括号。

使用注意点

箭头函数有几个使用注意点。

（1）函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。

（2）不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。

（3）不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用Rest参数代替。

（4）不可以使用yield命令，因此箭头函数不能用作Generator函数。

