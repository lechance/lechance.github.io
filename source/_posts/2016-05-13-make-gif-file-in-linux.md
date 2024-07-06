---
title: Notes - Make Gif File in Linux(debian)
date: 2016-05-13 13:30:00
categories:
- Android
tags:
- gif
- ffmpeg
- convert
---

本篇是记录如何在Linux(我的系统是Debian)下制作用来演示app操作的gif动画文件.因为之前知道的方法是只有在ps里制作演示gif文件,而在Linux下安装ps又需要安装[Wine](https://www.winehq.org)这样的软件,由于自己电脑硬盘容量只有320G,所以像ps这样的软件还是能不装就不装吧.

> 一位[朋友](http://my.csdn.net/harvic880925)推荐了在windows和mac下的两个工具`gifcam`和`licecap`,先记录在这里方便以后用到的时候有地方查.


### 工具
- GIMP这个图片处理工具一般系统默认安装的,没有安装的话可以下载.
- Kazam这个一个屏幕录制工具,使用非常简便,可以在这里[下载](https://launchpad.net/kazam)安装,也可以通过apt-get安装.

```shell 
sudo apt-get update && sudo apt-get install kazam
```
- FFMPEG这是一个非常强大的工具,这里我只是用它来转换格式,更多的信息你可以到[官网](https://ffmpeg.org)了解, 安装方式如下：

```shell
sudo apt-get update && sudo apt-get install libav-tools ffmpeg
```
使用ffmpeg从命令行能够转换一个视频文件(mp4或其他格式)到多个png格式的文件.下面命令提取视频的帧到独立的png文件中,当然你可以对这些帧进行编辑.

```shell
ffmpeg -i input.mp4 -r 10 output%05d.png` 
```

- ImageMagick同样是一个强大的图片处理工具,它用于创建,编辑,组合以及转换位图图片,支持超过200种格式(PNG,JPEG,JPEG-200,GIF,PDF,SVG).`Convert`程序用于在两种图片格式间进行转换,更多关于`Convert`命令的使用可以参考[这里](http://www.imagemagick.org/script/convert.php).

```shell
convert output*.png output.gif
```

也可以通过`Convert`命令进一步对图片进行编辑,选项 -delay [fps/sec] 和 -loop [numbers],如下：

```shell
convert -delay 1x20 -loop 0 out*.gif converted-out.gif
```
- 由于最终生成的文件太大,需要做一些优化,下面是通过`ImageMagick`的优化功能来减小gif文件的大小.

```shell
convert -layers Optimize demo.gif optimized-demo.gif
```

优化效果不怎么好(文件大小2.8M),要是你知道更好的优化gif文件大小的工具,可以在下方留言,-_-

### 示例

![demo](/images/small.gif)

再贴一个同类教程的帖子，很实用. 
[Make GIF Snapshot for Android APP ](http://www.liaohuqiu.net/posts/make-gif-for-android-app/)
