---
layout: post
title: "JS高级程序设计-1~3章-语言基础"
date: 2022-1-14
author: "何短短"
header-img: img/post-bg2-js.jpg
catalog: true
tags: 
  -JavaScript基础
---


之前学过C语言，也读了《C primer plus》，而js的语法很大程度借鉴了C语言。因此该书前四章只记录重点，并补充C语言知识促进理解。

#### 第一章--什么是JavaScript

#### 什么是ECMAScript

ECMAScript是一种脚本语言的标准化规范，定义了一种脚本语言实现应该包含的内容。 Web浏览器只是ECMAScript实现可能存在的一种宿主环境，它提供ECMAScript的基准实现和环境自身交互必须的扩展（如DOM)。扩展使用ECMAScript核心类型与语法，提供特定于环境的额外功能。其他宿主环境还有服务器端JavaScript平台Node.js。

如果不涉及浏览器，ECMA-262定义了一门语言的**语法/类型/语句/关键字/保留字/操作符/全局对象。**

各家浏览器均以ECMAScript作为自己JavaScript实现的依据，虽然具体实现情况各有不同。

#### 什么是JavaScript

JavaScript 是 ECMAScript 规范的一种实现，它和ECMAScript 基本算同义词，但包含更多内容。

作为一门用来与网页交互的脚本语言，JavaScript包含以下三个组成部分：

* **ECMAScript：**由ECMA-262定义并提供核心功能（核心）

* **DOM（文档对象模型）**：提供与网页内容交互的方法与接口（拓展）

* **BOM（浏览器对象模型）**：提供与浏览器交互的方法与接口（拓展）

#### 第二章-HTML中的JavaScript

#### \<script>元素    
 [MDN\<script>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/script)    
JavaScript是通过\<script>元素插入到HTML页面中的，这个元素可用于嵌入JavaScript代码，也可用于引入保存外部文件中的JavaScript。

在引用多个\<script>时，如果没有使用defer和async属性，浏览器会按照\<script>在页面中出现的顺序依次解释它们。

\<script>元素有8个属性，这里挑重点讲解：

* **src**：引用外部JS，必须将src属性设置为所包含文件的URL。使用了src属性的\<script>元素内部不该包含代码，因为浏览器只会下载并执行脚本文件，忽略行内代码。

* **type**：最好不指定type属性。因为type属性用一个MIME类型字符串标识\<script>内容，如果MIME类型不是受支持的JavaScript类型，则该元素所包含的内容会被当作数据块而不会被浏览器执行。而且新版本的浏览器一般将嵌入的脚本语言默认为JavaScript，因此在编写JavaScript代码时可以省略type属性。

* **async**：可选。立即开始下载脚本，但不能阻止其他页面动作，如下载资源或等待其他脚本加载。标记为async的脚本并不保证能按照出现的次序执行。

* **defer**：可选。脚本立即下载，但延迟到文档完全被解析和显示后再执行。



#### **为什么把\<script>放在\<body>末尾 ?**

如果把\<script>元素放在\<head>，当所有js代码下载、解析和解释完成后，浏览器才能渲染页面，这会导致页面渲染的明显延迟。而放\<body>末尾，用户会感觉页面内容加载更快。

#### \<noscript>元素

指定在浏览器 不支持脚本/对脚本的支持被关闭 时的替代内容，在浏览器支持并开启\<script>脚本时不渲染。一般无需使用该标签，因为现在的浏览器均支持JavaScript。

#### 第三章-语言基础
#### 区分大小写

 ECMAScript中一切（变量、函数名、操作符）都区分大小写。

#### 命名规范

* 函数名称采用驼峰命名法：第一个单词小写，第二个单词首个字母为大写。如系统自带的函数：`parseInt`、`isNaN`

* 对象名称第一个字母大写。如：`Math`、`Number`、`Array`

* 事件多为on开头,并且小写。如：`onclick`、`onload`
* 更多参考：[js开发命名规范](https://www.cnblogs.com/polk6/p/4660195.html)

#### 变量声明(let,const)

ECMAScript6增加[let和const](https://www.cnblogs.com/polk6/p/js-letAndconst.html)，用来更精确地声明作用域和语义。**`const`优先，`let`次之，最好不使用`var`。**

js不同于C，属于弱类型语言，其变量是可用于保存任何类型数据的命名占位符（声明时数据类型可忽略）。js中let和const声明的并不是不同类型的变量，只是指出变量在相关作用域如何存在。

**`var`**            
没有块级作用域，因为变量提升的特性，其声明变量的作用域为整个函数或全局范围。
> 提升（hoisting）：使用var在函数或全局内任何地方声明变量相当于在其内部最顶上声明它，且重复声明会自动合并。

**`let`**                
**块作用域**，其声明的变量作用域范围从声明处一直到当前块级语句(若存在)的结尾，否则会一直延伸到函数结尾(在函数内)或全局结尾。
> **块作用域**：块作用域由最近的一对花括号{}界定，如if{}, while{}, function{}, 单独的代码块。
和var不同，使用let在全局作用域中声明的变量不会成为window对象的属性：

``````js
let age = 26;
console.log(window.age); // undefined
``````

let声明的变量不会在作用域中被提升，因此在let声明之前引用该变量会出现“Referrence Error"，重复声明会出现"Syntax Error"。

let非常适合在循环中声明迭代变量，而使用var声明则容易导致结果出错，不应使用：

``````js
 for (var x = 0; x < 5; x++) {
        setTimeout(function() {
            console.log(x); // => 5 5 5 5 5
        }, 100);
    }//i用var声明，循环变量泄露，在全局范围内有效。所以每一次循环，新的i值都会覆盖旧的，导致最后输出的都是最后一轮的i的值。
 //如果在for后写var x=7，则后覆盖前，输出5个7
    for (let y = 0; y < 5; y++) {
        setTimeout(function() {
            console.log(y); // => 0 1 2 3 4
        }, 100);
    }//用let声明，js会为每个迭代循环声明一个新的迭代变量
``````

**`const`**               
const用于定义常量。行为与let基本相同（块作用域，不提升，不作为window属性），但也有区别。

当用const声明的常量为值类型(e.g. String、Number)时，修改此常量的值会报错；但当声明的常量为引用类型(e.g. Array、Object)时，只可以修改此常量的成员。

赋值为对象的const变量不能再被赋值为其他引用值，但对象的键（属性）不受限制：

``````js
//修改值类型的常量会报错
const x = 1;
x = 2; // => Uncaught TypeError: Assignment to constant variable.
//修改引用类型的常量的成员不会报错
// 1.const声明一个数组
const x = [1, 2, 3];
console.log(x); // => [1, 2, 3]
x[0] = 2; // 修改数组的第一个元素的值
console.log(x); // => [2, 2, 3]
// 2.const声明一个对象
const o1 = {};
o1.name = 'Jake';//(√)
``````

#### 数据类型

ECMAScript变量可以包含两种不同类型的数据：原始值和引用值。

##### 原始值--简单数据类型

> 7种，可使用 typeof 运算符检查，而且彼此之间存在互相转换的方法。无需记忆，在使用时增进了解，具体细节查阅《JavaScript高级程序设计》P30~P56，下面简单介绍。

**1.undefined**<br>声明变量但未初始化，相当于给变量赋”undefined“值。

``````js
//1. 建议声明变量时初始化，这样当typeof返回"undefined"时，可以知道那是因为变量未声明
let age;
console.log(message);//error
console.log(typeof age);//未初始化的变量获得undefined值
console.log(typeof message);//未声明的变量也返回undefined

//2. undefined是假值false
if(age){
    //不执行
}
``````

**2.null<br>**null值表示一个空对象指针，`typeof`返回”Object“。

``````js
//1. 任何时候，只要变量要保存对象，但当时又无那个变量可保存，就用”null“填充变量。
// foo 现在已经是知存在的，但是它没有类型或者是值,可以在后续赋予：
var foo = null;
//2. null是假值
if(foo){
    //不执行
}
//3. 当检测 null 或 undefined 时，注意相等（==）与全等（===）两个操作符的区别 ，前者会执行类型转换：
null === undefined // false
null  == undefined // false==false→true
``````

**3.Boolean<br>**布尔值是使用最频繁的类型之一，有两个字面值：true和false。

虽然布尔值只有两个，但所有其他ECMAScript类型的值都有等价布尔值。理解转换非常重要，因为if等流控制语句会自动执行其他类型值到布尔值的转换：

```js
if(true/非空字符串/非零数值/任意对象){
//执行
}
if(false/空字符串/0、NaN/null/undefined){
//不执行
}
```

**4.Number**        
* **整数**           
``````c
//以C语言为例
//十进制：0~9，直接写
int decimal = 10;
printf("十进制数 %d",decimal);
//八进制，0开头，后面跟0~7，
int octonary = 0123;
printf("八进制数 %o",octonary);
//十六进制，0x开头，后面跟数字0~9或字母A~F（小写也可以）
int hexadecimal = 0x2D;
printf("十六进制数 %x",hexadecimal);
``````

* **浮点数**       
存储浮点值用的内存空间是存储整数值的两倍，所以ECMAScript会设法把值转换为整数（1. 或1.0都会当整数1处理）。                        
下面补充C语言浮点数存储方式：
``````c
|=|===========|===================================================|
|s|-exponent--|--------------------mantissa-----------------------|
//上面的图示中，s(sign)为符号位，占1bit，用来表示整个float/double的正负性；
//中间部分exponent是科学计数法指数部分，即阶码；
//最后的也是最长的一部分mantissa，尾数，它的长度直接影响力浮点数的精度。
``````       
非常大/非常小的浮点数可用科学计数法表示： 120.5可以表示为1.205*10e2，0.01205表示为1.205e-2。ECMAScript可表示的数值范围（多数浏览器）为：5e-324~1.7976931348623157e+308。一般计算不会超出该范围。

* **NaN**               
不是数值，如0/0。可用`isNaN()`测试对象（不常见），先调用`valueof(Object)`返回对象值，再调用`isNaN(ObjectValue)`，数值/可转换为数值的值返回false（如true,"10")，不能转换的（如”blue“）返回true。

**5.String**<br>包裹在单/双/反引号中的字符。
> 字面量：字面量就是指这个量本身，比如字面量3。也就是指3. 再比如 string类型的字面量"ABC", 这个"ABC" 通过字来描述。
字符字面量/模板字面量/字符串插值等内容查书。

**6.BigInt**<br>**7.Symbol**

##### [引用值--Object对象]((2022-1-14-JavaScript高级程序设计-5~6-引用类型.md))

> 点击跳转至后续文章，第5、6章

基本引用类型<br>集合引用类型

#### 操作符

> 和C近似，此处不按书中分类，只列举自己见过、易忘或易混淆的，其余查书P57~P71

递增/减操作符： **“`++`” “`--`”**
``````js
// “++” “--”有前缀版和后缀版，前缀版在语句求值前发生，后缀版在求值后发生
let num1 = 2;
let num2 = 20;
let num3 = num1-- + num2;//2+20
let num4 = num1 + num2;//1+20
let num5 = --num + num2;//0+20
``````

布尔操作符：**"`！`" "`&&`" "`||`"**
``````js
// "!"非，会先将操作数转换为布尔值，再取反
if（!Object) //返回false
//`if（！method）`把不支持某特定方法的浏览器检测出来
if (!document.getElementById) return false;//
////如果浏览器不理解名为getElementById/getElementsByTagName的DOM方法，请离开
//"&&" 和，两者都返回true，才返回true
//”||“或，两者中任意一个返回true，则返回true
``````

乘性操作符：**“`%`“ ”`**`“**
``````js
let result = 23 % 3;//%取模,23/3余2，result=2
let squared = 2**2 //2的2次方等于4
squared ** = 3 //”**=“指数赋值操作符，4的立方
//”+=num“”-=num“用法相同，等同于"+num=""-num=",适用于循环语句
``````

相等/赋值操作符：**"`==`" `"!=`" "`===`""`=`"**
``````js
for (let i=1;i==3;i++){
    //”=“赋值，初始化”="将1赋给i
    //”==”相等，i==3时跳出循环，如果此处写i=3，则循环无出口
}
//if等流控制语句会自动执行其他类型值到布尔值true&false的转换
if("55"==55)//true->true=true
if("55"!=55);//false
if("55"===55)//false->string!=number
if("55"!==55)//true
//"!="不相等    
//"=="相等，两个操作数在转换的前提下相等
//"==="全等，两个操作数在不转换类型的前提下相等(要全等数据类型一定相同)
//"!=="不全等，两个操作数在不转换类型的前提下不相等    
``````

条件操作符：**"`? :`"**
``````js
//variable = boolean_expression ? true_value : false_value
let max = (num1 > num2) ? num1 :num2 //max==num1
``````

逗号操作符：**"`,`"**
``````js
//用逗号操作符同时声明多个变量
let num1 = 1, num2 = 2;
//辅助赋值
let num3 = (5, 4, 3)//num3==3
``````

位操作符：用于数据底层操作，即操作内存中表示数据的比特，这里先不深究。

#### 语句(流控制)

##### `if` & `switch`（条件）

``````js
if(condition) statement1 else statement
//这里的condition可以是任何表达式，并且求值结果不一定是布尔值
//ECMAScript会自动调用Boolean()函数将表达式值转换为布尔值
//如果条件求值为true，执行statement1，false执行2
//if和else间还可以有多个else if
``````

``````js
switch (i) {//switch(expression)
 case 25:
	statement1;
	break;
 case 35:
	statement1;
	break;
 default:
    statement;
}
//每个case（条件分支）相当于：如果表达式等于case后的值，执行相应语句
//如果case后没有break，则会继续匹配,若无一匹配，执行default下的语句
//ECMAScript中的switch可用于所有数据类型，如：
let num = 25;
switch (true) {
 case num>0:
	statement1;//25>0->true，执行s1
	break;
 case num<=0:
	statement1;
	break;
 default:
    statement;
}
``````

##### `do-while` &`while`（循环）

``````js
while(expression) statement;//检测出口
do {
    statement;
} while (expression)//do while循环体在退出前至少执行一次
``````

##### `for`(循环，常用方式)

``````js
for(初始化 ; 出口判定式 ; 更新) statement;
for(let i=0; i>5; i++){
    statement；
    //仅在初次循环初始化i=0，每轮循环结束更新i+1，新一轮开始时重新判定
}
``````

##### ` for-in & for-of`（循环，常用于遍历数组或对象）

参考：[for in 和 for of 的区别](https://juejin.cn/post/6916058482231754765)

它们两者都可以用于遍历，不过`for in`遍历的是数组的索引（`index`），而`for of`遍历的是数组元素值（`value`）。

`for in`总是得到对象的`key`或数组、字符串的下标；`for of`总是得到对象的`value`或数组、字符串的值。

``````js
// for in
var obj = {a:1, b:2, c:3}    
for (let key in obj) {
  console.log(key)
}// a b c
for(const propName in window) {
    document.write(propName); //用for-in 循环显示BOM对象window的所有属性
}
//for of
const array1 = ['a', 'b', 'c']
for (const val of array1) {
  console.log(val)
}// a b c
``````

##### `label`（标签语句）

`lable: statement;`标签语句用于给语句加标签，方便后续通过`break`或`continue`语句引用。常用于Java，JS也可用。

label 是标识代码块的标签。当执行这种形式的 break 语句时，控制权被传递出指定的代码块。被加标签的代码块必须包围 break 语句，但是它不需要直接包围 break 的块。也就是说，可以使用一个加标签的 break 语句来退出一系列的嵌套块，但是不能使用 break 语句将控制权传递到不包含 break 语句的代码块。

用标签（label）可以指定一个代码块，标签可以是任何合法有效的 Java 标识符，后跟一个冒号。加上标签的代码块可以作为 break 语句的对象，使程序在加标签的块的结尾继续执行。

``````js
outermost://标签 outermost 表示的是第一个 for 语句
for (var i=0; i<10; i++) {
  for (var j=0; j<10; j++) {
    if (i == 5 && j == 5) {//statement    
		break outermost;
  }
  iNum++;
  }
}
alert(iNum);	//输出 "55"
``````

##### `break` & `continue`（控制循环）

`break`退出整个循环，`continue`退出本轮循环。

``````js
let num1 = 0,num2 = 0;
for(let i=1; i<10; i++) {
    if(i % 5 == 0);
    	break;//i==5,循环结束
    num1++;//num1 == 4
}
for(let i=1; i<10; i++) {
    if(i % 5 == 0);
    	continue;//i==5,循环回到开头,继续6789
    num2++; //num2 == 8
}
``````

#### [函数](2022-1--JavaScript高级程序设计-10-函数闭包this.md)
> 点击跳转至第10章
函数对任何语言来说都是核心组件，因为它们可以封装语句，然后在任何地方与时间执行。ECMAScript函数使用function关键字声明，后跟一组参数，然后是函数体。

``````js
function function_name( parameter list )
{
   body of the function
}
//和C语言不同，function前无return_type，函数体末尾也不用写return 0；(除非需要给一个函数出口)
``````

ECMAScript函数不需要指定函数的返回值，因为任何函数可以在任何时候返回值。不指定返回值的函数实际上会返回特殊值undefined。
