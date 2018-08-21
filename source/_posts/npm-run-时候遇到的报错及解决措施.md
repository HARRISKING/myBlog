---
title: npm run 时候遇到的报错及解决措施
date: 2018-08-21 20:44:39
tags: npm
categories: 各种坑
---
![](http://pcpgpxhno.bkt.clouddn.com/%E4%B8%8B%E8%BD%BD.jpg)
npm时出现错误“Unexpected end of JSON input while parsing near '...hment":false,"”

解决措施：

- 一，清缓存

```
npm cache clean --force
```
<!-- more -->
- 二，重新装

```
npm install --registry=https://registry.npm.taobao.org
```

> 经试有效，已备后查！
