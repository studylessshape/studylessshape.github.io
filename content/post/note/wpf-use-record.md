---
title: "WPF 使用记录目录"
date: 2024-02-23T20:42:48+08:00
draft: false
tags: ["WPF", "C#","记录"]
categories: ["C#"]
summary: "记录使用 WPF 进行开发的时候一些难点的解决方案（不定时更新）"
---

## 说明
最近正在做 WPF 的项目，制作的时候有些功能的实现比较难，但是实际上编写出来之后也没有特别多的代码。

不过由于网络上能找到的资料很少，大部分都是网上找到的方法没有效果之后，通过一些其他较为复杂的方法实现了。

所有用到的参考文档和资料除了在每篇博客文章中外，还会在此目录页做汇总。相关的项目有时间会上传。

所有示例项目用的版本为 `.net 6.0`。

### 用到的依赖
为了方便快速开发，所有的项目基本都会用到下面的依赖：
- [CommunityToolkit.Mvvm](https://github.com/CommunityToolkit/dotnet)
- [MaterialDesignThemes](https://github.com/MaterialDesignInXAML/MaterialDesignInXamlToolkit)
  > MaterialDesignThemes 需要根据 [Getting Start](https://github.com/MaterialDesignInXAML/MaterialDesignInXamlToolkit/wiki/Getting-Started) 来进行配置。
  >
  > 同时其 Demo 程序是重要的参考，如果和我一样使用 Release 版，可以直接去 Github 的 Release 中下载 DemoApp，如果想使用最新版的功能，可以克隆整个项目，并直接运行 Demo 项目。

## 目录
- [ItemsControl 组件的 Item 获取自己的 Index](../../../csharp/wpf/item-of-itemscontrol-get-self-index)
- [（紧接上一篇）将多个参数传入 Command 中]()