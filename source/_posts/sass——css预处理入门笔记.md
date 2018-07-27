---
title: sass——css预处理入门笔记
date: 2018-07-27 14:11:38
tags: 样式表相关
categories: 技术文档学习总结
---
![](http://upload-images.jianshu.io/upload_images/3706166-815998ede42caed3.gif?imageMogr2/auto-orient/strip)


# 基本用法

- 变量
解释：sass支持css变量化。与less区别在于，less使用 `@+变量名` 声明变量，而sass使用 `$+变量名` 声明变量。 
<!-- more -->

```
$new-color: red;

.h1{
    color: $new-color;
}
```

如果需要使用变量镶嵌，需要使用 `#{ }`

```
$direction: right;

.h2{
    border-#{$direction}:1px solid red;
}
```
*****
- 计算
解释：这个不用解释，和less一样，可以对css进行计算。

```
.h3{
    color: #333+#456;
    position: relative;
    top: 10px * 3;
    left: 30px / 2
}
```
*****
- 嵌套
解释： 一层套一层。（多一点通俗，少一点套路）
```
div {
    h1{
        color: blue;
    }
    color : red;
}
```
*****
- 属性嵌套
```
div{
    border: {
        color: red;
    }
}
```
*****
- 嵌套中的伪类用法
解释：在嵌套的代码块内，可以使用&引用父元素。
```
div{
    &：hover{
        border:1px solid red;
    }
}
```

*****
- 注释
解释：sass的注释有区别对待，主要看你想不想在编译后的css文件中保留注释代码。

- 只保留在sass文件中而不出现在编译好的css文件中：`// + 注释代码`；
- 需要出现在编译好的css文件中：`/*!  + 注释代码 +  */`
*****
# 代码复用
牛叉的来了，当然下面这些不常用，如果刚刚使用sass或者less，从这里开始可以忽略了。当然，这里并不难懂，有精力还是要学会的哈。
*****
- css继承
解释：使用 `@extend` 样式A继承了样式B所有的css样式。

sass:
```
.h2{
    border-#{$direction}:1px solid red;
}

.h4{
    @extend .h2;
    margin: 10px;
}
```
css:

```
.h2, .h4 {
  border-right: 1px solid red;
}

.h4 {
  margin: 10px;
}
```
*****
- Mixin
解释：将css模块化，形成类似代码块的css块。允许使用变量。`@Mixin` 声明css块，`@include` 调用css块。

sass:
```
@mixin bigBox {
    width: 100px;
    height: 100px;
    border: 1px solid #ccc;
}

div{
    @include bigBox;
}
```
css:
```
div {
  width: 100px;
  height: 100px;
  border: 1px solid #ccc;
}
```

牛叉的是，他居然可以接受参数！
sass:
```
@mixin bigBox($haha:100px) {
    width: $haha;
    height: $haha;
    border: 1px solid #ccc;
}

div{
    @include bigBox;
}
div{
    @include bigBox(500px);
}
```

css:

```
div {
  width: 100px;
  height: 100px;
  border: 1px solid #ccc;
}

div {
  width: 500px;
  height: 500px;
  border: 1px solid #ccc;
}
```
*****
- 插入
解释： 类似css的import吧。
```
@import "path/filename.scss";
```
*****
# 函数化加循环
我真的开始怀疑人生，这还是我认识的css吗？

- if判断

sass:

```
$haha: 100px;
.h1{
    @if $haha > 80px {
        margin: $haha;
        border: 1px solid red;
    }
    @if $haha <= 80px{
        border: 1px solid blue;
    }
}
```
css:
```
.h1 {
  margin: 100px;
  border: 1px solid red;
}
```
还有 `@else`的搭配使用。
sass:
```
$haha: 1px;
.h1{
    @if $haha > 80px {
        margin: $haha;
        border: 1px solid red;
    }@else {
        color: red;
    }
}
```
css:
```
.h1 {
  color: red;
}
```

- for循环
sass:
```
@for $i from 20 to 25 {
    .border-#{$i}{
        border : #{$i}px solid red;
    }
}
```


css:

```
.border-20 {
  border: 20px solid red;
}

.border-21 {
  border: 21px solid red;
}

.border-22 {
  border: 22px solid red;
}

.border-23 {
  border: 23px solid red;
}

.border-24 {
  border: 24px solid red;
}
```

- while循环：

sass:
```
$i : 5;
@while $i > 0 {
    .item-#{$i}{
        width: #{i}px;
    }
    $i : $i - 1;   
}
```
css:
```
.item-5 {
  width: ipx;
}

.item-4 {
  width: ipx;
}

.item-3 {
  width: ipx;
}

.item-2 {
  width: ipx;
}

.item-1 {
  width: ipx;
}
```

自定义函数：
sass:
```
　　@function double($n) {
　　　　@return $n * 2;
　　}
　　#sidebar {
　　　　width: double(5px);
　　}
```
css:
```
#sidebar {
  　　width: 10px;
}
```

******
如有遗漏，欢迎提醒更改，谢谢啦！

>THE END





![大哥，不点个赞嘛](http://upload-images.jianshu.io/upload_images/3706166-99475230d792e169.gif?imageMogr2/auto-orient/strip)