---
title: 排序算法——BAT必考
date: 2018-07-27 14:15:33
tags: 算法
categories: 面试相关
---

![](http://upload-images.jianshu.io/upload_images/3706166-44d9dbda0eb28df8?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
前端算法虽然简单，但是思路还是很值得学习的，而且面对程序越来越复杂的当下，掌握算法，有益无害，特总结如下，以备后查！

# 狗熊掰玉米法（冒泡法）
需求是几根玉米从短到长排列。
狗熊先拿起第一个放在手中，去跟第二个比，如果大于第二个，就比较第三个，否则就放下第一个，拿起第二个，直到比较了所有，这是，手中的肯定是最大的，并把它放在最后。以此类推。
<!-- more -->


        //冒泡函数：
        function bubble(arr){
            for (let n=0; n< arr.length-1; n++){
                for(let m=0; m<arr.length-n-1;m++){
                    if(arr[m]>arr[m+1]){
                        conversion(arr,m,m+1)
                    }
                }
            }
            return arr;
        }
        //交换函数：
        function swap(array,po1,po2){
            let po = array[po1];
            array[po1] = array[po2];
            array[po2] = po;
        }


# 教官排位置法（选择排序法）
需求是几个人从小到大站队。
教官先找最小的放在第一位，于是，记住现在第一个人的身高去跟后面的每一个比个，一旦发现有更矮的，就记住这个更矮的，继续比，直到找到最矮，并把最矮的和第一位的人换位置，以此类推。

        //选择排序法:
        let array1 = [2,34,2,3,5,256,7,4,23,2,34,213,12,4125,6];
        function selectionSort(arr){
            for(let n=0; n<arr.length-1; n++){
                let min = arr[n];
                let idx = n;

                for(let m=n+1; m<arr.length+n; m++){
                    if(min>arr[m]){
                        min = arr[m];
                        idx = m;
                    }
                }
                swap(arr,n,idx);
            }
            return arr;
        }
        //交换函数：
        function swap(array,a1,a2){
            var a = array[a1];
            array[a1] = array[a2];
            array[a2] = a;
            return array;
        }
        console.log(array1)
        console.log(selectionSort(array1));



# 摸牌法（插入法）

需求是牌从小到大排列。
开始摸牌，第二张和第一张比，如果大于第一张，就放在后面，否则放前面。摸第三张牌，往前找，如果出现比自己小，就放在这张牌后面。以此类推。
我的方法：

        let array1 = [2,34,2,3,5,256,7,4,23,2,34,213,12,4125,6];

        //摸牌法
        function insertionSort(arr){
            for(let i=1; i<arr.length; i++){
                for(let j=0; j<i; j++){
                    if(arr[i]<arr[j]){
                        swap(arr,i,j);
                    }
                }
            }
            return arr;
        }
       //交换函数：
        function swap(array,a1,a2){
            var a = array[a1];
            array[a1] = array[a2];
            array[a2] = a;
            return array;
        }
        console.log(array1)
        console.log(insertionSort(array1));

网上某大神的方法：(甚为精妙)

        function insertionSort(arr){
            let value,
                j,
                i;
            for(i=0;i<arr.length;i++){
                value = arr[i];
                for(j=i-1; j> -1 && arr[j]>value;j--){
                    arr[j+1] = arr[j];
                }
                arr[j+1] = value;
            }
            return arr;
        }

# 领导分配任务法——递归（不会）

需求是将任务从小到大排列。
拿到任务，大领导将任务分给两个主管，每个主管又将自己的任务分给下属，层层分配，最后一级将拿到的两个任务大小进行排序，他的上级同样操作，以此类推。


# 快速排序——自私算法（不会）

需求是从大到小排序。
第一个不考虑其他人排序，而是只考虑自己的位置。即我只要找一个位置，这个位置满足前面的都比我小，后面的都比我大，那这个位置就是我的正确位置。以此类推。

# 随机算法——解决自私算法bug(同侧为同小或同大时)（不会）

道理同上，只不过这次不是按照顺序，从第一个开始，依次往下，而是随机的抽取一个，然后去找自己的位置。
这样做可以解决同一侧都是比自己大或小的情况，节省时间。

# 桶排法

在排序之前，我先准备几个桶（桶顺序从小到大），然后把相同高度的放在一个桶里，不同高度放在响应的桶里。放好之后在按照桶的顺序一个个拿出来。

# 基数排法

每一位进行比较，放在桶里，在按照从小到大顺序拿出，以此类推。

#六种思路：（纯自己实现，有bug欢迎提醒）[图解排序算法](https://visualgo.net/sorting)
