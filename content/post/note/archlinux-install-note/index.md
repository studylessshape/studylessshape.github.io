---
title: "Archlinux 安装笔记"
date: 2024-06-06T10:33:09+08:00
lastmod: 2024-06-07T16:05:09+08:00
draft: false
tags: ["archlinux", "linux", "安装"]
categories: ["Linux"]
summary: "记录下安装 Archlinux 的经历"
---

## 前言
在 618 期间买了台新电脑，因为很早之前就想试试直接在电脑上而不是虚拟机里使用 Linux 系统，所以尝试着给旧电脑安装 Archlinux，原因有两点：

1. Archlinux 自由度高
2. 因为有 `archinstall`，目前的安装 Archlinux 已经非常简单了。

> 另外这台旧电脑准备送给家人用，Linux 不太方便的安装软件方式，可以杜绝调很多安全问题。

旧电脑为华硕（ASUS）的飞行堡垒5。

- CPU: Intel(R) Core(TM) i5-8300H CPU @ 2.30GHz
- 显卡: NVIDIA GeForce GTX 1050Ti
- 内存: 16GB Samsung 2667MHz （内存曾经自己加了一个 8GB 的）
- 主板: ASUSTek COMPUTER INC. FX505GE

## 用到的网站
- [Archlinux 官网](https://archlinux.org/)
  > 下载 Archlinux 镜像
- [rufus](https://rufus.ie/zh/)
  > 镜像安装引导盘制作
- [官方安装教程（中文翻译）](https://wiki.archlinuxcn.org/wiki/%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97)
- [Archlinux 简明指南](https://arch.icekylin.online/guide/)
  > 包含安装教程，和一些常用软件、工具的安装
- [RsProxy](http://rsproxy.cn/)
  > 目前使用过最快的国内 Rust 镜像
- [paru](https://github.com/Morganamilo/paru)
  > AUR 软件包安装助手
- [AUR](https://aur.archlinux.org/)
  > archlinux 的 aur 仓库网页，搜索需要的软件包很方便，官方直接支持的软件包可以点开 `Home` 旁边的 `packages` 查看

## 下载镜像
进入 [Archlinux 官网](https://archlinux.org/)，点击导航栏最右侧的 Download，然后下载 torrent 文件，使用自己常用的种子下载工具下载镜像即可。

> 直接使用磁力链接也可以，但是部分时候磁力链接可能加载不了下载列表。

![archlinux-download](01.arhclinux-download.png)

Archlinux 的镜像文件相对来说比较小。写这篇博客时，下载的版本是 `archlinux-2024.06.01-x86_64`。

### 启动盘制作
此处我使用 [rufus](https://rufus.ie/zh/)，当然其他制作工具也是可以的。

**！！！制作启动盘前，请将 U 盘内的重要数据进行备份！！！**

插入一个 U 盘，然后打开 rufus，分区类型选择 GPT，目标系统类型选择 UEFI，然后点击开始。

![rufus-make](02.arhclinux-ustart-make.png)

U 盘会先被格式化，然后 rufus 会写入系统镜像。

## BIOS 设置
在 BIOS 设置里关闭安全启动，不同的主板可能 BIOS 界面有所区别。

## 启动安装引导
此处需要根据不同的主板来进行操作，比如华硕的主板，出现 ASUS Logo 时长按 ESC 键，然后选择 USB 启动即可。

因为电脑已经寄走了，所以之后的安装画面都会使用虚拟机。

安装选择版本选择 `x86_64` 即可，也可根据自己电脑的实际情况选择。

![archlinux-bios](03.arhclinux-bios.png)

![archlinux-bios-enter](04.arhclinux-bios-enter.png)

出现上图的样子，就是成功进入了安装界面了。

## 检查网络
在进入安装界面的上面，有这样一段话：

![network-hint](05.network-hint.png)

输入 `iwctl` 可以进入无线网络的连接程序。输入 `help` 指令可以获取帮助。

Wlan 联网可以参考简明指南的[连接网络章节](https://arch.icekylin.online/guide/rookie/basic-install.html#_3-%E8%BF%9E%E6%8E%A5%E7%BD%91%E7%BB%9C)。

因为有线网能直接检测到，所以我选择的方法是直连随身 WIFI 或者通过手机的 USB 来共享网络。

## 检查磁盘
这一步的目的是为了清空磁盘。如果需要重装 Archlinux，这一步比较关键。

> 在重装 Archlinux 时，如果不预先删掉分区，`archinstall` 的磁盘选项里，通过建议创建的分区后，不会删掉原来的分区，安装报错并中断。

！！！清空磁盘前，请确认磁盘的重要数据已经备份！！！

输入 `fdisk -l`，查看自己的磁盘。

![fdisk-list](06.fdisk-list.png)

上面显示的有两个磁盘，其中 `/dev/sda` 是我现在的磁盘。

如果有分区，即上面的命令有 `/dev/sda1`、`/dev/sda2`，建议输入 `fdisk /dev/sda`，进入磁盘管理控制台。然后输入 `d` + 回车删除分区，多个分区需要多次 `d` 指令。

> 电脑如果有两个磁盘，或者只有一个磁盘但是已经安装了 Windows 系统，可能会导致 `archinstall` 无法找到该磁盘，但是 `fdisk` 能够列出该磁盘，所以可能需要同样的方式删除磁盘分区（待验证）。

## 安装
网络和磁盘配置好之后，可以键入 `archinstall` + 回车，来安装 Archlinux 了。

![enter-archinstsall](07.enter-archinstall.png)

> 如果一直卡在 `Check version`，可以使用 `Ctrl` + `C` 先终止安装，再输入 `archinstall --skip-version-check` 来禁止版本检查。

进入 `archinstall` 的页面如上所示。

`Archinstall language` 不作修改，点击 `↓` 或 `j` 选择 `Mirrors`，回车进入后选择 `Mirror region` 并回车。等待一小会儿之后，键入 `/china`，搜索国内镜像，并回车选择。

![mirrors-select](08.mirrors-select.gif)

按 `j` 到 `← Back` 并回车。进入 `Locales` 设置，需要更改的是 `Locale language`，进入后，选择 `zh-cn.UTF8`。

![locale-select](09.locale-select.gif)

然后是磁盘配置。直接使用建议配置就行，因为之前已经做了删除分区的操作，所以执行安装时应该不会出错了。磁盘格式可以按自己需要选择，此处选择 `btrfs`。

![disk-configuration](10.disk-configuration.gif)

主机名可以按需求更改，此处不变。

`Root Password` 即 root 用户密码，建议设置一个。`User account` 里，设置一个用户，是否有 `sudo` 权限可以根据自己需要来，为了方便我设置了 `sudo` 权限。

![user-set](11.user-set.gif)

`Profiles` 选择桌面类型，当然如果是有其他需要，也可以不直接通过 `archinstall` 安装桌面环境。桌面环境挺多的，具体可以在 [分类:桌面环境](https://wiki.archlinuxcn.org/wiki/Category:%E6%A1%8C%E9%9D%A2%E7%8E%AF%E5%A2%83) 查看，此处选择 Kde Plasma。

![profile-set](12.profile-set.gif)

`Audio` 可以 `No audio server`，但还是建议随意选一个。

`Kernels` 即内核，建议多选择一个 `linux-zen`，方便之后安装 `waydroid` 并运行安卓应用。

`Network configuration` 建议如果选择安装了桌面环境，就使用 `Use NetworkManager`，这样可以使有无线网卡的主板，连接 WIFI。

`Timezone` 选择上海。

最终的配置如下所示。

![end-archinstall-configuration](13.end-archinstall-configuration.png)

选择 `Install`，然后回车，Archlinux 就正式开始安装了。安装过程中尽量保证网络畅通。

经过一段时间的等待，安装便完成了，会提示是否进入控制界面，选择进入。

![install-end](14.install-end.png)

Archlinux 没有自带中文字体，所以使用 pacman 安装一下中文字体：

```bash
pacman -S noto-fonts-cjk
```

安装完成后，输入 `exit` 退出 chroot 模式，然后输入 `poweroff` 关机，将 U 盘拔掉，再开机。

## 配置系统
再次启动后，会进入到 ssdm 的启动页，如下所示，说明我们已经成功安装了 Archlinux，接下来就是配置系统的时候了。

![enter-ssdm](15.enter-greeter.png)

> 如果觉得当前的启动页与 Kde 的风格不符合，可以进入 `系统设置->颜色和主题->登录屏幕(ssdm)`，选择 `Breeze 微风`并应用即可。
> ![modify-ssdm-theme](15.1.modify-ssdm-theme.png)

输入密码进入桌面，我们依次打开 `左下角菜单->系统->Konsole 终端`。

> 因为在 archinstall 中，已经修改了镜像地区，所以后续使用 pacman 或 paru 时，并不需要修改地区。如果出现安装卡住，大部分时候是因为 aur 上软件包安装文件的指定服务器，国内无法访问或者访问过慢。

### 安装浏览器
首先需要安装的是浏览器，火狐其实就很不错，输入下面的命令安装火狐浏览器：

```bash
sudo pacman -S firefox
```

输入密码和 `y` 并回车继续安装。

![install-firefox](16.install-firefox.png)

### 安装 paru
安装 `git` 和 `rustup`：

```bash
sudo pacman -S git rustup
```

打开浏览器，在地址栏输入 `http://rsproxy.cn/` 或者输入 `rsproxy` 并搜索，进入 rsproxy 的网站。按照教程修改 `.bashrc`，然后输入下面的命令安装 rust：

```bash
rustup install stable
```

安装完成之后，按照 rsproxy 的教程，修改 `~/.cargo/config` 或 `~/.cargo/config.toml` 的内容。

> 修改文件内容时，可能会出现找不到 `vi` 指令，这是因为 Archlinux 不会默认将 `vim` 指定 `vi` 别称。

打开 [paru 的 github 仓库](https://github.com/Morganamilo/paru)，找到 `Install` 出，复制下面的命名开始安装 paru。

```bash
sudo pacman -S --needed base-devel
git clone https://aur.archlinux.org/paru.git
cd paru
makepkg -si
```

### 安装输入法
输入法选择安装 fcitx5，即小企鹅 5。输入下面的命令安装输入法所需的所有软件包：

```bash
paru -S fcitx5-im fcitx5-chinese-addons
```

> 除了 `fcitx5-chinese-addons`，也可以查看 [fcitx5 的 archlinux 页面](https://wiki.archlinuxcn.org/wiki/Fcitx5)，安装其他中文输入法组件。
> 同时可以寻找想要安装的词库、皮肤或其他组件，也可以去搜索引擎中搜索。

安装全部内容完成之后，进入系统设置。打开`键盘->虚拟键盘`，选择 `Fcitx5` 并应用。应用之后会提示是否启用云拼音，可以根据自己需要选择。

![select-virsual-keyboard-fcitx5](17.select-virsual-keyboard-fcitx5.png)

现在在可以输入的地方按下 `Ctrl` + `Space`，就可以看到光标旁的 `en` 变成了 `拼`，就可以输入中文了，同时也可以再次按下 `Ctrl` + `Space` 切换回英文输入。

#### 配置输入法（可选）
返回系统设置，找到区域和语言，选择输入法。点击配置全局选项，将 `切换启用/禁用输入法` 的快捷键修改为自己习惯的按键，此处我修改成了 `Shift` 键。

![config-fcitx5](17.1.config-fcitx5.png)

修改好后，应用并返回，找到 `配置附加组件...->经典用户界面`，往下翻找到 `主题`，在这里可以修改输入法的主题。

![config-fcitx5-theme](17.2.config-fcitx5-theme.png)

### 安装 QQ（安装示例）
QQ 的安装比较典型，因为 QQ 在安装之后无法正常使用输入法。

安装 QQ 可以使用下面的命令：

```bash
paru -S linuxqq
```

执行时会提示有几个不同的软件包：

![execute-paru-s-linuxqq](18.execute-paru-s-linuxqq.png)

不确定的话，可以上 [Aur](https://aur.archlinux.org/) 上搜索 `linuxqq`，看看几个软件包有什么区别：

![search-linuxqq](19.search-linuxqq.png)

QQ 在之前发布过 NT 版，支持多平台。而在 [Aur](https://aur.archlinux.org/) 上，`linuxqq` 被标记过期，那么其实最好是选择其他的软件包安装。其中 `linuxqq-nt-bwrap` 和 `linuxqq-appimage` 任选其中一个安装即可。此处选择 2，即 `linuxqq-appimage`。

安装完成，打开并登录 QQ，此时如果想发消息，会发现切换不了输入法。

退出 QQ，打开左下角菜单，找到 QQ，`右键->编辑应用程序...->应用程序(A)`，看到参数一栏，在后面添加下面的参数：

```bash
--enable-features=UseOzonePlatform --ozone-platform=wayland --enable-wayland-ime
```

然后就可以正常使用输入法了。

## 结束
剩下的都可以自己慢慢摸索了。

想要的软件包可以在 [Packages - Archlinux](https://www.archlinux.org/packages/) 和 [Aur](https://aur.archlinux.org/) 上安装，Kde 社区的软件可以上 [Kde 应用软件官网](https://apps.kde.org/zh-cn/) 上查找。

也可以看看 [Archlinux 简明指南](https://arch.icekylin.online/guide/) 和 [Archlinux wiki](https://wiki.archlinux.org/)（[Archlinux 中文维基](https://wiki.archlinuxcn.org/wiki/%E9%A6%96%E9%A1%B5)）。