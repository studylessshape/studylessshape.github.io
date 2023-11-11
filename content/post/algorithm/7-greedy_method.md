---
title: "算法课题目笔记：贪心算法"
date: 2020-11-27
lastmod: 2020-12-19
draft: false
cover: "cover.png"
tags: ["算法", "笔记"]
toc: true
categories: ["笔记"]
summary: "用来做算法笔记，本文对应的是《算法设计与分析（第二版）》第七节贪心算法，题目是来自NOI的，用来练习算法的。这里记下笔记方便日后复习"
---

> 程序实验课做的题，题目来自[NOI-4.6算法之贪心](http://noi.openjudge.cn/ch0406/)。本文留作复习使用

## 目录
- [2469-电池寿命](#1-battery-life)
  - [三种情况](#three-situations)
    - [1.只有两个电池](#only-two-battery)
    - [2.一个电池的使用寿命特别长](#one-longer-than-other)
    - [3.任何一个电池的寿命都不可能比其他电池寿命的总和大](#split-group)
  - [代码实现](#code-implement)
- [702:Crossing River](#2-crossing-river)
  - [题目](#problem)
    - [描述](#problem-description)
    - [输入](#problem-intput)
    - [输出](#problem-output)
    - [样例输入](#problem-input-example)
    - [样例输出](#problem-output-example)
    - [来源](#problem-from)
  - [两种策略](#two-strategies)
    - [次快的把船送回来](#second-send-boat)
    - [让最快的送其他人过去](#fast-send-people)
  - [代码实现](#code-implement-2)
<hr/>

<a name="1-battery-life"></a>

## [2469-电池寿命](http://noi.openjudge.cn/ch0406/2469/) [¶](#1-battery-life)
解题的关键在于理解如何分配电池。代码实现很简单，但是要首先知道电池是怎么分配出来的。有三种情况

<a name="three-situations"></a>

### 三种情况 [¶](#three-situations)
<a name="only-two-battery"></a>

#### 1.只有两个电池 [¶](#only-two-battery)
这是最简单的情况，只需要输出最短的那个时间就行。

<a name="one-longer-than-other"></a>

#### 2.一个电池的使用寿命特别长，比其他电池的总和还长 [¶](#one-longer-than-other)
这是第二种情况，可以画一个图来具象表示一下。下面的电池就是那个寿命特别长的电池，上面的小电池的寿命的总和都没有下面的长。这时候直接计算上面的小电池的总和就行了。<br/>
![sum_smaller_than_one](7-1-battery-1.png)

实质上，第一种情况是第二种情况的特殊情况，所以可以将这两种归为一种情况。

<a name="split-group"></a>

#### 3.任何一个电池的寿命都不可能比其他电池寿命的总和大 [¶](#split-group)
第三种情况是任何一个电池的寿命都不可能比其他电池寿命的总和大，这种情况需要分组了。分组要尽量保证两组的寿命相近。比如样例中的`[3,3,5]`，就可以按照下面的样子分组。<br/>
![group_of_335](7-1-battery-2.png)

分组之后会发现上面那组会比下面那组的要多出一节，那么先用上面两节电池，直到上下寿命长度相等，然后再上下匹配使用，这时候就能达到最长使用长度了。（灰色代表用去的部分）<br/>
![use_group_of_335](7-1-battery-3.png)

相应的，其他数量的电池都可以这样分组，并进行相同的处理。最终会得到，无论怎样分组，最后的使用寿命都是电池寿命总和的1/2。

<a name = "code-implement"></a>

### 代码实现 [¶](#code-implement)
在输入时，就处理好最大电池寿命和其他电池寿命的总和：<br/>
(7.1.battery.cpp，部分代码)
```c++
        while(n--)
        {
            cin>>ba;
            if(max_life < ba)
            {
                // 如果比当前值大，就把之前没有加的值加上，然后记录当前值
                sum += max_life;
                max_life = ba;
            }
            else
            {
                // 将比最大电池寿命小的电池加到总和
                sum += ba;
            }
        }
```

这里为了理清程序逻辑，将核心部分分离了出来。下面是对不同情况进行处理的代码：<br/>
(7.1.battery.cpp，部分代码)
```c++
float calculate_longest_life(int sum, int max_life)
{
    if(max_life > sum)
        return sum; // 第一、二种情况
    return (float)(sum + max_life) / 2.0;// 第三种情况
}
```

全部代码在[这里](https://gitee.com/study_less_shape/Practice_Saving/blob/master/Algorithm/Greedy_method/7.1.battery.cpp)可以找到。

<a name = "2-crossing-river"></a>

## [702:Crossing River](http://noi.openjudge.cn/ch0406/702/)[¶](#2-crossing-river)
原题是英文，直译成中文。


<a name = "problem"></a>

### 题目[¶](#problem)

<a name = "problem-description"></a>

#### 描述[¶](#problem-description)
有N个人想用最多只能载两个人的船过河。因此必须安排某种过河顺序来回划船以至于所有人都能过河。每个人有不同的划船速度；每一对的速度取决于最慢的那一个。你的工作是决定一个使这些人过河时间最短的策略。

<a name = "problem-intput"></a>

#### 输入[¶](#problem-intput)
第一行输入一个整型`T`（1<=`T`<=20），表示测试用例数。之后跟着`T`种情况。每种情况的第一行是`N`，第二行数`N`个整型数，表示每个人过河的时间。不会超过`1000`个人并且过河时间均不会超过`100`秒。

<a name = "problem-output"></a>

#### 输出[¶](#problem-output)
对于每一测试用例，打印一行`N`个人都过河所需要的总时间。

<a name = "problem-input-example"></a>

#### 样例输入[¶](#problem-input-example)
```
1
4
1 2 5 10
```
<a name = "problem-output-example"></a>

#### 样例输出[¶](#problem-output-example)
```
17
```

<a name = "problem-from"></a>

#### 来源[¶](#problem-from)
POJ Monthly--2004.07.18

<a name = "two-strategies"></a>

### 两种策略[¶](#two-strategies)
首先，考虑只有三个或三个以下的人怎么送。这时候最简单的最省时间的方法就是用最快的那个把其他两个人或一个人或自己送过去。<br/>
![three-people-cross-river](7-2-cross-river-1.gif)

接下来看看四个人的。

<a name = "second-send-boat"></a>

#### 次快的把船送回来[¶](#second-send-boat)
让最快送次快的人过去，再让最慢的两个过河，然后次快的把船送回来。

举个例子，四个人，划船速度分别是`1`、`4`、`16`、`15`。这时，先让`1`、`4`过河，然后`1`把船送回来，再让`16`、`15`过河，`4`把船送回来，最后`1`、`4`过河。总的时间为`4+1+16+4+4=29`。

但是有些情况这样送就不妥当。

<a name = "fast-send-people"></a>

#### 让最快的送其他人过去[¶](#fast-send-people)
直接举例子，四个人，划船速度分别是`9`、`10`、`10`、`11`，如果这次用上面的方法，总时间为`10+9+11+10+10=50`。如果让最快的那个送其他人过河呢？这时总时间为`10+9+10+9+11=49`。明显是第二种方法快！

那么贪心策略就是选上面两种方法中耗时最少的！

<a name = "code-implement-2"></a>

### 代码实现[¶](#code-implement-2)
为了方便使用贪心策略，需要对输入的数据进行排序。这里直接调用`algorithm`头文件里的`sort`函数进行升序排序。

和之前一样把主要的逻辑部分分出来，塞在单独的函数中。
```c++
#include <iostream>
#include <algorithm>
using namespace std;

int cross_river(int people[], int N)
{
    // continue...
}

int main()
{
    int T, N, people[1001];
    cin>>T;
    while(T--)
    {
        cin>>N;
        for(int i = 0;i < N;i ++)
        {
            cin>>people[i];
        }

        sort(people, people+N);

        cout<<cross_river(people, N)<<endl;
    }

    return 0;
}
```

在`N>=4`时，从末尾开始遍历`people`数组。根据[贪心策略](#two-strategies)，选择最小的一种情况。
```c++
    int total_time = 0;
    while(N >= 4)
    {
        total_time += min(
            // 最快的和次快的先过去，然后最快的把船送回来，即people[1] + people[0]
            // 然后最慢的两个人过去，次快的把船送回来，即people[N-1] + people[1]
            people[1] + people[0] + people[N-1] + people[1],

            // 最快的送最慢的人过去，然后最快把船送回来，即people[N-1] + people[0]
            // 最快的送次慢的人过去，然后最快把船送回来，即people[N-2] + people[0]
            people[N-1] + people[0] + people[N-2] + people[0]
        );
        // 每次循环送过去两个人，所以减二
        N-=2;
    }

    // continue...
```

之后退出循环或是未进入循环，会有三种情况，`N=1`、`N=2`、`N=3`，使用`switch`或者`if`判断即可
```c++
    // 剩下的1-3个人，用最快的人送过去
    switch(N)
    {
        case 1:
            total_time += people[0];
            break;
        case 2:
            total_time += people[1];
            break;
        case 3:
            total_time += people[2] + people[0] + people[1];
            break;
    }
    return total_time;
```

运行看一看！
![running-code-result](7-2-cross-river-2.png)

没有问题。

全部代码在[这里](https://gitee.com/study_less_shape/Practice_Saving/blob/master/Algorithm/Greedy_method/7.2.cross-river.cpp)可以找到。