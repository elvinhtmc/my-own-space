---
layout: post
title: "JavaScript DOM-CSS基础"
date: 2021-12-31
author: "何短短"
header-img: img/post-bg2-js.jpg
catalog: true
tags: 
  -JavaScript基础
---

#### 基础概念与方法

* **属性**

  文档中的每个元素都是对象，每个对象又有各种属性。有些属性，如`parentNode,previousSibling,Lastchild`告诉我们各节点的位置信息；还有一些属性包含元素本身的信息，如`nodeName`会返回如p之类的字符串。除此之外，文档的每个元素节点还有属性`style`，它包含着元素的样式，将返回一个对象而非简单的字符串。

* **DOM-CSS方法**

  * 寻找样式`element.style.property`

  * 设置样式`element.style.property = value`(value需要放在引号里，否则会解释为变量)

    **！ 通过style获取样式有很大局限性，它不支持获取外部CSS设置的样式，只能返回内嵌样式。**


#### 实例（DOM-CSS适用情况）

一般不用DOM设置样式，用CSS，但有些情况（尤其是有规律地批量添加）可以用DOM增强文档样式。（代码见example.js)

1. **根据元素在节点树中的位置设置样式**（如：通过遍历加粗每个h1后的第一个文段）

   ``````js
   function setnextpara(){
       var headers = document.getElementsByTagName('h1');
       for(i=0;i<headers.length;i++){
       var elem;
       //elem = headers[i].nextSibling;
       elem = getNextElement(headers[i].nextSibling);
       elem.style.fontSize = '1.2em';
       elem.style.fontWeight = 'bold';
   	}
   }
   //此处要的是下一个元素节点，而非下一个节点，需要判定
   function getNextElement(node) {
       if(node.nodeType==1) { //先判定h后是否为元素节点
           return node;
       }
       if(node.nextSibling) {//下一个不是元素节点，看下下个
           return getNextElement(node.nextSibling);
       }
       return null;//如果h1后没有元素节点，返回null
   }
   ``````

   > Notice：
   >
   >  **nextsibling：** 返回元素节点之后的兄弟节点（包括文本节点、注释节点即回车、换行、空格、文本等等），而不仅指元素节点。在本例中，p还包含文本节点，因此nextsibling无法精确指某节点，报错。
   >
   > **nextElementSibling：**属性只返回元素节点之后的兄弟元素节点（不包括文本节点、注释节点），如果在本例中使用，则无需创建getNextElement函数，直接`node.nextElementSibling`即可。

   

2. **根据某种条件反复设置某种样式**（如：表格的斑马线效果，奇数行变色）

   可以直接使用css3中的结构伪类选择器，`E:次序-child`,常用于选父元素的第n个子元素E

   ``````css
   tr:nth-child(odd) {
       background-color: silver;
   }
   ``````

   如果不支持css3（一般不会）,可以在js操作，遍历表格，改变奇数行样式属性。

   ``````js
   function stripeTables() {
       if (!document.getElementsByTagName) return false;
       var tbody = document.getElementsByTagName('tbody');
       var rows;
       rows = tbody[0].getElementsByTagName('tr');
       for (var i = 0; i < rows.length; i++) {
           if (i % 2 == 0) { //奇偶判定用%余数，不能i==odd
               rows[i].style.backgroundColor = 'silver';
           }
       }
   }
   ``````

3. **为对应特定事件的元素赋予样式**

   ``````js
   //响应事件
   //定义变量与遍历，赋予符合条件的元素事件，类似var:hover{}
   function highlightRows() {
   for(i初始判定更新){
   	var[i].onmouseover = function(){
   		this.style.fontWeight = 'bold';//this 指向的是调用这个方法的对象
   	}
      }
   }
   ``````

4. **通过className属性赋予元素样式**

   与其用DOM直接设置样式，让行为层处理表示层的事，不如用DOM设置/修改元素的样式，然后在样式表调整样式属性。

   `element.className = Value` 通过样式名赋予元素样式

   在给元素追加新class时，可以按照以下步骤操作：

   ``````js
   function addClasses(){
       var elem = [获取一个变量];
       addClass(elem,'valuename');//注意这里的引号，值要以引号形式代入
   }
   function addClass(element,value) {
     if (!element.className) { //检查element.className是否是null
       element.className = value;//是则直接赋予className
     } else {
       newClassName = element.className;//不是则和在html书写class一样
       newClassName+= " "; //class="c1[空格]c2"
       newClassName+= value;
       element.className = newClassName;
     }
   }
   ``````

#### bug

1. ##### 对`getElementsByTagName`理解不当

   `getElementsByTagName`理解不当方法返回一个对象数组，每个对象对应文档里有着给定标签的一个元素，一次后续引用元素需要带下标，否则会导致"The nodetype is undefined"。

   对比：`getElementsById`返回唯一元素，`getElementsByTagName`即使只有一个元素也是数组，该元素下标为[0]。

   ``````js
   `var title = document.getElementsByTagName("h1");`
   h1.style.fontSize = 2em;（×）
   //Cannot set properties of undefined(h1是数组，非特定变量) 
   → h1[0].style.fontSize = '2em'（√）//em有引号，视为值

2. ##### 对`nextSibling`理解不清(待解答) 

3. ##### table结构与CSS细节不清

   [MDN table](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/table)(有很多被废弃的属性)

   [MDN 用dom方法创建表格（方法与表格结构的梳理）](https://developer.mozilla.org/zh-CN/docs/Web/API/Document_Object_Model/Traversing_an_HTML_table_with_JavaScript_and_DOM_Interfaces)
 ![img](/img/post-domcss.table].jpg)
 
   * 只有table，th和td有独立的边框；tr没有，css设置无用。這些元素都有bgcolor。

   * html5不支持`cellspacing`，用`border-collapse: collapse`代替

     > separate :　 默认值。边框独立（标准HTML）
     > collapse :　 相邻边被合并

   * 不建议使用align居中，建议设置`margin-left/right`，或`margin：0 auto`

4. ##### ！函数始终不起作用

   * 不小心在开头打了断点

     断点是调试器设置源程序在执行过程中自动进入中断模式的一个标记。当程序运行到断点时，程序中断执行（断点所在的行还没有执行），进入调试状态。通过设置断点可以查找程序运行时的错误，是调试程序常用的手段。

   * 函数失效问题

     ``````js
     addLoadEvents(setnextpara); 
     addLoadEvents(stripeTables);
     addLoadEvents(addClass);
     //内部引用，三个函数只有最后一个能生效；外部引用，只有stripeTables生效
     //debug 2h 未明原因
     ``````

     **内部引用，只有最后一个函数生效**，原因是`window. onload= function`连续使用，后覆盖前。正确用法为：

     ``````js
        function addLoadEvents(func) {
                 var oldonload = window.onload;
                 if (typeof oldonload != 'function') {
                     window.onload = func;
                 } else {//此处需要另设函数function()
                     window.onload = function () {
                         oldonload();
                         func();
                     }
                 }
             }
     /*else{
         window.onload = function1;
         window.onload = function2;
     }(×)*/
     ``````

     外部引用，stripeTables不报错，`stripeTables()`和`addClass()`报错

     > Uncaught ReferenceError: stripeTables / addClass is not defined

     在修改了`addClass()`和之前的`addLoadEvents(func) `后，运行恢复正常，说明可能是函数冲突/错误导致报错，但原因不完全确定。

     **! 添加多个\<script>时，一个js文件的错误可能导致连锁报错，因此一次性运行所有js文件会导致纠错困难。最好一个一个运行（添加断点或转为注释），这样便于纠察js文件内部的错误，而暂时无需考虑文件间的冲突。**

     

5. ##### **其他细节**

   * `element: nth-child(n)`，表示父元素中子元素为第n且名为element的标签，nth-child可以视为第n个element，而非element的第n个子元素.如`tr:nth-child(2n+1/odd)表示HTML表格中的奇数行。`

   * `typeof operand` 非字符串不必加引号

   * 奇偶判定用%余数，不能`i==odd`

   * js变量声明问题 ` newClassName = element.className;`
   
     > Var i=100 显示申明，i=100 隐式申明。
     >
     > 在函数中使用var关键字进行显式申明的变量是做为局部变量，而没有用var关键字，使用直接赋值方式声明的是全局变量。
     >
     > 当我们使用访问一个没有声明的变量时，JS会报错。而当我们给一个没有声明的变量赋值时，JS不会报错，相反它会认为我们是要隐式申明一个全局变量，这一点一定要注意。
     
     
