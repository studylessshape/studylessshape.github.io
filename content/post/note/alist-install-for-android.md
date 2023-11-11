---
title: "安卓手机安卓Alist以及配置"
date: 2023-01-04T16:24:38+08:00
draft: false
tags: ["Alist","Termux","私有网盘","WebDav"]
categories: ["私有网盘搭建"]
summary: "无需 root，利用 termux 轻松安装和配置 alist"
---

## 前言
最近换手机了，旧的手机放着没用，想着拿旧手机的空间搭个私有网盘。由于之前试过可道云，可道云的配置比较麻烦，这次试试 [Alist](https://alist.nn.ci/zh/) 网盘。

[Alist](https://alist.nn.ci/zh/) 有这么几个好处：
1. 安装方便快捷；
2. 可以配置除本地之外的网盘，如可以添加百度网盘；
3. 播放视频等可以调用本地播放器，如PC端的PotPlayer，安卓端的MxPlayer；
4. 无需 root；
5. 支持WebDav（据说也只支持 WebDav，其实也算是一个缺点）。

## 安装 termux
termux 现在需要从 f-droid 上下载了，地址为 [https://f-droid.org/zh_Hans/packages/com.termux/](https://f-droid.org/zh_Hans/packages/com.termux/)。选择最新版下载即可，写此博客时，termux 的最新版本为 0.118.0。termux 本身的文件还是比较大的，可能会下载比较久。
![termux-on-fdroid](./1.termux-on-fdroid.jpg)

下载好之后直接安装，能够打开的话，一般来说就没什么问题了，可以进行下一步操作了。

### 换源
在搜索引擎里搜索 tuna，找到清华大学镜像站，如果进入的是主页，则继续选择开源镜像站。进去后搜索 termux，点击名称旁边的问号，则会进入 termux 换源的帮助页面，链接为 [https://mirrors.tuna.tsinghua.edu.cn/help/termux/](https://mirrors.tuna.tsinghua.edu.cn/help/termux/)。复制下图的命令并运行：
![tuna-mirror-command](./2.tuna-mirror-command.png)

在回车后，会提示是否继续，输入 y 并回车，会更新源。之后在提示的时候，按下回车，使用默认配置即可。

### 安装 vim
vim 是 linux 下常用的编辑器，当然也可以选择安装 neovim。

输入下面的命令安装 vim：
```sh
pkg install vim -y
```

如果安装时非常慢，可能是换源失败，或者是当时的清华镜像源出了问题，可以选择换其他的镜像源。

### 安装 ssh（可选）
安装 ssh 主要是为了方便电脑对 termux 进行配置，没有此需求的可以无视。

输入下面指令安装 ssh：
```sh
pkg install openssh -y
```

然后输入下面的指令修改 ssh 的密码：
```sh
passwd
```

修改后输入下面的指令启用 ssh：
```sh
sshd
```

使用 ifconfig 查看机身的 ip 地址，并使用电脑端的 ssh 应用连接。termux 的 ssh 端口为 8022，登录账户为 root，密码为刚才修改的密码。

## 安装质感文件
在酷安上搜索质感文件，或者进入 f-droid 上搜索质感文件，下载并安装。（f-droid的链接为 [https://f-droid.org/packages/me.zhanghai.android.files/](https://f-droid.org/packages/me.zhanghai.android.files/)）。

质感文件有两个比较重要的作用，一个是可以直接操作 termux/home 下的文件，另一个作用是查看文件的路径。

## 安装 Alist
Alist 的安装较为简单。打开 termux，输入下面的指令直接安装：
```sh
pkg install alist -y
```

安装好之后，使用下面的指令查看 admin 的账户和密码：
```sh
alist admin
```

输入指令和比较重要的结果如下图所示：
![alist-admin](./3.alist-admin.jpg)

在开启 alist 网盘之前，首先使用 ifconfig 查看自己的 ip 地址。
![ifconfig](./4.ifconfig.jpg)

然后输入下面的指令开启 alist：
```sh
alist server
```

### 配置 Alist 存储
如果使用当前安装 alist 的手机访问 alist，直接在手机浏览器中输入 `127.0.0.1:5244` 进行访问。如果使用其他设备的浏览器，则需要输入 `[ip]:5244` 进行访问，其中 `[ip]` 为之前 ifconfig 中查看的地址。进入之后，大概率会看到这个界面：
![alist-start-page](./5.alist-start-page.png)

点击下面的登录，输入之前的账户和密码。登录成功之后，点击下面的管理，进入管理界面。
![alist-manage-page](./6.alist-manage-page.png)

首先将密码修改为自己能够记住的密码，然后点击保存。之后在左侧导航栏点击存储，点击添加存储，驱动选择时，向下划，选择本地存储。
![alist-storage-page](./7.alist-storage-page.png)

挂载路径，是我们在进入 alist 网盘时访问的路径，此处按照自己需求填写。本人此处填入了 `/`，即根路径。往下翻，排序选择`名称`，提取文件夹选择`提取到最前`。再往下翻，有一个根文件夹路径，这个路径的设置比较麻烦。

打开 termux，按左侧并向右滑动，点击 new session，打开一个新的终端。输入 `whereis alist` 指令查看 alist 的位置：
![whereis-alist](./8.whereis-alist.jpg)

由上图可以看到 alist 位于 `/data/data/com.termux/files/usr/bin/alist`。那么可以知道 termux/home 的位置为 `/data/data/com.termux/files/home`。那么为了文件较为干净并且方便管理，我们在 termux/home 下为 alist 专门创建一个文件夹。

先输入指令 cd，然后输入指令 mkdir alist-files，最后输入 ls 查看是否创建成功。
![cd-mkdir-alist-files](./9.cd-mkdir-alist-files.jpg)

然后回到浏览器，将位置 `/data/data/com.termux/files/home/alist-files` 填入根文件夹路径。最后点击添加。没有问题的话，存储界面会多一个存储的条目，回到主页会发现之前的报错已经没有了。
![alist-storage-state](./10.alist-storage-state.png)
![alist-home-page](./11.alist-home-page.png)

在主页创建几个文件夹，并尝试上传文件来测试是否可用。（此处我尝试创建了三个文件夹，并上传了一个 gif 图。）
![test-alist](./12.test-alist.png)

这里将 termux/home 挂载到了 alist 的根目录。接下来将挂载机身文件地址。

### 挂载机身地址
本人不太建议直接将机身挂载到 alist 的根目录，因为这样管理起来会很乱。其实之前的 termux/home 也应该挂载在一个路径中，而不是根路径上。但是直接挂载到根路径使用会很方便，而且 termux/home 中文件的改变并不会影响到机身文件，只会占用存储空间，所以这样操作了。

打开质感文件，随便选一个文件夹，复制路径。
![copy-android-storage-path](./13.copy-android-storage-path.jpg)

然后回到 alist，点击管理，进入存储，点击添加，驱动选择本地存储。这一次，挂载路径填入 `/phone`。
![alist-storage-phone](./14.alist-storage-phone.png)

往下划，排序选择`名称`，提取文件夹选择`提取到最前`，这样比较符合使用习惯，当然如果你的使用习惯不太一样可以自行选择。根文件夹路径填入刚才复制的路径，并且删掉最后的文件夹名称。最终如下所示：
![alist-storage-phone-path](./15.alist-storage-phone-path.png)

点击保存，如果报错，可能是挂载路径冲突或者根文件夹路径错误。如果还是不能报错或者出错就需要自己排查了。

进入主页，进入 phone 文件夹，发现无法打开，有如下报错：
![alist-phone-dir-error](./16.alist-phone-dir-error.png)

出现这个问题是我们还没有给 termux 存储权限，在手机上，给 termux 存储权限之后，就可以了：
![alist-phone-dir](./17.alist-phone-dir.png)

## 一些问题
termux 很容易被后台杀掉，需要在电池设置中，将 termux 保持常驻，启用 termux 的各种允许启动或者是关闭 termux 的各种电池优化。总之就是要防止 termux 被杀掉。

另外建议保持手机的充电状态。这样才能保证 termux，即 alist 能一直运行。所以使用手机作为服务器或者私有网盘，对手机电池的消耗还是比较大的。

Alist 方面，手机上配置好的 Alist 网盘，在打包下载方面存在着问题，所以如果想批量下载文件可能会不太行。还有就是文件的上传，在手机上使用 Alist 时，只能单个文件上传，不能选择文件夹上传或者选择多个文件上传。除此之外，Alist 并不会显示当前存储空间的情况。

不过目前来说还不错，能够利用闲置的空间做点有趣的事情。比如上传视频，然后可以通过 MxPlayer 或者 PotPlayer 观看，还挺流畅的。

## 题外话
手机上有个开源软件叫阅读，可以在酷安上找到并下载。这个软件可以通过 WebDav 来备份和恢复。这里就简单的说一下 Alist 的 WebDav 怎么使用。

打开阅读，选择最右边的人物图标，有一个选项叫做备份和恢复。在 WebDav 服务器地址处，输入 `[你的 Alist 网盘地址]/dav`，WebDav 帐号输入 admin，密码输入设置的密码，如果忘了，可以在 termux 中打开新的终端输入 alist admin 查看密码。（此处我填入的是我使用 alist 管理员添加的一个账户）
![reader-webdav-config](./18.reader-webdav-config.jpg)

然后点击备份，备份完成后，点击恢复，查看下是否备份成功。下图中显示了两个备份恢复文件，一个是我刚才备份的，一个是之前的备份。刚才的备份中的设备名称，和配置中显示的设备名称一致，说明 WebDav 的备份配置成功了。之后可以使用 Alist 进行备份和恢复了。
![reader-webdav-recover](./19.reader-webdav-recover.jpg)

如果无法备份，那么在 Alist 管理中，选择用户，添加一个拥有 WebDav 权限的账户，并将阅读中的账户改为新添加的账户即可。