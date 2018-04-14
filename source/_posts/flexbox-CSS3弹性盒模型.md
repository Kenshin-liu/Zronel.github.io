---
layout: post
title: flexbox-CSS3弹性盒模型
date: 2017/07/09
---


# flexbox

弹性布局（flexible box）模块旨在提供一个更加有效的方式来布置，对齐和分布在容器之间的各项内容，即使它们的大小是未知或者动态变化的。

弹性布局的主要思想是让容器有能力来改变项目的宽度和高度，以填满可用空间（主要是为了容纳所有类型的显示设备和屏幕尺寸）的能力。

最重要的是弹性盒子布局与方向无关，相对于常规的布局（块是垂直和内联水平为基础），很显然，这些工作以及网页设计缺乏灵活性，无法支持大型和复杂的应用程序（特别当它涉及到改变方向，缩放、拉伸和收缩等）。

# 基础知识

由于flexbox是一个整体模块，而不是单一的一个属性，它涉及到了很多东西，包括它的整个属性集。它们之中有一些是在父容器上设置，而有一些则是在子容器上设置。

如果经常布局基于块和内嵌流方向，柔性布局基于“柔性流动方向”。请看看这个数字从规范，解释背后的柔性布局的主要思路。

![image](http://ojg2nfova.bkt.clouddn.com/flexbox.png)


# 属性介绍


## 作用在父元素上的属性

display: flex | inline-flex;

定义一个flex容器，内联或者根据指定的值，来作用于下面的子类容器。

* box：将对象作为弹性伸缩盒显示。（伸缩盒最老版本）（css3）
* inline-box：将对象作为内联块级弹性伸缩盒显示。（伸缩盒最老版本）（CSS3）
* flexbox：将对象作为弹性伸缩盒显示。（伸缩盒过渡版本）（CSS3）
* inline-flexbox：将对象作为内联块级弹性伸缩盒显示。（伸缩盒过渡版本）（CSS3）
* flex：将对象作为弹性伸缩盒显示。（伸缩盒最新版本）（CSS3）
* inline-flex：将对象作为内联块级弹性伸缩盒显示。（伸缩盒最新版本）（CSS3）


flex-direction
定义：设置或检索伸缩盒对象的子元素在父容器中的位置。

```JavaScript
flex-direction: row | row-reverse | column | column-reverse
```

* row：横向从左到右排列（左对齐），默认的排列方式。
* row-reverse：反转横向排列（右对齐，从后往前排，最后一项排在最前面。
* column：纵向排列。
* column-reverse：反转纵向排列，从后往前排，最后一项排在最上面。

flex-wrap</font>
定义：设置或检索伸缩盒对象的子元素超出父容器时是否换行。

```JavaScript
flex-wrap: nowrap | wrap | wrap-reverse
```

![image](http://ojg2nfova.bkt.clouddn.com/flex-wrap.png)

* nowrap：当子元素溢出父容器时不换行。
* wrap：当子元素溢出父容器时自动换行。
* wrap-reverse：反转 wrap 排列。

flex-flow 
定义：复合属性。设置或检索伸缩盒对象的子元素排列方式。

```JavaScript
flex-flow: <‘flex-direction’> || <‘flex-wrap’>
```
* [ flex-direction ]：定义弹性盒子元素的排列方向。
* [ flex-wrap ]：定义弹性盒子元素溢出父容器时是否换行。

justify-content
定义：设置或检索弹性盒子元素在主轴（横轴）方向上的对齐方式

```JavaScript
justify-content: flex-start | flex-end | center | space-between | space-around
```
![image](http://ojg2nfova.bkt.clouddn.com/justify-content.png)

* flex-start：弹性盒子元素将向行起始位置对齐。该行的第一个子元素的主起始位置的边界将与该行的主起始位置的边界对齐，同时所有后续的伸缩盒项目与其前一个项目对齐。
* flex-end：弹性盒子元素将向行结束位置对齐。该行的第一个子元素的主结束位置的边界将与该行的主结束位置的边界对齐，同时所有后续的伸缩盒项目与其前一个项目对齐。
* center：弹性盒子元素将向行中间位置对齐。该行的子元素将相互对齐并在行中居中对齐，同时第一个元素与行的主起始位置的边距等同与最后一个元素与行的主结束位置的边距（如果剩余空间是负数，则保持两端相等长度的溢出）。
* space-between：弹性盒子元素会平均地分布在行里。如果最左边的剩余空间是负数，或该行只有一个子元素，则该值等效于'flex-start'。在其它情况下，第一个元素的边界与行的主起始位置的边界对齐，同时最后一个元素的边界与行的主结束位置的边距对齐，而剩余的伸缩盒项目则平均分布，并确保两两之间的空白空间相等。
* space-around：弹性盒子元素会平均地分布在行里，两端保留子元素与子元素之间间距大小的一半。如果最左边的剩余空间是负数，或该行只有一个伸缩盒项目，则该值等效于'center'。在其它情况下，伸缩盒项目则平均分布，并确保两两之间的空白空间相等，同时第一个元素前的空间以及最后一个元素后的空间为其他空白空间的一半。

align-items
定义：设置或检索弹性盒子元素在侧轴（纵轴）方向上的对齐方式。

```JavaScript
align-items: flex-start | flex-end | center | baseline | stretch
```

![image](http://ojg2nfova.bkt.clouddn.com/align-items.png)

* flex-start：弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴（纵轴）起始边界。
* flex-end：弹性盒子元素的侧轴（纵轴）结束位置的边界紧靠住父容器的侧轴结束边界。
* center：弹性盒子元素在该行的侧轴（纵轴）上居中放置。（如果该行的尺寸小于弹性盒子元素的尺寸，则会向两个方向溢出相同的长度）。
* baseline：如弹性盒子元素的行内轴与侧轴为同一条，则该值与'flex-start'等效。其它情况下，该值将参与基线对齐。
* stretch：如果指定侧轴大小的属性值为'auto'，则其值会使项目的边距盒的尺寸尽可能接近所在行的尺寸，但同时会遵照'min/max-width/height'属性的限制。

align-content
定义：设置或检索弹性盒堆叠伸缩行的对齐方式。

```JavaScript
align-content: flex-start | flex-end | center | space-between | space-around | stretch
```

![image](http://ojg2nfova.bkt.clouddn.com/align-content.png)

* flex-start：各行向弹性盒容器的起始位置堆叠。弹性盒容器中第一行的侧轴起始边界紧靠住该弹性盒容器的侧轴起始边界，之后的每一行都紧靠住前面一行。
* flex-end：各行向弹性盒容器的结束位置堆叠。弹性盒容器中最后一行的侧轴起结束界紧靠住该弹性盒容器的侧轴结束边界，之后的每一行都紧靠住前面一行。
* center：各行向弹性盒容器的中间位置堆叠。各行两两紧靠住同时在弹性盒容器中居中对齐，保持弹性盒容器的侧轴起始内容边界和第一行之间的距离与该容器的侧轴结束内容边界与第最后一行之间的距离相等。（如果剩下的空间是负数，则各行会向两个方向溢出的相等距离。）
* space-between：各行在弹性盒容器中平均分布。如果剩余的空间是负数或弹性盒容器中只有一行，该值等效于'flex-start'。在其它情况下，第一行的侧轴起始边界紧靠住弹性盒容器的侧轴起始内容边界，最后一行的侧轴结束边界紧靠住弹性盒容器的侧轴结束内容边界，剩余的行则按一定方式在弹性盒窗口中排列，以保持两两之间的空间相等。
* space-around：各行在弹性盒容器中平均分布，两端保留子元素与子元素之间间距大小的一半。如果剩余的空间是负数或弹性盒容器中只有一行，该值等效于'center'。在其它情况下，各行会按一定方式在弹性盒容器中排列，以保持两两之间的空间相等，同时第一行前面及最后一行后面的空间是其他空间的一半。
* stretch：各行将会伸展以占用剩余的空间。如果剩余的空间是负数，该值等效于'flex-start'。在其它情况下，剩余空间被所有行平分，以扩大它们的侧轴尺寸。

## 作用在子元素的属性

order
定义：各个子元素的排序

```JavaScript
order: <integer>
```

* \<integer>：用整数值来定义排列顺序，数值小的排在前面。可以为负值。

flex-grow
定义：设置或检索弹性盒的扩展比率。根据弹性盒子元素所设置的扩展因子作为比率来分配剩余空间。

```JavaScript
flex-grow: <number> (default 0)
```

* \<number>：用数值来定义扩展比率。不允许负值
* flex-grow的默认值为0，如果没有显示定义该属性，是不会拥有分配剩余空间权利的。

flex-shrink
定义：设置或检索弹性盒的收缩比率（根据弹性盒子元素所设置的收缩因子作为比率来收缩空间。）

```JavaScript
flex-shrink: <number> (default 1)
```

* flex-shrink的默认值为1，如果没有显示定义该属性，将会自动按照默认值1在所有因子相加之后计算比率来进行空间收缩。

flex-basis 
定义：设置或检索弹性盒伸缩基准值。

```JavaScript
flex-basis: <length> | auto (default auto)
```

* auto：无特定宽度值，取决于其它属性值
* \<length>：用长度值来定义宽度。不允许负值
* \<percentage>：用百分比来定义宽度。不允许负值

flex
定义：复合属性。设置或检索伸缩盒对象的子元素如何分配空间。

```JavaScript
flex：none | [ flex-grow ] || [ flex-shrink ] || [ flex-basis ]
```

* none：none关键字的计算值为: 0 0 auto
* [ flex-grow ]：定义弹性盒子元素的扩展比率。
* [ flex-shrink ]：定义弹性盒子元素的收缩比率。
* [ flex-basis ]：定义弹性盒子元素的默认基准值。

align-self
定义：设置或检索弹性盒子元素自身在侧轴（纵轴）方向上的对齐方式。

```JavaScript
align-self: auto | flex-start | flex-end | center | baseline | stretch
```
* auto：如果'align-self'的值为'auto'，则其计算值为元素的父元素的'align-items'值，如果其没有父元素，则计算值为'stretch'。
* flex-start：弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴起始边界。
* flex-end：弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴结束边界。
* center：弹性盒子元素在该行的侧轴（纵轴）上居中放置。（如果该行的尺寸小于弹性盒子元素的尺寸，则会向两个方向溢出相同的长度）。
* baseline：如弹性盒子元素的行内轴与侧轴为同一条，则该值与'flex-start'等效。其它情况下，该值将参与基线对齐。
* stretch：如果指定侧轴大小的属性值为'auto'，则其值会使项目的边距盒的尺寸尽可能接近所在行的尺寸，但同时会遵照'min/max-width/height'属性的限制。



