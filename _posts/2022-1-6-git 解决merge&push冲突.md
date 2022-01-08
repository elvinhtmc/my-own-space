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

> 下面的<>在实际命令中不写，该符号表示分支/标签。实际写为git checkout main

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

在vscode中打开存在冲突的文件，你会看到不同颜色标注的代码，绿色的current change和蓝色的incoming change。点击上方小字“accept incoming change”即为接受新更改的代码，点击“accept current change”即为采用当前代码。

如果觉得两者均有可取之处，需要点击小字“accept both changes”，保留两种代码，手动修改。

![image-20220107171804725](/img/post-gitmerge-smalltext.png)

如果明确有一方的代码都正确，可以在“source control”侧边栏“merge changes”右键目标文件，选择“accept all current"或"accept all incoming",选择只采用一方的代码。

<img src ="/img/post-gitmerge-sidecolumn.jpg" width="300px" align="left" ><br>

如果是别人的代码，实在不知道如何解决冲突，`可以git merge--abort`放弃合并，去找能处理该冲突的人，硬改可能会出大错。

> **补充：计算机词汇stage：暂存区（Stage 或 Index），stage changes将更改提交到暂存区，相当于命令git add**

解决冲突后，重新提交文件。

### 补充：git文件的四种状态

![img](/img/post-gitmerge-fourstatus.png)

**git库所在的文件夹中的文件大致有4种状态：**

- Untracked: 未跟踪, 此文件在文件夹中, 但并没有加入到git库, 不参与版本控制. 通过`git add` 状态变为`Staged`
- Unmodify: 文件已经入库, 未修改, 即版本库中的文件快照内容与文件夹中完全一致. 这种类型的文件有两种去处, 如果它被修改, 而变为`Modified`. 如果使用`git rm`移出版本库, 则成为`Untracked`文件
- Modified: 文件已修改, 仅仅是修改, 并没有进行其他的操作. 这个文件也有两个去处, 通过`git add`可进入暂存`staged`状态, 使用`git checkout`则丢弃修改过, 返回到`unmodify`状态, 这个`git checkout`即从库中取出文件, 覆盖当前修改
- Staged: 暂存状态. 执行`git commit`则将修改同步到库中, 这时库中的文件和本地文件又变为一致, 文件为`Unmodify`状态. 执行`git reset HEAD filename`取消暂存, 文件状态为`Modified`

### git push解决合并冲突

`git pull`相当于 `git fetch` 跟着一个 `git merge`。拉取可以看做是把远程分支集成到本地当前分支，类似分支之间的集成，因此拉取时遇到冲突，与merge时的处理是一样的。

当本地文件落后于云端，先`git pull`拉取，在本地处理冲突（没有则直接push)，再将修改后的文件`git push`推送，达成本地和云端的一致。



---------------------------------------

##### 参考资料
[Fengyu. 40分钟学会git](https://www.bilibili.com/video/BV1db4y1d79C?spm_id_from=333.999.0.0)                 
[git文件四种状态](https://www.cnblogs.com/thirteen-yang/p/13878118.html)           
[廖雪峰git教程](https://www.liaoxuefeng.com/wiki/896043488029600/900004111093344) （！实用强推）

