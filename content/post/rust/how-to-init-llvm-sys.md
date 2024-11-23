---
title: "如何初始化 llvm-sys"
date: 2024-11-23T14:37:14+08:00
draft: true
tags: ["Rust"]
categories: ["Rust"]
summary: "尝试在 rust 中调用 LLVM 并生成中间代码时出现了问题，并尝试解决"
---

## 导入 llvm-sys库

添加 crate [llvm-sys](https://crates.io/crates/llvm-sys) 到现有的项目中。

```shell
cargo add llvm-sys
```

如果有指定 llvm 版本需求，可以指定版本，比如版本 `18.1.x` 的 LLVM，可以指定 llmv-sys 的版本为 `180`。

目前最新版本支持到了 `190`，即当前最新版本 `19.1.0` 的 LLVM。

## 编译问题

如果试用 rust-analyzer 或运行命令 `cargo check`，会提示一个错误。

```shell
D:\Projects\rust\test-project>cargo check                               
warning: unused manifest key: worksapce
   Compiling llvm-sys v191.0.0
error: No suitable version of LLVM was found system-wide or pointed
              to by LLVM_SYS_191_PREFIX.
       
              Consider using `llvmenv` to compile an appropriate copy of LLVM, and
              refer to the llvm-sys documentation for more information.
       
              llvm-sys: https://crates.io/crates/llvm-sys
              llvmenv: https://crates.io/crates/llvmenv
   --> C:\Users\studylessshape\.cargo\registry\src\rsproxy.cn-0dccff568467c15b\llvm-sys-191.0.0\src/lib.rs:529:1
    |
529 | / std::compile_error!(concat!(
530 | |     "No suitable version of LLVM was found system-wide or pointed
531 | |        to by LLVM_SYS_",
532 | |     env!("CARGO_PKG_VERSION_MAJOR"),
...   |
539 | |        llvmenv: https://crates.io/crates/llvmenv"
540 | | ));
    | |__^

error: could not compile `llvm-sys` (lib) due to 1 previous error
```

这是在提示需要为 `LLVM_SYS_191_PREFIX` 指定正确的 LLVM 路径。

### 下载安装 LLVM

{{< notice info >}}
Linux 直接使用包管理器安装 LLVM，在安装完成后，试着输入命令 `llvm-config`，如果打印出了使用帮助信息，则可以不需要这一节的内容。
{{< /notice >}}

进入 LLVM 的官网 https://llvm.org/，点击左侧的 `All Releases`，或者直接进入 https://releases.llvm.org/。

找到自己需要的版本，并点击 <u>`Download`</u>。

![download-llvm-page](download-llvm-page.png)

目前的本人尝试下载的两个版本，都将下载地址放到了 github 上，而不是其官方网站了。

在 github release 中下载需要注意，不要下载 `win32.exe` 或 `win64.exe`，因为这两个执行文件的是安装包，安装好的 bin 文件夹下没有 `llvm-config` 程序。以 `19.1.0` 为例，需要下载下方的 `LLVM-19.1.0-Windows-X64.tar.xz` 文件。文件很大，所以如果下载不下来需要魔法才行，或者尝试去寻找并使用 github 文件下载加速站。

![select-file-from-github-release](select-file-from-github-release.png)

下载好之后，将压缩包解压出来，建议路径中最好没有空格和非英文字母的字符。

解压好之后，可以直接在 bin 文件夹下看一下有没有 `llvm-config.exe`，确认好之后就需要配置环境变量了。

### 设置环境变量

根据提示，可以在控制台中设置临时变量。设置好之后运行 `cargo check`。

```shell
set LLVM_SYS_191_PREFIX=D:\\Programs\\LLVM
cargo check
```

如果还出现了编译错误，并且与上面提到的问题一致，则可能需要重启一下控制台、IDE 或者系统。

也可以设置到系统的环境变量里。

![set-env-for-system](set-env-for-system.png)

没问题时，`cargo check` 应该会有如下输出：

![cargo-check-success](cargo-check-success.png)

{{< notice note>}}
如果出现了类似于 `cl.exe did not execute successfully` 的错误，可能是使用了 LLVM 的安装包安装，并且直接将原来的安装目录下的文件，替换为了下载好的完整 LLVM 包里的文件，或者是路径中包含了空格。
{{</ notice >}}