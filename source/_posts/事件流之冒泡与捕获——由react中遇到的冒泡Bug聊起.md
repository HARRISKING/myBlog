---
title: 事件流之冒泡与捕获——由react中遇到的冒泡Bug聊起
date: 2018-08-24 16:25:03
tags: 事件流之冒泡与捕获
categories: 小知识点总结备忘

---

![](http://pcpgpxhno.bkt.clouddn.com/153666498.jpg)

> React项目中遇到需要阻止冒泡的需求，觉得这是一个之前没有被发现的坑，总结在此，以备后查！

# 一，背景

世纪初的网景与微软公司的浏览器大战想必身为程序员都略有耳闻，冒泡与捕获——两种截然相反的处理事件机制，就是这场战争的主角之一。

```
      <div class='s1'>
          <p class='s2'></p>
      </div>
```
***

<!-- more -->
# 二，冒泡事件及事件捕获

- ## 什么是冒泡事件

顾名思义，像是水中冒泡一样，从下往上。事件由第一个被触发的元素接收，然后逐级向上传播。`s1`、`s2`同时有点击事件，当点击子元素`s2`时，会先触发`s2`再触发`s1`。
![冒泡示意图](http://upload-images.jianshu.io/upload_images/3706166-a285be644a5a4025.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- ## 什么是事件捕获

与冒泡事件相反，是冲上往下。事件由body逐级向下传播，直到到达目标元素。`s1`、`s2`同时有点击事件，当点击子元素`s2`时，会先触发`s1`再触发`s2`。
![捕获示意图](https://upload-images.jianshu.io/upload_images/3706166-d53354f882eceb44.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/244/format/webp)

***
# 三，DOM2之后的世界太平——addEventListener的第三个参数

网景和微软曾经的战争还是比较火热的，当时， 网景主张捕获方式，微软主张冒泡方式。后来 w3c 采用折中的方式，平息了战火，制定了统一的标准——先捕获再冒泡。
addEventListener的第三个参数就是为冒泡和捕获准备的。
addEventListener有三个参数：
```
element.addEventListener(event, function, useCapture)

//第一个参数是需要绑定的事件
//第二个参数是触发事件后要执行的函数
//第三个参数默认值是false，表示在事件冒泡阶段调用事件处理函数;如果参数为true，则表示在事件捕获阶段调用处理函数。
```
w3c说，大家有话好好说，争来争去的干嘛呀，来，用这个，你爱用啥用啥，反正我的事件流固定就是先捕获后冒泡，至于你愿意谁先谁后，你自己注册（就是js里声明的意思）。
![新的事件流示意图](https://upload-images.jianshu.io/upload_images/3706166-a3d820a799cc471e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/244/format/webp)

举个例子

html
```
<div id="s1">s1
    <div id="s2">s2</div>
</div>
```
javaScript
```
<script>
s1.addEventListener("click",function(e){
        console.log("s1 冒泡事件");         
},false);
s2.addEventListener("click",function(e){
        console.log("s2 冒泡事件");
},false);
        
s1.addEventListener("click",function(e){
        console.log("s1 捕获事件");
},true);
        
s2.addEventListener("click",function(e){
        console.log("s2 捕获事件");
},true);
</script>
```
好，大家跟我拿着这个图一步一步照着看：
![新的事件流示意图](https://upload-images.jianshu.io/upload_images/3706166-a3d820a799cc471e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/244/format/webp)

捕获阶段

- 点击s2（目标节点），click事件从document->html->body->s1->s2

- 这里在s1上发现了捕获注册事件，则输出"s1 捕获事件"

- 到达s2，已经到达目的节点，

- s2上注册（在js中声明，下面不一一解释）了冒泡和捕获事件，先注册的冒泡后注册的捕获，则先执行冒泡，输出"s2 冒泡事件"

- 再在s2上执行后注册的事件，即捕获事件，输出"s2 捕获事件"

冒泡阶段

- 下面进入冒泡阶段，按照s2->s1->body->html->document

- 在s1上发现了冒泡事件，则输出"s1 冒泡事件"

***

# 四，如何停止冒泡事件和阻止浏览器默认行为

- #### e的兼容

```
function fn(e){
    var event = e || window.event;
}
```

`FireFox`里的`Event`跟`IE`里的不同，`IE`里的是全局变量，随时可用。`FireFox`里的要用参数引导才能用，是运行时的临时变量在`IE/Opera`中是`window.event`，在`FireFox`中是`event`。而事件的对象，在`IE`中是`window.event.srcElement`，在Firefox中是`event.target`，`Opera`中两者都可用

- #### 阻止事件冒泡

`W3C`的方法是`e.stopPropagation()`，`IE`则是使用`e.cancelBubble = true`。`stopPropagation`是事件对象`Event`的一个方法，作用是`阻止目标元素的冒泡事件`，但是`不会阻止默认行为`。`stopPropagation`就是阻止目标元素的事件冒泡到父级元素了解更多请点这:[理解DOM中的事件流](https://blog.luckyw.cn/2015/11/15/dom-event-flow/)

- #### 阻止事件冒泡兼容:

```
function stopPropagation(e) {
    var e = e || window.event;
    if ( e && e.stopPropagation ){
        e.stopPropagation();
    }else{
        e.cancelBubble = true;
    }
}
```

- #### 阻止浏览器默认行为

`W3C`的方法是`e.preventDefault()`，`IE`则是使用`e.returnValue = false`。`preventDefault`是事件对象`Event`的一个方法，作用是取消一个目标元素的默认行为。如果元素没有默认行为，调用无效。什么元素有默认行为呢？如链接`<a href="xxx">点我</a>`，提交按钮`<input type=”submit”>`等

**return false**:
`JS`的`return false`只会阻止默认行为，而`jQuery`则既阻止默认行为又防止对象冒泡

**阻止浏览器默认行为兼容：**

```
function stopDefault(e) {
    var e = e || window.event;
    if (e && e.preventDefault){
        e.preventDefault();
    }else{
        e.returnValue = false;
    }
    return false;
}
```

***

# 五，react中采用的事件处理机制及阻止方式

首先我们要了解react中合成事件和原生事件是什么。

- #### 合成事件

在jsx中直接绑定的事件，如

```
<a ref="aaa" onClick={(e)=>this.handleClick(e)}>更新</a>
```
这里的handleClick事件就是合成事件
- #### 原生事件

通过js原生代码绑定的事件，如

```
document.body.addEventListener('click',e=>{
    // 通过e.target判断阻止冒泡
    if(e.target&&e.target.matches('a')){
        return;
    }
    console.log('body');
})
```
或者
```
this.refs.update.addEventListener('click',e=>{
    console.log('update');
});
```
- #### 阻止冒泡事件分三种情况

A、阻止合成事件间的冒泡，用e.stopPropagation();
```
import React,{ Component } from 'react';
import ReactDOM,{findDOMNode} from 'react-dom';

class Counter extends Component{
    constructor(props){
        super(props);
        this.state = {
            count:0,
        }   
    }

    handleClick(e){
        // 阻止合成事件间的冒泡
        e.stopPropagation();
        this.setState({count:++this.state.count});
    }

    testClick(){
        console.log('test')
    }

    render(){
        return(
            <div ref="test" onClick={()=>this.testClick()}>
                <p>{this.state.count}</p>
                <a ref="update" onClick={(e)=>this.handleClick(e)}>更新</a>
            </div>
        )
    }
}

var div1 = document.getElementById('content');

ReactDOM.render(<Counter/>,div1,()=>{});
```
B、阻止合成事件与最外层document上的事件间的冒泡，用e.nativeEvent.stopImmediatePropagation();
```
import React,{ Component } from 'react';
import ReactDOM,{findDOMNode} from 'react-dom';

class Counter extends Component{
    constructor(props){
        super(props);
        this.state = {
            count:0,
        }
    }

    handleClick(e){
        // 阻止合成事件与最外层document上的事件间的冒泡
        e.nativeEvent.stopImmediatePropagation();
        this.setState({count:++this.state.count});
    }

    render(){
        return(
            <div ref="test">
                <p>{this.state.count}</p>
                <a ref="update" onClick={(e)=>this.handleClick(e)}>更新</a>
            </div>
        )
    }
    
    componentDidMount() {
        document.addEventListener('click', () => {
            console.log('document');
        });
    }
}

var div1 = document.getElementById('content');

ReactDOM.render(<Counter/>,div1,()=>{});
```
C、阻止合成事件与除最外层document上的原生事件上的冒泡，通过判断e.target来避免
```
import React,{ Component } from 'react';
import ReactDOM,{findDOMNode} from 'react-dom';

class Counter extends Component{
    constructor(props){
        super(props);
        this.state = {
            count:0,
        }
    }

    handleClick(e){
        this.setState({count:++this.state.count});
    }
    render(){
        return(
            <div ref="test">
                <p>{this.state.count}</p>
                <a ref="update" onClick={(e)=>this.handleClick(e)}>更新</a>
            </div>
        )
    }

    componentDidMount() {
        document.body.addEventListener('click',e=>{
            // 通过e.target判断阻止冒泡
            if(e.target&&e.target.matches('a')){
                return;
            }
            console.log('body');
        })
    }   
}

var div1 = document.getElementById('content');

ReactDOM.render(<Counter/>,div1,()=>{});
```
# 六，参考文章：

- [JS中事件冒泡与捕获](https://segmentfault.com/a/1190000005654451)
- [react阻止冒泡事件，绝对干货](https://zhuanlan.zhihu.com/p/26742034)
