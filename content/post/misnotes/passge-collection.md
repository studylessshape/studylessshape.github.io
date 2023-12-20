---
title: "Passge Collection"
date: 2022-02-12
lastmod: 2023-12-20T20:06:02+08:00
draft: false
tags: ["文章收集", "C#", "Rust"]
categories: ["随记"]
summary: "收集一些我看过的好文章、工具或框架等"
---

最近突然想把看到的一些好的文章收集一下了，慢慢更新，看到一篇收集一篇。

## 计算机基础
- [小林Coding](https://xiaolincoding.com/)
  > 有图解网络、图解系统、图解MySql、图解Redis的文章，分析的非常厉害。

## C#
- [小心IDictionary<TKey, TValue>](https://www.nimaara.com/beware-of-the-idictionary-tkey-tvalue/)
  > 大致说了下使用IDictionary实例化对象和直接使用Dictionary的区别，并从IL汇编代码进行了分析。
- [MonoGame](https://www.monogame.net/)
  > `C#` 的一个游戏框架，XNA 的开源版本。虽然微软官方的 XNA 更新貌似已经停掉了，但是 MonoGame 一直有在更新，而且一些独立游戏也是使用这个框架开发的，比如[蔚蓝](https://www.celestegame.com/)。
  > 刚开始学习，官方的教程还是较为简单的，可以看看官方推荐的一些第三方教程。

## Rust
### 学习
- [Rust宏入门](../rust/how-to-learn-rust-macro.md)

### Web 框架
- [actix-web](https://actix.rs/)
  > 据说源码中非常多的 `unsafe` 引起了争议，但是这个框架确实还不错。并且有比较厉害的维护者接手项目，有着不错的更新频率。
  > 官方的文档啃的话对我自己来说有点难度，所幸找到了官方给的例子，通过例子学习也不错，[例子仓库](https://github.com/actix/examples)。可以边学边看文档来熟悉 actix，另外也找到了中文翻译的文，[链接](https://web.veaba.me/rust/actix-web/)。对于 actix 的学习应该有很大帮助。

  > 我计划将我学习的过程记录下来。[地址](../../../rust/actix-web-study-note/00.00index/)

- [axum](https://axum.rs/)
  > 也是不错的 Web 框架，目前的文档比较完善了，并且可以用来当主力 Web 框架使用了

- [tarui](https://tauri.app/)
  > 可以将前端发布为当前所在系统的程序的框架，支持一些主流的前端框架，并且可以通过编写 rust 代码来给前端添加更多的功能