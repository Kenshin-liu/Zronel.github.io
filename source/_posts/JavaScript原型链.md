---
layout: post
title: JavaScript原型链
date: 2017/06/30
---


# 原型对象（prototype）

无论什么时候，只要创建了一个新函数，就会根据一组特定的规则为该函数创建一个prototype属性，这个属性指向函数的原型对象，在默认情况下，所有原型对象都会自动获得一个constructor属性，这个属性是一个指向prototype属性所在函数的指针。例如

```javascript
function Person() {
    ...
}

Person.prototype.constructor = Person
```
而通过这个构造函数，我们还可以继续为原型对象添加其他属性跟方法。

```javascript
function Person() {
}

Person.prototype.name = 'Nichlas'
Person.prototype.age = '29'
Person.prototype.jod = 'Software Engineer'
Person.prototype.sayName = function () {
    console.log(this.name)
}

person1 = new Person()

```
![image](http://ojg2nfova.bkt.clouddn.com/A9CDC563-2D63-4CB0-A484-3998E9AB21A9.png)


### prototype属性

​ prototype存在于构造函数中，他指向了这个构造函数的原型对象

### [[prototype]]属性

存在于实例中，指向构造函数的prototype，在Firefox Safair Chome中可通过__prototype__访问，但是在其他浏览器实现中对脚本是不可见的，慎用

不过能通过个函数访问到他Object.getPrototypeOf(),在支持的实现中他返回实例的[[prototype]]

```javascript
cosnole.log(Object.getPrototypeOf(person1).age)

//29
```

### constructor属性

constructor属性存在于原型对象中，他指向了构造函数

```javascript
Person.prototype.constructor = Person
```

### hasOwnProperty方法

hasOwnProperty()函数用于指示一个对象自身(不包括原型链)是否具有指定名称的属性。如果有，返回true，否则返回false。

该方法属于Object对象，由于所有的对象都"继承"了Object的对象实例，因此几乎所有的实例对象都可以使用该方法。

```javascript
person2 = new Person()
person2.name = 'liudiguang'

cosnole.log(person2.hasOwnProperty("name"))
cosnole.log(person2.hasOwnProperty("age"))

//ture
//false
```
在这里虽然name是原型上的属性但是在person2中从新给name赋了次值所以在这name属于自身的恶属性而不是原型链上的

### in操作符

​ in操作符用来判断一个属性是否存在于这个对象中。但是在查找这个属性时候，现在对象本身中找，如果对象找不到再去原型中找。换句话说，只要对象和原型中有一个地方存在这个属性，就返回true

```javascript
person2 = new Person()
person2.name = 'liudiguang'

cosnole.log("name" in person2)
cosnole.log("age" in person2)

//ture
//ture
```


# 原型链

JS在创建对象（不论是普通对象还是函数对象）的时候，都有一个叫做__proto__的内置属性，用于指向创建它的函数对象的原型对象prototype。以上面的例子为例：

```javascript
  console.log(zjh.__proto__ === person.prototype) //true
```

同样，person.prototype对象也有__proto__属性，它指向创建它的函数对象（Object）的prototype

```javascript
  console.log(person.prototype.__proto__ === Object.prototype) //true 
```

继续，Object.prototype对象也有__proto__属性，但它比较特殊，为null

```javascript
  console.log(Object.prototype.__proto__) //null
```

我们把这个有__proto__串起来的直到Object.prototype.__proto__为null的链叫做原型链

用一张图来描述原型链如下：
![image](http://ojg2nfova.bkt.clouddn.com/574093-c03529e3f0943633.jpg)

