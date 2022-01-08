---
layout: post
title: "JavaScript DOM基础--图片库项目优化篇（一）"
date: 2021-11-27
author: "何短短"
header-img: img/post-bg2-js.jpg
catalog: true
tags: 
  -JavaScript
---
#### 项目简介（imggalleryProject）

该图片库项目”imagegallery来自《JavaScript DOM编程艺术》，通过它来巩固DOM基础，学习网页制作规范。

**项目要求**：<br>
网页结构：图片选项、图片占位符与图片简介          
网页交互：点击选项即可在占位符位置更换图片与图片简介，停留在原页面，链接不跳转          
规范: 1. 分离CSS与样式 2. 考虑浏览器兼容 3. 考虑性能

#### 完整JS代码
``````javascript
//事件函数
function showPic(whichpic) {
    if (!document.getElementById("placeholder")) return true;
    if (!document.getElementById("description")) return false;
    var source = whichpic.getAttribute("href");
    var placeholder = document.getElementById("placeholder");
    placeholder.setAttribute("src",source);
    if (whichpic.getAttribute("title")) {
      var text = whichpic.getAttribute("title");
    } else {
      var text = "";
    }
    var description = document.getElementById("description");
    if (description.firstChild.nodeType == 3) {
      description.firstChild.nodeValue = text;
    }
    return false;
  }
//测试检查
function prepareGallery() {
    if (!document.getElementById) return false;
    if (!document.getElementsByTagName) return false;
     //前两项是普遍适用性测试，检查浏览器能否理解dom操作，保证不理解的老浏览器不会执行该函数
    if (!document.getElementById("imagegallery")) return false;
    //第三项检查列表元素是否存在，不存在则无必要执行函数
    var gallery = document.getElementById("imagegallery"); //简化
    var links = gallery.getElementsByTagName("a");
    for (var i = 0; i < links.length; i++) {
        links[i].onclick = function () {
            showPic(this);
            return false;//禁止默认跳转行为
        }
        //function(）匿名函数，内部所有语句在links元素链接被点击时执行
        links[i].onkeypress = links[i].onclick; //onkeypress容易出错，用onclick即可包含其大部分功能
    }
}
//必须让js代码在html文档全部加载到浏览器后立马执行（太早太晚DOM都可能不完整）
function addLoadEvent(func) {
    var oldonload = window.onload;//简化
    if (typeof window.onload != "function") {
        window.onload = func;
    } else {
        window.onload = function () {
            oldonload();
            func();
        }
    }
}
addLoadEvent(prepareGallery);
    //创建执行函数的队列
``````

#### 项目优化

##### 1. 渐进增强，平稳退化

我们在浏览器中看到的网页其实是由以下三层信息构成的一个共同体：
- 结构层：由html之类的标记语言创建，由标签对页面内容的含义做出描述。
- 表示层：由CSS负责完成，描述页面内容的呈现效果。
- 行为层：由JS和DOM主宰，负责内容该如何响应事件。
”渐进增强“意为用额外的信息层包裹原始数据（html结构层），CSS负责提供"表示"信息，JS负责提供“行为”信息。按照“渐进增强原则创建出的网页几乎都符合”平稳退化“原则，即，虽然浏览器不支持CSS/JS，最基本的操作依然可以完成。在本例中，我们通过分离JS（内含事件处理函数）和CSS来优化网页。

**分离JS和CSS**
分离表示层：` <head><link rel="stylesheet" href="gallery.css" type="text/css"></head>`     
分离行为层：`<body> <script src="gallery.js" type="text/javascript"></script></body>`放在body末尾

**分离事件处理函数**
事件处理函数添加给某个元素，可在特定事件发生时调用特定的JS代码，在html中语法如下：  
`<lable event = "JavaScript statement(s)"></lable> //JavaScript statement(s)可以是JS中的自定义函数`  

类似给元素添加style属性，在html文档中使用onclick之类的属性既没有效率又容易引发问题，不如将事件分离出去。要将内嵌的事件处理函数分离到外部JS文件，关键在于确定这个事件的元素。

对于一个元素，可以利用class或id属性来解决：  
`element.event = action → getElementById(id).event = action`

对于多个元素，具体步骤如注释1~3： 

``````javascript
//涉及多个元素，把事件添加到具有特定属性的数组上
var links = gallery.getElementsByTagName("a");  //1.把文档里的所有链接放入一个数组
    for (var i = 0; i < links.length; i++) {    //2.遍历数组
        links[i].onclick = function () {     //3.为满足条件的元素添加事件，并在发生行为时调用函数
            showPic(this);
            return false;//禁止默认的跳转行为
        }
        //function(）匿名函数，内部所有语句在links元素链接被点击时执行
        links[i].onkeypress = links[i].onclick;
    }
}
``````

为了保证DOM的完整性，必须让这些代码在HTML全部加载到浏览器后马上开始执行，即把函数添加到windows对象的onload事件上去。下面的addLoadEvent函数是很好的方案，不管我们打算在页面加载完毕后执行多少个函数，它都可以应对自如，只需将页面加载后执行函数的名字添加到参数。具体步骤如下：

``````javascript
function addLoadEvent(func) {
    var oldonload = window.onload;//1. 把现有window.onload的值存入变量，简化
    if (typeof window.onload != "function") {
        window.onload = func; //2. 如果这个函数上没绑定任何函数，就为事件（加载页面）添加新函数
    } else {
        window.onload = function () { //3. 如果已经绑定了一些函数，就把新函数追加到事件的末尾
            oldonload();
            func();
        }
    }
}
addLoadEvent（firstfunction）;
addLoadEvent（secondfunction）;//将函数添加到执行队列
``````

##### 2. 对象检测，向后兼容

不同浏览器对JS支持程度不同，绝大多数浏览器对DOM支持都不错，但较古老的浏览器可能无法理解DOM提供的方法与属性。针对这一问题的最简解决方案式：检测浏览器对JS的支持程度。几乎所有东西（包括各种方法）都可以视作对象，我们可以通过`if（！method）`把不支持某特定方法的浏览器检测出来。如通用检测：

``````javascript
 if (!document.getElementById) return false;
 if (!document.getElementsByTagName) return false;
//如果浏览器不理解名为getElementById/getElementsByTagName的DOM方法，请离开
``````

又如针对性测试 `if (!document.getElementById("imagegallery")) return false;`  
即使将来该imagegallery表单被删除，也不必担心网页的JS代码出错。

##### 3. 性能考虑

* 尽量少访问DOM和尽量减少标记

  只要查询DOM中的某些元素，浏览器都会搜索整个DOM树，从中查找可能匹配的元素。因此最好把第一次查找的结果保存在一个变量中，然后再循环里复用，而不是每次都写完整的`document.getElementsByTagName("a")`

* 压缩脚本

  JavaScript 代码压缩是指去除源代码里的所有不必要的字节，如删除空格与注释，从而达到压缩文件的目的。
- 工具：[JavaScript Minifier](https://www.toptal.com/developers/javascript-minifier/)



#### 细节补充

* **return**

  总的来说在js中对于return用法的三种情况的总结如下：

  - `return true`； 返回正确的处理结果。

  - `return false`；

    1. 返回错误的处理结果 

    2. 终止处理             

       函数应该只有一个出口和入口，但在实际工作中，拘泥这项原则往往会导致代码难以阅读。函数可以有多个出口，只要这些出口集中在函数的开头部分，就是可以接受的。常见于if的多层嵌套。

    3. 阻止提交表单 

    4. 阻止执行默认的行为

       常见于`onclick = "function(); return false;"`

  - `return`；把控制权返回给页面。
 <br>
 <br>
* **文本节点与元素节点之间的关系**
  ​	<img src="/img/post-imagegallery-dom.png" width="500px">

  文本节点和属性节点总被包含在元素节点内，是元素节点的子节点，可以通过`childNodes[n]`查找与调用。在这个练习中，p元素本身的nodeValue为空值，其子节点（文本节点）的值才对应文本内容,`description.childnodes[0].nodeValue=text`。

  #### 思索

  畏难无用，慢慢梳理总能明白每一步是怎么回事，模仿也是学习方法的一种。              




