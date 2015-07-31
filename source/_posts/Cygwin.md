title: Cygwin simple introduction
date: 2015-03-29 13:45:45
categories: [Apps]
tags: [Linux,Shell,Cygwin]
photos:
---
<img src="http://img3.douban.com/lpic/s6584385.jpg" class="full-image" />

最近正在学习`Shell`编程，但是我使用的是Windows系统，又不想破费麻烦的去为此装一个虚拟机。

百度之后发现[Cygwin](http://www.cygwin.com/)这个工具很好用，介绍给大家。

### Cygwin是什么

* 一个遵循GNU和开源工具标准协议的超大集合，它可以在Windows系统上提供一套相似的Linux功能。
* 一个DLL(cygwin1.dll)提供了大量的POSIX API的功能。

### Cygwin不是什么

* 不是Windows上运行本地Linux应用程序的一种方式。如果你非得在Windows上运行，必须重构这个应用程序的源代码。
* 你如果想利用Cygwin所提供的功能，达到能够很神奇的使得本机Windows应用程序去了解Unix的功能，比如机制、终端等，那是不可能的。除非从源代码重新构造您的应用程序。

<!-- more -->

### 下载安装

* [win32](https://cygwin.com/setup-x86.exe)
* [win64](https://cygwin.com/setup-x86_64.exe)

开始安装
![](http://blog.chinaunix.net/attachment/201206/16/26425645_1339845755HpPa.png)
一般选择第一个就可以，下一步继续
![](http://blog.chinaunix.net/attachment/201206/16/26425645_1339845859FzDe.png)
这一步有4个软件包版本下载/安装的全局设置：
* Keep：表示保持已安装的软件包的版本不变。
* Prev：表示安装上一个稳定版本的软件包。
* Curr：表示安装当前稳定版本的软件包，通常使用这个设置。
* Exp：表示安装最新的实验性版本的软件包。

继续下一步，总共有50M，如果资源好，数度会很快的
![](http://blog.chinaunix.net/attachment/201206/16/26425645_13398459892216.png)
安装完毕。
![](/img/cygwin.png)

### 优化配置

1.我下载的这个版本是默认有Vi的，如果没有Vi的你可以继续点击setup.exe，在第3布，选择要安装的Vi package安装就可以了
2.clear清屏命令，在默认安装时没有这个的，也要额外download package，可以用“Ctrl+l”l是字母l哦
3.修改桌面\cygwin\.bashrc
```plain
# 让ls和dir命令显示中文和颜色 
alias dir='dir -N --color' 
alias less='/bin/less -r' 
alias ls='/bin/ls -F --color=tty --show-control-chars'

# 设置为中文环境，使提示成为中文 
export LANG="zh_CN.GB2312" 
# 输出为中文编码 
export OUTPUT_CHARSET="GB2312"
```
4.修改桌面\cygwin\.inputrc
```
# 支持输入中文 
set convert-meta off 
set input-meta on 
set output-meta on
```

更多的信息可以去[Cygwin官网](http://www.cygwin.com/)查看。


