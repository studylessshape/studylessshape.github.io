---
title: "修改/移动 rustup 和 cargo 位置"
date: 2023-02-04T15:29:43+08:00
draft: false
description: "change rustup and cargo location"
tags: ["Rust"]
categories: ["Rust"]
summary: "rust 的各种内容默认安装并存储在 C 盘，为减小压力，有必要移动其位置。"
---

## 移动方法
移动方法按照这篇博客移动即可，博客地址：[https://tian051011.me/archives/7/](https://tian051011.me/archives/7/)。

## 环境变量来源
当然不能够说就这样完了，对于我说，我更想知道到底为什么会是这两个变量。

在进行 rust 开发的时候，有两个比较重要的工具，一个是 rustup，另一个是 cargo。rustup 中包含了 rust 开发的工具链，而 cargo 则是包管理。如果没有改变位置，那么在原本 C 盘的位置应该分别位于 `%USERPROFILE%/.rustup` 和 `%USERPROFILE%\.cargo` 下。`%USERPROFILE%` 是用户目录，一般来说就是在 C 盘。

那么对应的两个环境变量可以在 [Environment Variables - The Cargo Book](https://doc.rust-lang.org/cargo/reference/environment-variables.html) 和 [Environment Variables - The rustup book](https://rust-lang.github.io/rustup/environment-variables.html) 中找到。

原文为：
> `CARGO_HOME` — Cargo maintains a local cache of the registry index and of git checkouts of crates. By default these are stored under `$HOME/.cargo` (`%USERPROFILE%\.cargo` on Windows), but this variable overrides the location of this directory. Once a crate is cached it is not removed by the clean command. For more details refer to the [guide](https://doc.rust-lang.org/cargo/guide/cargo-home.html).
> 
> `RUSTUP_HOME` (default: `~/.rustup` or `%USERPROFILE%/.rustup`) Sets the root rustup folder, used for storing installed toolchains and configuration options.

可以看到 `CARGO_HOME` 是 cargo 的包和仓库的位置，cargo 的配置也会存在对应的文件夹下。而 `RUSTUP_HOME` 则是 rustup 的根目录，存放了工具链和配置（工具链中包含 cargo 和 rustc 等重要的工具）。

## 修改时需要注意的地方
在修改的 `CARGO_HOME` 之后，对应的位置会直接存入或下载之前 .cargo 文件夹下的内容。建议在修改之后，先不要使用 cargo，而是将原来文件夹下的 config 文件移动到新位置，或者在新位置创建一个 config。这是为了确保镜像不会出错。

并且在修改之后，也不要将原来的 .cargo 直接删除。这并不是为了将原本的内容复制到新位置，太费时间了，而是为了确保不删掉可执行文件。建议查看一下环境变量。像我这边，cargo 的运行程序就是使用的 .cargo/bin 目录下的 cargo.exe，而不是 .rustup/toolchains/%version%/cargo.exe。所以如果和我情况一样，就将 bin 复制到新目录，并将环境变量做对应的修改。

> 可以在 Path 中，新建一个位置，内容为 `%CARGO_HOME%\bin`。注意在修改之后，需要重启控制台或电脑，来确认 cargo 和 rustup 能否运行（rustup 也在 .cargo\bin 中）。

而 `RUSTUP_HOME` 这边，在修改之后，直接将原文件夹下的内容全复制到新位置就行了。因为除了工具链，其他的是文档和配置文件等内容，不算特别大，复制不需要特别久。也可以选择不复制，将原来的 settings.toml 复制到新位置，然后运行下面的命令补全文件即可。
```sh
rustup default stable
```

> 如果运行 `rustup update`，可能会提示一些警告，一般来说按照警告操作即可。