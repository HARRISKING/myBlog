---
title: typeOf jQuery，typeOf Vue，typeOf React输出的结果分别是什么？
date: 2018-10-14 11:19:32
tags: 知识点整理
categories: 小知识点总结备忘
---
 - 答：
    - typeOf jQuery === 'function'
    - typeOf Vue === 'function'
    - typeOf React === 'function'

- 解释：

#### 什么是typeOf？
它返回一个字符串，表示未经计算的操作数的类型。
- typeOf 333 === 'number'
- typeOf NaN === 'number'

<!-- more -->

- typeOf Symbol('foo') === 'symbol'

- typeOf true === 'boolean'

- typeOf '字符串' === 'string'

- typeOf undefined === 'undefined'

- typeOf null === 'object'
- typeOf {a:1} === 'object'
- typeOf new Date() === 'object'
- typeOf [23,3,11,232] === 'object'

- typeOf function(){} === 'function'
- typeOf class C{} === 'function'

#### 为什么都是'function'?
- Vue:
```
var app = new Vue({
    el:'app',
})
```

- React:
```
class App extend React,Component{

}
```

- jQuery:

从jQuery的源代码中,我们可以知道:var $ = jQuery.因此当我们$(selector)操作时,其实就是jQuery(selector),创建的是一个jQuery对象.当然正确的写法应该是这样的:var jq = new $(selector);而jQuery使用了一个小技巧在外部避免了new,在jquery方法内部:if ( window == this ) return new jQuery(selector);