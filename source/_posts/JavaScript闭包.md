---
layout: post
title: JavaScript闭包
date: 2017/01/07
---


# 什么是闭包

闭包,官方对闭包的解释是：是指有权访问另外一个函数作用域中的变量的函数。创建闭包的常见方式就是在一个函数内部创建另外一个函数。

在javascript中没有块级作用域，一般为了给某个函数申明一些只有该函数才能使用的局部变量时，我们就会用到闭包，这样我们可以很大程度上减少全局作用域中的变量，净化全局作用域。

 使用闭包有如上的好处，当然这样的好处是需要付出代价的，代价就是内存的占用。
 
 每个函数的执行，都会创建一个与该函数相关的函数执行环境，或者说是函数执行上下文。这个执行上下文中有一个属性 scope chain（作用域链指针），这个指针指向一个作用域链结构，作用域链中的指针又都指向各个作用域对应的活动对象。正常情况，一个函数在调用开始执行时创建这个函数执行上下文及相应的作用域链，在函数执行结束后释放函数执行上下文及相应作用域链所占的空间。
       　　
# 变量的作用域

要理解闭包，首先必须理解Javascript特殊的变量作用域。
变量的作用域有两种,全局变量,局部变量

```javascript
var a = '我是全局变量'
function f() {
    alert(a);
}

```
在这里a是属于全局变量

```javascript
function f() {
var a = '我是局部变量'
    alert(a);
}

```
在这里a是属于局部变量

当我们需要从一个函数的外部读取它局部变量的时候就需要用到闭包。

```javascript
function f1() {
    var n=999;
　　　　function f2(){
　　　　　　alert(n); // 999
　　　　}
}

```

也可以这样:

```javascript
function f1() {
    var n=999;
　　 function f2(){
　　　　alert(n); 
　　　}
　　return f2;　
}

var result=f1();
result(); // 999

```
在这里我们是把f2直接当做返回结果赋值给了result(),这样我们就可以在f1外部读取它的内部变量了。

# Javascript闭包的用途 

通过使用闭包，我们可以做很多事情。比如模拟面向对象的代码风格；更优雅，更简洁的表达出代码；在某些方面提升代码的执行效率。

1、匿名自执行函数

```javascript
var data= {    
    table : [],    
    tree : {}    
};    
     
(function(dm){    
    for(var i = 0; i < dm.table.rows; i++){    
       var row = dm.table.rows[i];    
       for(var j = 0; j < row.cells; i++){    
           drawCell(i, j);    
       }    
    }    
       
})(data); 
```

我们创建了一个匿名的函数，并立即执行它，由于外部无法引用它内部的变量，因此在函数执行完后会立刻释放资源，关键是不污染全局对象。

2、结果缓存

我们开发中会碰到很多情况，设想我们有一个处理过程很耗时的函数对象，每次调用都会花费很长时间。

```javascript
var CachedSearchBox = (function(){    
    var cache = {},    
       count = [];    
    return {    
       attachSearchBox : function(dsid){    
           if(dsid in cache){//如果结果在缓存中    
              return cache[dsid];//直接返回缓存中的对象    
           }    
           var fsb = new uikit.webctrl.SearchBox(dsid);//新建    
           cache[dsid] = fsb;//更新缓存    
           if(count.length > 100){//保正缓存的大小<=100    
              delete cache[count.shift()];    
           }    
           return fsb;          
       },    
     
       clearSearchBox : function(dsid){    
           if(dsid in cache){    
              cache[dsid].clearSelection();      
           }    
       }    
    };    
})();    
     
CachedSearchBox.attachSearchBox("input");    
```
这样我们在第二次调用的时候，就会从缓存中读取到该对象。

3.封装

```javascript
var person = function(){    
    //变量作用域为函数内部，外部无法访问    
    var name = "default";       
       
    return {    
       getName : function(){    
           return name;    
       },    
       setName : function(newName){    
           name = newName;    
       }    
    }    
}();    
     
print(person.name);//直接访问，结果为undefined    
print(person.getName());    
person.setName("abruzzi");    
print(person.getName());    
   
得到结果如下：  
   
undefined  
default  
abruzzi
```


