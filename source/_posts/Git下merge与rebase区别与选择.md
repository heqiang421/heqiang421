---
title: ' Git下merge与rebase区别与选择'
tags:
  - Git
abbrlink: e159ec26
date: 2018-12-09 20:54:22
---
> 之前pull代码时一直使用merge，遇见了冲突也就直接解决，这样做还算平安。但最近加入一个项目，Team人数，提交Code次数都偏多，这样拉取代码，出现冲突的频率也就很高。使用Merge的话，log点线图就会出现多条线，然后来回合并的情况，很混乱。
> 
> 针对这情况，Team提倡使用rebase方式拉取代码，但这样好在哪呢，这里查了些资料，整理下。

## 创建分支
Git因为有了分支，使得我们可以并行进行许多feature的开发，并且互不干扰。在完成后，利用MR来合并到主干分支，比如dev或者master.

## 合并分支
我们以分支的模式进行开发，然后希望将开发的功能代码放进到主干，这个办法就是合并，在GitLab里叫做MR。

啰嗦下，Git是分布式的，所以比如我们提交的上游服务器存在多条分支，我们每个人的开发机上也会是多条分支，分支合并不仅仅指origin上的A与B的合并，其实我们本地分支提交到上游对应分支，也是一次分支合并。

## 分支冲突处理
冲突起源是代码之前修改的地方在同一位置，这时，其实Git也不知道该要谁的，所以才会让你去操作，比如accept theirs，yours,或者两者手动去结合下。

### merge方式处理冲突
merge方式合并分支，并不保留merge分支的commit,比如下图，棕色的是sprint27,而紫色的是master，合并过来其实之算作一次merge-commit

![](https://static.1991421.cn/2018-12-09-043723.png)

### rebase方式处理冲突
rebase方式合并分支，会将合并分支的commit都合并到目标分支线上，并且也不会创建新的提交

假设合并前是这样：
![](https://static.1991421.cn/2018-12-09-122640.png)

merge合并后：
![](https://static.1991421.cn/2018-12-09-122651.png)

rebase合并后：
![](https://static.1991421.cn/2018-12-09-122730.png)


习惯使用rebase经常拉取代码之后，现在项目上的线就基本就是条笔直的线了，每次谁提交，提交了什么， 简单明确。

![](https://static.1991421.cn/2018-12-09-123124.png)

## 写在最后
- Git这家伙，玩的越多越觉得它简单，强大和灵活。对于它的理解也在实践中不断强化。我司有人说过，一个复杂的东西，做成简单的样子来使用，这才难，我觉得Git就是这样。
- 针对rebase还是merge操作，与几个好友聊天及结合团队情况，觉得以下`实践较佳`
	- 获取远程项目代码时，使用 `git pull -- rebase`
	- 合并分支，使用`git merge --no-ff`,创建一个merge commit

## 参考
- [使用 git rebase 避免無謂的 merge](https://ihower.tw/blog/archives/3843)
- [[git]merge和rebase的区别](http://www.cnblogs.com/xueweihan/p/5743327.html)
