---
title: git创建分支以及版本回退
date: 2018-08-13 19:50:54
tags: git操作
categories: 小知识点总结备忘
---
![](http://pcpgpxhno.bkt.clouddn.com/31104808.jpg)
> git的一大特点就是可以在一套代码流水线上创建多个分支，这些分支互不影响，但可以在需要时候合并，这对开发者太友好了，具体操作方法整理如下，以备后查！

# 一，如何创建分支

现在我要修一条北京通往杭州的公路（为什么要通往杭州，因为我喜欢），我开始拿着铲子一点一点的修，终于修完了开始通车跑。后来我想把这条路的盘山路优化成通山隧道，但是我不能影响现在的公路正常运行啊，所以我运用魔法在平行世界复制了一条一模一样的路，并在平行世界里开始改造这条路，改造好了利用魔法替换现实世界里的路即可，这个平行世界就是git的分支。
<!-- more -->
- 这个开启平行世界魔法就是：
```
//在现实世界（merge中）
git checkout -b [分支名]
```
- 合并两个世界的魔法是：
```
//回到现实世界（merge中）
git merge [平行世界]
```

- 删除平行世界的魔法是：
```
//在现实世界（merge中）
git branch -d [平行世界]
```