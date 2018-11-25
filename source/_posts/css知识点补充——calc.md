---

title: css知识点补充——calc
date: 2018-11-25 14:08:13
tags: 知识点整理
categories: 小知识点总结备忘
---

> 最近遇到需求，需要使用css3的animation实现div从容器左侧移动到右侧，由于容器宽度未知，所以很苦恼如何实现获取右边的移动距离，这个css方法可以实现。

### 这个方法就是：calc()

上代码：

- html:
<body>
<div class="box">
  <div class="inner"></div>
</div>
</body>

- css:

.box{
  width:100px;
  height: 100px;
  border: 1px solid;
  position: relative;
}

.inner{
    position: absolute;
    background: yellow;
    width:30px;
    height: 30px;
    animation: animate 1s;
}

<!-- more -->

@keyframes animate{
  from{
    background: red;
    left: 0px;
  }
  to{
    background: green;
    left: calc(100% - 30px);
  }
}
```
### TIP:

calc()方法中，运算符的优先级遵循运算优先级，并且运算符前后要有空格才能生效。
