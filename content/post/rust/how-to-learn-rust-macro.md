---
title: "如何入门 Rust 的宏？"
date: 2023-12-17T22:16:02+08:00
draft: false
tags: ["Rust", "宏"]
categories: ["Rust"]
summary: "学习宏的教程、网站和工具"
---

> 避免长篇大论，可以直接翻到最底下的[整理](#整理)一栏。

## Rust 的宏是什么？
Rust 的宏可以看作编译时期，将代码按照编写好的宏规则去生成的指令。换句话说就是用一定的规则将原来的代码替换掉。

常见的宏有两种，一个是 `macro_rules!` 声明的**声明式宏**，另一种是和 Rust 函数一样的写法的**过程宏**。

## 如何入门/学习
之前我也是看见宏复杂的符号和奇怪的语法，而没有想法去学习。

学习宏我觉得最重要的还是通过实例去学习，需要一定的积累才能够理解宏，首先要理解宏做了什么，然后才是实现它。

### 入门学习必备的 crate 和工具
有几个很方便的 `crate`，在宏小册中也提到了，学习过程宏时，使用这几个 `crate` 会很方便。
- [syn](https://docs.rs/syn)：面向过程宏，解析语法树
- [quote](https://docs.rs/quote)

必备的工具是 [cargo-expand](https://github.com/dtolnay/cargo-expand)，使用下面的指令安装和使用：

```bash
cargo install cargo-expand # install
cargo expand # run
```

[cargo-expand](https://github.com/dtolnay/cargo-expand) 可以展开宏代码，看到代码通过宏替换后的样子，非常方便，除了可以观察自己的宏有哪些问题，还可以观察其他 `crate` 中，复杂的宏在使用时发生了什么。

### 入门学习的教程和网站
宏到底是个什么，可以阅读一下 [Rust 参考手册（中文版非官方翻译）](https://rustwiki.org/zh-CN/reference/macros.html)，了解声明宏和过程宏。

入门学习宏可以使用 [proc-macro-workshop](https://github.com/dtolnay/proc-macro-workshop) 进行学习。这个仓库中包含了几个比较常用的声明宏和过程宏的实现模板。

> [syn](https://docs.rs/syn)、[quote](https://docs.rs/quote) 和 [proc-macro-workshop](https://github.com/dtolnay/proc-macro-workshop) 都是大佬 [dtolnay](https://github.com/dtolnay) 实现的，真的太强了！

另外，宏小册，即 [The Little Book of Rust Macros](https://veykril.github.io/tlborm/)，可以当作工具书进行参阅。

## 闲话
一个朋友之前尝试写了个类似于 `lisp` 的脚本语言的解析库，名叫 [dj](https://gitee.com/ZerAx/dj-rs)。我就尝试为其实现一个运行器，方便测试和运行。

而最近突然萌生了写宏的想法，刚好又完成了 [dj-runner](https://gitee.com/study_less_shape/dj-runner) 的编程。在实现的过程中发现 [dj](https://gitee.com/ZerAx/dj-rs) 中的 `Value::Builtin`，和 [actix-web](https://actix.rs/) 中的路由以及 [bevy](https://bevyengine.org/) 中的 `System` 有着相似的形式，就想着能不能尝试将内建函数的形式简化。

后来实际的实现与路由和 `System` 差别挺大的，实现的方式也比较粗暴，但总的来说效果还不错，能够感觉到一些学习宏并实现一个的成就感。

## 整理
- `crate`
  - [syn](https://docs.rs/syn)：面向过程宏，解析语法树
  - [quote](https://docs.rs/quote)
- 教程
  - [Rust 参考手册（中文版非官方翻译）](https://rustwiki.org/zh-CN/reference/macros.html)
  - [proc-macro-workshop](https://github.com/dtolnay/proc-macro-workshop)
  - [The Little Book of Rust Macros](https://veykril.github.io/tlborm/)