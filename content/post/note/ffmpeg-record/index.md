---
title: "ffmpeg使用的命令记录"
date: 2021-06-11T16:44:41+08:00
lastmod: 2021-07-17T16:07:01+08:00
draft: false
tags: ["笔记","ffmpeg","常用命令"]
categories: ["笔记", "ffmpeg"]
summary: "最近FFMPEG用得越来越多了，为了防止自己忘记一些常用命令，在这篇博客持续更新常用的命令"
---

题外话：gitee审核博客，我的博客因为含有色情内容（？）被毙掉了，转战github。

## 正题
我会把最近用到的 ffmpeg 指令都记下来，并尝试解释每一个 option 和参数的详细含义（也许有一些只是个人理解）。希望能够在需要用到的时候很方便的找到，并且也希望能够帮助到看博客的人，虽说应该没什么人会看我的博客（。

很多都来自于之前搜索到的方法，由于个人之前清理过一次浏览记录，而且有时候用手机查有时候用电脑查，所以暂时没法列出来源。

## 有关文件路径/文件名
个人使用 ffmpeg 时，由于部分文件名过长，所以有一个小技巧（也许只是我不知道大家都知道）。将选中的文件拖拽到控制台，控制台上就会自动填入文件路径。这是一个很方便的功能，基本一直在使用。

（如下是使用 Windows Terminal 时的界面，其他控制台/控制台集成软件应该差不多）
![拖拽文件](./1.drag-file.png)

## 目录
该项目会不定期更新。

1. [GIF（图片）与视频互转](../../../ffmpeg-record/1-gif2video-video2gif)
2. [视频中不同的输入流的提取](../../../ffmpeg-record/2-extract-video-stream)

## 非常有帮助的一些资料
1. [19 FFmpeg Commands For All Needs](https://catswhocode.com/ffmpeg-commands/)
2. [ffmpeg audio format conversions](https://linuxconfig.org/ffmpeg-audio-format-conversions)