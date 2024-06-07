---
title: "文章、工具收集"
date: 2022-02-12
lastmod: 2024-06-07T15:05:02
draft: false
tags: ["文章收集", "C#", "Rust", "Github", "工具", "Tools"]
categories: ["随记"]
summary: "收集一些我看过的好文章、工具或框架等"
---

最近突然想把看到的一些好的文章收集一下了，慢慢更新，看到一篇收集一篇。

## 计算机基础

- [小林 Coding](https://xiaolincoding.com/)
  > 有图解网络、图解系统、图解 MySql、图解 Redis 的文章，分析的非常厉害。

## C#

- [小心 IDictionary<TKey, TValue>](https://www.nimaara.com/beware-of-the-idictionary-tkey-tvalue/)
  > 大致说了下使用 IDictionary 实例化对象和直接使用 Dictionary 的区别，并从 IL 汇编代码进行了分析。
- [MonoGame](https://www.monogame.net/)
  > `C#` 的一个游戏框架，XNA 的开源版本。虽然微软官方的 XNA 更新貌似已经停掉了，但是 MonoGame 一直有在更新，而且一些独立游戏也是使用这个框架开发的，比如[蔚蓝](https://www.celestegame.com/)。
  > 刚开始学习，官方的教程还是较为简单的，可以看看官方推荐的一些第三方教程。
- [Reference Source](https://referencesource.microsoft.com/)
  > C# 的 .net framework 的源码网站，可以在线查看源码，也可以下载下来查看源码

### C# UI

- [ScottPlot](https://github.com/scottplot/scottplot)
  > 高性能的数据图标展示库，支持 WPF、WinForm 等主流的 C# 应用框架
- [LiveCharts2](https://livecharts.dev/)
  > 性能和占用上没有 ScottPlot 那么好，但是风格化和动画表现优秀

## Rust

- [RsProxy](http://rsproxy.cn/)
  > 目前用过最快的 Rust 国内镜像，来自字节跳动团队。

### 学习

- [Rust 官方文档中文教程](https://rustwiki.org/)

  > 有非常多 Rust 文档的中文版，适合查阅和学习

- [Rust 宏入门](../rust/how-to-learn-rust-macro.md)
  - [Rust 参考手册（中文版非官方翻译）](https://rustwiki.org/zh-CN/reference/macros.html)
  - [proc-macro-workshop](https://github.com/dtolnay/proc-macro-workshop)
  - [使用 macro_rules! 来创建宏（通过例子学 Rust 中文版）](https://rustwiki.org/zh-CN/rust-by-example/macros.html)
  - [The Little Book of Rust Macros](https://veykril.github.io/tlborm/)

### Web 框架

- [actix-web](https://actix.rs/)

  > 据说源码中非常多的 `unsafe` 引起了争议，但是这个框架确实还不错。并且有比较厉害的维护者接手项目，有着不错的更新频率。
  > 官方的文档啃的话对我自己来说有点难度，所幸找到了官方给的例子，通过例子学习也不错，[例子仓库](https://github.com/actix/examples)。可以边学边看文档来熟悉 actix，另外也找到了中文翻译的文，[链接](https://web.veaba.me/rust/actix-web/)。对于 actix 的学习应该有很大帮助。

  > 我计划将我学习的过程记录下来。[地址](../../../rust/actix-web-study-note/00.00index/)

- [axum](https://axum.rs/)

  > 也是不错的 Web 框架，目前的文档比较完善了，并且可以用来当主力 Web 框架使用了

- [tarui](https://tauri.app/)
  > 可以将前端发布为当前所在系统的程序的框架，支持一些主流的前端框架，并且可以通过编写 rust 代码来给前端添加更多的功能

## Github

- [Github action 官方文档](https://docs.github.com/zh/actions)

## 前端

- [vue](https://cn.vuejs.org/)：响应式前端框架
  - [vuepress](https://vuepress.vuejs.org/zh/)：静态文档生成
  - [vuepress-theme-hope](https://theme-hope.vuejs.press/zh/)：vuepress 的一个主题
  - [Element Plus](https://element-plus.org/zh-CN/)：vue 的一个 UI 组件库
  - [vuetify](https://vuetifyjs.com/zh-Hans/)：Modern 风格的 UI 组件库

- [Animotion](https://animotion.dev/)
  > css 动画制作

## Markdown
- [markmap](https://github.com/markmap/markmap)
  > 使用 markdown 制作思维导图

## 3D建模/游戏资源
- [PolyHaven](https://polyhaven.com)
- [Blender](https://www.blender.org/)

## 视频相关
- [ffmpeg](https://ffmpeg.org/): 强大的命令行工具，可以用来做二次开发
- [Kdenlive](https://kdenlive.org/zh/): 来自 Kde 社区的开源视频编辑工具
- [Aegisub](https://aegisub.org/zh-cn/): 字幕编辑工具，虽然很久没更新了但是是功能最多最强大的

## 图像制作/处理
- [Krita](https://krita.org/zh-cn/): 来自 Kde 社区的开源画图软件，风格类似 PS

## 音频/音乐相关
- [PxTone](https://pxtone.org/): 来自洞窟物语作者的软件，可以制作音乐

## Shell
- [MobaXterm](https://mobaxterm.mobatek.net/): 免费的终端软件