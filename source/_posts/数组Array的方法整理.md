---
title: 数组Array的方法整理
date: 2018-11-12 15:13:04
tags: 知识点整理
categories: 小知识点总结备忘
---

### 写出数组arr的每一步转换方法：`let arr=[1,2,2,5]`，转换方法为`arr=>[1,2,5]=>[1,2,3,4,5]=>[5,4,3,2,1]=>15（最后一步是求和）`

- concat去重:`[1,2,2,5] => [1,2,5]`

```
let arr=[1,2,2,5];
let arr1 = arr.concat()
```

- splice添加:[1,2,5]=>[1,2,3,4,5]

```
let arr2 = arr1.splice(1,0,3,4)
```

- reverse倒序:[1,2,3,4,5]=>[5,4,3,2,1]

```
let arr3 = arr2.reverse()

```
<!-- more -->
- eval/map求和:15

```
<!-- eval -->
let arr4 = eval(arr3.join('+'))

<!-- map -->
let sum = 0;
let arr4 = arr3.map((item)=>{
    sum += item;
})
```

