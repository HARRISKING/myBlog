---
title: less——css预处理语言入门笔记
date: 2018-07-27 13:55:41
tags: 样式表相关
categories: 技术文档学习总结
---

# 引入概念

## 什么是css预编译？ 
在书写公司代码时，我们会发现同一公司的网页或者同一个项目的网页，在样式上都是成套匹配的。
举个例子，QQ10.1版本这个APP的主题色是蓝色，那么在可能次页的颜色为蓝色的60%，某个按钮颜色为蓝色的40%等等。那么如果现在发布QQ10.2版本，主题色改为当季最流行的抹茶绿，难道要我把样式表中的所有蓝色都要更改，那这工作量太巨大了。于是，css预编译诞生了，它将css变成了可编译，可赋变量的形式。这样，我就可以通过设置变量为主题色，然后大范围引用这个变量，那么，下次我直接修改变量的颜色，整个页面的颜色就自动修改了，达到牵一发而动全身的目的。这就是css预编译。目前主流的有 **[LESS](http://lesscss.cn/)** 和 **[SCSS](http://sass-lang.com/)** 。

<!-- more -->

## 什么是css预编译器？ 
我们通过自定义写了一大堆除了我们自己，浏览器根本不认识的东西，怎么用啊？于是，css预编译器诞生啦，将你写的谁也看不懂的东西再发布之前自动编译成真正的css，这样你方便了，浏览器能看懂了，皆大欢喜！

## 什么是css后编译？ 
挨千刀的网景公司和微软公司当年的浏览器大战虽然早已折戟沉沙，但是这场战争的影响残留至今并且每天折磨着前端工程师。因为不同的浏览器有不同的规则，因此即使是使用 **[FLEX](https://developer.mozilla.org/en-US/docs/Web/CSS/flex)** 也要因不同的浏览器内核而加不同的前缀，这太闹心啦！因此css后编译诞生，你只需要正常的去写你的css就好了，写完之后，由css后编译(例如[postcss](https://www.npmjs.com/package/postcss))来帮你添加上无聊而又冗杂的前缀、兼容代码等等。

# 使用方法

## 变量化
解释：允许定义css变量，然后在需要这个变量的地方引用。

- less:


    @beautiful-color: blue;

    .a{
        color: @beautiful-color;
        margin : 0;
    }
- css:


    .a {
      color: blue;
      margin: 0;
    }

*******
## 混合化
解释：在样式A中引入样式B，样式B中的css样式就会自动复制到样式A中。
- less:


    .header{
        border: 1px solid red;
        .a;
    }
- css:


    .header {
      border: 1px solid red;
      color: blue;
      margin: 0;
    }

or

- less:


    .box-radius(@my-radius:5px){
        border-radius: @my-radius;
        -webkit-border-radius: @my-radius;
        -moz-border-radius: @my-radius;
    }

    #header{
        .box-radius;
    }
    #header1{
        .box-radius(4px);
    }
- css:


    #header {
    border-radius: 5px;
    -webkit-border-radius: 5px;
    -moz-border-radius: 5px;
    }
    #header1 {
    border-radius: 4px;
    -webkit-border-radius: 4px;
    -moz-border-radius: 4px;
    }

*******
## 嵌套化
解释：子元素，直接子元素等可以层层嵌在其中。

- less:


    .b{
        color: @beautiful-color;
        .c{
            margin-top: 10px;
        }
        .d >p {
            margin-bottom: 10px;
            .e{
                color:@beautiful-color*3;
                &:before{
                    color: @beautiful-color;
                }
                > li{
                    display: inline;
                }
            }
        }
        &:after{
            content: ' ';
            clear: both;
            display: block;
        }
    }
- css:


    .b {
      color: blue;
    }
    .b .c {
      margin-top: 10px;
    }
    .b .d > p {
      margin-bottom: 10px;
    }
    .b .d > p .e {
      color: #0000ff;
    }
    .b .d > p .e:before {
      color: blue;
    }
    .b .d > p .e > li {
      display: inline;
    }
    .b:after {
      content: ' ';
      clear: both;
      display: block;
    }
*******
## 运算
解释：css允许进行运算

- less:


    .f{
        color:@beautiful-color*3
    }
- css:


    .f {
      color: #0000ff;
    }
