---
layout: post
title: "JavaScript DOM-CSS基础"
date: 2022-1-8
author: "何短短"
header-img: img/post-bg2-js.jpg
catalog: true
tags: 
  -JavaScript基础
---

### html5简介

HTML5是HTML当前的新标准。随着HTML5到来，网页的结构层、样式层和行为层（还有浏览器中的JavaScript API）已经被整装到一个小集合中，其中包含不少新变化。

* 结构层：新标记元素，如\<section>\<header>，媒体元素**\<canvas>\<audio>\<video>**。
* 行为层：新交互方式与新API，如自定义\<video>控件。
* 表现层：高级选择器与渐变动画等。

HTML5推荐工具，开源JS库 [Modernizr](https://modernizr.com/)

**Modernizr** 是一个 JavaScript 库，用于检测用户浏览器的 HTML5 与 CSS3 特性，使你可以方便地为各种情况编写 JavaScript 和 CSS，无论浏览器是否支持这些特性。这是处理渐进增强的完美方案。Modernizr 会在页面加载后立即检测特性；然后创建一个包含检测结果的 JavaScript 对象，同时在 `html` 元素加入方便你调整 CSS 的 class 名。

使用方式：下载后将脚本放在\<head>中：`<script src="modernizr-1.5.min.js></script>"`，并给\<html>添加no-js类 `<html class="no-js">`

### 示例

#### video&audio

[MDN \<video>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video)

像movie.mp4这样的视频，其实是一个包含很多东西的容器，这个容器规定了不同音频和视频轨道在文件中的位置，以及其他与回放那个相关的特性。在每个影片容器中，视频和音频轨道需使用不同编解码器访问，编解码器决定了浏览器在播放时要如何解码音频和视频。编解码器的核心是一个算法，用于压缩和存储视频，以减小原始文件的大小，并尽可能不损失品质。视频编解码器有多种，代表的有三个：H.264、Theora和VP8，音频常见的编解码器是mp3、aac和ogg。

没有一种浏览器支持所有容器和编解码器，所以浏览器无法播放所有格式的音频和视频，且不同浏览器支持的格式存在差异。为保证每个人都能看见视频，需要在`<source>`元素里提供多个视频源，然后浏览器将会使用它所支持的第一个源，第一个不行则会依次尝试后备格式。实例：

```html
<video controls>
    <!--<video>和<audio>的controls属性告诉浏览器启动内置播放器（有可控制的进度条）autoplay自动播放，loop视频结束后自动重放。controls可和autoplay同时使用，即打开窗口自动就播放，但能控制，类似b站。controls是一个布尔值属性，这意味着它不需要一个值，标签存在即开启设置。-->
  <source src="myVideo.mp4" type="video/mp4">
  <source src="myVideo.webm" type="video/webm">
  <source src="myVideo.ogv" type="video/ogg">
   <!--Type属性：用于说明src属性指定媒体的类型，帮助浏览器在获取媒体前判断是否支持此类别的媒体格式。-->
  <p>Your browser doesn't support HTML5 video. Here is a <a href="myVideo.mp4">link to the video</a> instead.</p>
    <!--都不支持的补救措施-->
</video>
```

controls属性告诉浏览器启动内置播放器，这些标准播放控件与浏览器样式统一，如果想要自定义控件的外观，可以通过一些DOM属性实现，此处不多说，感兴趣可以翻看《DOM编程艺术》P211。

#### form

在HTML5当中，新的输入控件类型包括：`email /url /date /number /range(滑动条) /search(搜索框)/tel(电话)/color`，新的属性包括：`autocomplete(为文本输入框添加一组建议输入项）/ autofocus(让表单元素自动获得焦点) /placeholder(在文本输入框显示临时性提示) /required(必填)等。`实例：

``````html
<input type="email" placeholder="please input your email!"/>
``````

为应对不兼容的浏览器，需用特性检测准备另一个方案。若浏览器不支持新属性，如placeholder，得编写DOM脚本实现同样的功能。具体方法参考《DOM编程艺术》P217~P218。



