---
layout: post
title: "JavaScript DOM-CSS基础"
date: 2022-1-5
author: "何短短"
header-img: img/post-bg2-js.jpg
catalog: true
tags: 
  -JavaScript基础
---


JavaScript DOM-动画基础

#### 基础概念与方法

##### **position**

position属性值：static（默认）, fixed（相对于可视窗口）, relative（相对该元素自身）, absolute（相对父容器）

获取元素位置：`element.style.position`

设置元素位置：`element.style.position = "position属性值”；`（再设置left/right或top/bottom，为避免冲突，在x,y均只选一个方位，如left和top)

**time**

设置时间：`setTimeout("function",interval)`（function是所要执行函数的名字；interval是数值，多少毫秒以后执行该函数）

在function中设置样式：`element.style.property = value` (value需要放在引号里，否则会解释为变量)

清除积累在setTimeout队列中的事件，防止操作太快导致动画滞后：`clearTimeout(variable)`

``````js
//e.g.
var ani = setTimeout("moveMessage()",interval);
clearTimeout(ani)
``````

#### moveElement动画函数

``````js
function moveElement(elementID,final_x,final_y,interval) {//引号问题
    if (!document.getElementById) return false;
    if (!document.getElementById(elementID)) return false;
    var elem = document.getElementById(elementID);
    var ypos = parseInt(elem.style.top);//parseInt函数提取数值信息
    var xpos = parseInt(elem.style.left);//1.获取元素位置
    if (xpos == final_x && ypos == final_y) return true;
    //2.设置递归出口
    if (xpos < final_x) xpos++;//3.在未到达最终位置时让位置逐渐改变
    if (xpos > final_x) xpos--;
    if (ypos < final_y) ypos++;
    if (ypos > final_y) ypos--;
    elem.style.left = xpos + 'vw';//给变化后的数值添加单位
    elem.style.top = ypos + 'vh';
    var repeat="moveElement('"+elementID+"',"+final_x+","+final_y+","+interval+")";//将函数赋给repeat变量，组合字符串，方便后续调用函数
    movement = setTimeout(repeat, interval);
    //递归，调用函数自己，直到函数达成xy条件，通过return true结束
} 
``````

在调用`moveElement`函数时，要注意id名加引号。（在函数中`elementID`无需加引号）

``````js
function positionMessage() {
	 moveElement('image',20,40,50);
}
``````

#### 动画实例-mySlide

要求：当鼠标悬浮到文字上方，下方图片如幻灯片一般滑动，变更到对应的图片。

一开始想用css动画解决，但发现目前所学的css主要是元素自身响应（即一个元素触发某事件后，动画在该元素上发生，如a:hover{animation}），而我们需要的时让发生在某元素上的事件引起零一元素的变化，非自身响应，因此此处采用js动画。

**符合直觉的方法**：像之前的图片库项目一样，四张图片。获取文字选项对应的\<a>元素节点，为每个节点添加onmouseover事件，对应不同动画。但该方法每次更换选项都要重新加载新图片，在网络不好时，可能无法立即响应。

**更好的方法：**为所有选项生成一张集体照，设置一个用以显示部分图片的div容器，当鼠标悬停在某选项时，让图片对应部分移动到供显示的容器中。我们选用这一方法。

##### 1.为div的添加overflow样式属性

css的overflow属性用于处理一个容器内容溢出的情况（元素尺寸超出容器尺寸）。

可取值有四种：

* visible：全部内容可见，溢出部分呈现在容器外。
* hidden：隐藏溢出内容，只有在容器内的部分可见。
* scroll：隐藏溢出内容，但可以通过滚动条调整所见区域。
* auto：默认，类似scroll，只在溢出时显示滚动条。

幻灯片效果选用hidden，通过移动图片调整容器内显示的区域。

``````css
#slideshow {
    position:relative;
    width: 100px;/* 1/4图片宽 */
    height: 100px;/* 不能少,否则图片不可见 */
    overflow: hidden;
}
``````

##### 2.使用moveElement函数为每个链接的onmouseover事件添加动画效果

``````js
var list = document.getElementById("linklist");
  var links = list.getElementsByTagName('a');
  links[0].onmouseover = function () {
    moveElement("preview", -100, 0, 10);
  }//后面省略，依次添加，moveElement参数final_x（-100)根据具体情况调整
``````

##### 3.改进动画效果

* **改进动画滞后现象!**

`moveElement`函数中的 `movement = setTimeout(repeat, interval)`将movement变量声明为全局变量，如果鼠标移动速度够快，积累在`setTimeout`队列中的事件会导致动画效果滞后，类似拔河般左右晃动。可以在函数开头用`clearTimeout(movement)`清除积累的事件。但此处不能用全局`movement`，也不能用局部`var movement`(局部变量movement在clearTimeout上下文不存在，无法工作）,需要一个只与正在被移动的元素有关的变量，即“属性”。

` elem.movement = setTimeout(repeat, interval);`，不管函数正在移动哪个元素，它都将获得一个movement属性，即使快速移动鼠标，实际执行的也只有一条 `setTimeout(repeat, interval)`。

``````js
function moveElement(elementID,final_x,final_y,interval) {
    if (!document.getElementById(elementID)) return false;
    var elem = document.getElementById(elementID);
    //clearTimeout(elem.movement);
    if(elem.movement){
    clearTimeout(elem.movement);
 	 } //修改
   //中间省略
    var repeat="moveElement('"+elementID+"',"+final_x+","+final_y+","+interval+")";
    //movement = setTimeout(repeat, interval);
    elem.movement = setTimeout(repeat, interval);//修改
}

``````

* **改变移动速度**

``````js
//效果：平滑，越接近目的地越慢
//dist是元素与目的地距离的1/10,即函数每次移动剩余间距的1/10，再通过Math.ceil(number)向大于方向取最近的整数。当（final_x - xpos）/10<1,取1保证元素能移动到目的地。
var xpos = parseInt(elem.style.left);
  var ypos = parseInt(elem.style.top);
  if (xpos == final_x && ypos == final_y) {
    return true;
  }
  if (xpos < final_x) {
    var dist = Math.ceil((final_x - xpos)/10);
    xpos = xpos + dist;//xpos++变为新的计算方法
  }
  if (xpos > final_x) {
    var dist = Math.ceil((xpos - final_x)/10);
    xpos = xpos - dist;
  }
  if (ypos < final_y) {
    var dist = Math.ceil((final_y - ypos)/10);
    ypos = ypos + dist;
  }
  if (ypos > final_y) {
    var dist = Math.ceil((ypos - final_y)/10);
    ypos = ypos - dist;
  }
``````

#### 注意

##### 引号问题

在编写函数时，最好把需要带引号的参数（如id和tag）看成一个整体，即`elementID='idName'`，这样做，当参数出现在函数中时就无需重写引号，`document.getElementById(elementID)`。

``````js
function moveElement('elementID',final_x,final_y,interval) (×)
//形参带引号，后续使用elementID也要引号，这容易导致引号错误

function moveElement(elementID,final_x,final_y,interval) （√）
var elem = document.getElementById(elementID);
moveElement('image',20,40,50);//调用函数，名字带引号
``````

```js
//此处引号也易出错
repeat="moveElement('"+elementID+"',"+final_x+","+final_y+","+interval+")"
```

##### 子绝父相

子元素绝对定位，不影响兄弟盒子；父盒子相对定位，占有位置，不影响页面布局，也保证子盒子在内部偏移。通过使用值relative，子元素的坐标(0,0)将固定在容器左上角。

在本例中，div的position为relative是必须的，因为我们想让子图片使用绝对位置。

##### 作用域问题

后续分解



