---
title: JavaScript的基本数据类型有哪些？
date: 2018-10-14 11:13:34
tags: 知识点整理
categories: 小知识点总结备忘
---

- 答案：
string/number/boolean/undefined/null/
- 解释：

#### 简单（基本）数据类型：
string/number/boolean/undefined/null/
这些类型的值本身无法被改变，例如：



```
//JavaScript 中对字符串的操作一定返回了一个新字符串，原始字符串并没有被改变
var str = 'harrisking'
str.split('') //["h", "a", "r", "r", "i", "s", "k", "i", "n", "g"]
console.log(str) //'harrisking'
```
<!-- more -->
#### 复杂（引用）数据类型：
object/symbol

包括：
- object：一个 Javascript 对象就是键和值之间的映射.。键是一个字符串（或者 Symbol） ，值可以是任意类型的值。
- array（数组）：是一种使用整数作为键(integer-key-ed)属性和长度(length)属性之间关联的常规对象。
- Date（日期）：当你想要显示日期时，毋庸置疑，使用内建的 Date 对象。
- function（函数）：是一个附带可被调用功能的常规对象。
