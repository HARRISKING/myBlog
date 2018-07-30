---
title: 奔跑吧，requireJS
date: 2018-07-27 14:59:57
tags: requireJS
categories: 技术文档学习总结
---

![](http://upload-images.jianshu.io/upload_images/3706166-d21bd740249c8e8b?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> requeryJS在前端模块化的地位，就好像迪丽热巴在我心中的地位（痴汉脸~_~）

我们都知道，写一个功能复杂的页面，例如有一个轮播功能，一个回到顶部，一个曝光加载功能。这三个功能组件写在一起代码复杂程度可以媲美老孙给我家接的网线。
<!-- more -->

于是，有些人考虑将三个组件分别写在三个js文件中，然后统一使用 `<script src=' '>` 来进行调用。但是问题来了，在调用过程中，需要考虑调用顺序，否则会出现报错。

这个时候，requireJS登场。我们只需要在代码中加入 `<script src='require.js' data-main='main.js'></script>` 即可。这个标签中，`src`引用的是`require.js`文件，加载改文件之后会自动寻找`data-main`的链接作为主程序入口文件。

ok，现在requireJS已经被引用，也设定好了主程序入口文件main.js，那么，主程序如何写呢？

首先，我们的目录层级是这样的 ：


![](http://upload-images.jianshu.io/upload_images/3706166-33a91c8feeb79da3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


main.js:

    //配置：
    requirejs.config({
      //设置根路径
      baseURL: 'js/lib',
      //设置特殊路径
      paths:{
        app: '../app'
      }
    })

    //正式调用：
    requirejs(['jquery','canvas','app/sub'],function($,canvas,sub){
      具体的实现过程。。。
    })
******
或者直接引用一个文件而不做处理:

    //配置：
    requirejs.config({
      //设置根路径
      baseURL: 'www/js',
      //设置特殊路径
      paths:{
        app: '../app'
      }
    })

    requirejs(['app/sub'])

此时，sub.js的内容：

    //AMD:
    define(['jquery','com/a','com/b'],function($,a,b){
      $('./a')
    })
    //CMD:
    define(function(require,exports,module){
      var jquery = require('jquery');

    })

> 光说不练假把式，动手实操

实操的目录结构：

![](http://upload-images.jianshu.io/upload_images/3706166-69cc442a78f205f3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

html部分：

    <!DOCTYPE html>
    <html>
      <head>
        <meta charset="utf-8">
        <title>requireJS练手</title>
        <style media="screen">
          .main{
            background-color: red;
            width: 500px;
            height: 500px;
            margin: 0 auto;
            position: relative;
          }
          .box{
            width: 300px;
            height: 300px;
            background-color: #fff;
            position: absolute;
            top: 50%;
            left: 50%;
            margin-top: -150px;
            margin-left: -150px;
          }
        </style>
      </head>
      <body>
        <div class="main">
          <div class="box">
            <button type="button" class="btn1">蓝色</button>
            <button type="button" class="btn2">绿色</button>
            <button type="button" class="btn3">黄色</button>

          </div>
        </div>
      <script src="./js/lib/require.js" data-main="./js/main.js" charset="utf-8"></script>
      </body>
    </html>

主程序入口文件：

    requirejs.config({
      baseURL:'./js',
      paths:{
        jquery: './lib/jquery-3.2.1.min'
      }
    })

    requirejs(['./app/app'])

引用的主要js文件：

    //  CMD规范：
    define(function(require,exports,module){
      var blue = require('com/blue');
      var yellow = require('com/yellow');
      var green = require('com/green');
      blue();
      yellow();
      green();
    })

    // AMD规范：
    define(['jquery','com/blue','com/green','com/yellow'],function($,blue,green,yellow){
      blue();
      green();
      yellow();
    })

组件部分：

    define(['jquery'],function($){

      function turnBlue(){
        $('.btn1').on('click',function(){
          $('.box').css('background-color','blue')
        })
      }
      
      return turnBlue;
    })

[实现效果](https://harrisking.github.io/myDemo/test/requirejs/r1/index.html)

> 总结（按照顺序）

1. 在HTML文件中引用一个带有`data-main`的script的标签，类似`      <script src="./js/lib/require.js" data-main="./js/main.js" charset="utf-8"></script>
`。
 - src中引用require.js文件
 - data-main引用主程序入口文件

2. 在主程序入口文件中，需要有配置命令。即`requirejs.config`。
 - requirejs.config是一个对象。主要有baseURL，paths属性。
 - baseURL设置的是一个根目录，是相对于HTML所在目录为依托。
 - paths设置的是baseURL之外的特殊路径，是相对于baseURL为依托。
3. 组件一定要return出来，否则没卵用。组件部分就是在组件外加一个壳，这个壳是这样滴：


    define(['jquery'],function($){

        组件内容部分。。。

      return turnBlue;
    })


![解释给小白听，以便更好理解带土当时的痛苦：血轮眼每一次升级的代价，就是亲手杀死至亲之人。带土将琳和一只血轮眼托付给卡卡西，却眼见着他杀死自己的挚爱，血轮眼连跳三级直接升为最终版的万花筒，当时他承受了多么大的打击啊！](http://upload-images.jianshu.io/upload_images/3706166-d20cfc35b5bdbd84.gif?imageMogr2/auto-orient/strip)