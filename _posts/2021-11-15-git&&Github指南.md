---
layout: post
title:  "git&&Github指南"
date: 2021-11-15
author: htmc
header-img: ./img/post_bg1_blue.png
catalog: true
tags: 
  -git
  github
---

### 什么是git和Github？

git是版本控制工具，Github是基于git的远程代码仓库。开发者可以将本地代码托管到公共平台github上，来与其他开发者线上远程协作。

git和Github的基本功能体现了三个核心概念： commit（提交），repository（仓库），branch（分支）。我们可以通过repository本地或远程托管代码，通过commit将确定的更改提交来完成迭代，通过branch让协作者在其他分支上进行开发，完成后将各个branch合并到主分支上（主分支是成品）。



### 为什么要使用git来管理代码？

在日常开发中，我们会经常对代码进行修改，但有时修改后的程序会出现bug，或是还不如之前的之前的版本。这个时候，我们就希望能有一颗”后悔药“，能让代码返回更改前的状态。

git就是这样一颗”后悔药“，其中留档的commit历史赋予我们随时穿越回过去版本的”超能力“。



### 如何用git管理代码--git的工作流

- #### 使用git管理本地代码



![image-20211115113202004](/img/git_flow.png)

> **git终端命令（右键git Bash或vscode内下拉选择git终端）**

1. 创建新仓库：打开新文件夹，右键`git Here`，打开git终端，执行`git init`建立仓库（该操作会生成一个隐藏的.git文件）
2. 克隆本地仓库：`git clone path`
3. **将文件添加到暂存区**：`git add`<file> (某文件)|`git add -A` (全部文件)
4. **提交改动到本地仓库**：`git commit -m "代码提交信息（如first）"`
5. 以当前分支为基础新建分支：`git checkout -b branchname`
6. 切换到某分支：`git checkout branchname`
7. 删掉特定分支：`git branch -D branchname`
8. 合并分支：`git merge branchname`
9. 查看提交历史 `git log --stat`
10. 将工作区中的文件回滚到保存期前的版本 `git checkout filename`
11. 回滚文件到commit前的版本 `git reset HEAD^n`

> **vscode图形界面操作**
>
> 下载GitLens插件，可以直接看到版本历史和分支，比命令终端方便



- #### 实现git和github之间的代码往来

1. 将本地git仓库与github连接：`git remote add origin <server>(具体代码内容github会在新建仓库时告诉你)`
2. 将本地改动提交到远程仓库：`git push origin master(master是你想推送的任何分支)`

> `git push`总失败，要使用完整的`git push origin master`

3. 获取github远程仓库中的改动 `git pull`

> 执行push和pull操作时会检查领先性，有可能先pull，在本地解决代码冲突，再push，达成两边的一致



------------------------

### github与开源项目

> 开源项目的代码完全公开，对我们而言，可以通过阅读他人的源码学习，也可以在他人代码的基础上进行二次开发，既能减缓学习曲线，也可以避免重复性代码劳动。

* #### 如何寻找开源项目

1. [github trending](https://github.com/trending)(可以筛选编程语言)

2. [掘金](https://juejin.cn/)

3. 相关的开源项目推荐博客

* #### git搜索的特殊技巧（前后缀）

1. awesome.技术名（找与技术相关的百科全书）
2. xxx sample（找例子）
3. xxx starter/boilplate（找空项目框架）
4. xxx tutorial（找教程）

* #### 开源项目中的文件

1. README.md（项目简介和使用方法）
2. license （使用许可）

> MIT等机构的项目可以在保留版权信息的情况下随意使用，但有些项目不是，阅读license有助于规避后续的纠纷。

3. issue（提问与讨论，接近于项目论坛）
4. main，branch，commit（项目的主干、分支与更新）

* #### 下载开源项目

1. 打开用来保存项目的文件夹，右键`git Here`，打开git终端并执行`git clone path（path是从github code-clone复制的http地址）`

2. 也可以在项目页面右上角`Star`收藏或`Fork`到自己的仓库

> 为什么不直接下载ZIP？
>
> 下载为git才能看到开发者的过往操作，可以从中窥见程序迭代的全过程，更有助于学习。
>
> ZIP只有文件，没有记录，不过也可以通过git init创建新仓库，将它完全变成自己的项目。



---------------------------------------

##### 参考资料
[Roger Dudler. git简明指南](https://www.runoob.com/manual/git-guide/)     
[Fengyu. Github新手够用指南](https://www.bilibili.com/video/BV1e541137Tc?from=search&seid=11261212184045141491&spm_id_from=333.337.0.0)     
[Fengyu. 40分钟学会git](https://www.bilibili.com/video/BV1db4y1d79C?spm_id_from=333.999.0.0)       
[菜鸟教程](https://www.runoob.com/w3cnote/git-guide.html)

