---
layout: post
title:  "git解决merge&push冲突"
date: 2022-1-6
author: "何短短"
header-img: img/post_bg1_blue.png
catalog: true
tags: 
  -git&github
---

### git 分支与合并相关命令

``````terminal
git branch                # 显示所有本地分支
git checkout <branch/tag> # 切换到指定分支或标签*
git branch <new-branch>   # 创建新分支*
git branch -d <branch>    # 删除本地分支*
git checkout dev          # 合并特定的commit到dev分支上
``````

``````terminal
git merge <branch>        # 合并指定分支到当前分支*
git merge --abort         # 取消当前合并，重建合并前状态*
git merge dev -Xtheirs    # 以合并dev分支到当前分支，有冲突则以dev分支为准
git rebase <branch>       # 衍合指定分支到当前分支
``````

### git merge解决合并冲突

git 代码创建分支让不同的人和不同的开发过程隔离，最后通过merge汇聚到主分支。主分支是一个完成态，是一切的起点和终点，一切分支最好从主分支产生。

merge汇总时，不同分支的代码可能存在冲突，需要调整为唯一版本，下文介绍解决merge冲突的方法。



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

