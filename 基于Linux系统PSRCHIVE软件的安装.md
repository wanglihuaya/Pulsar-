---
id: 基于Linux系统PSRCHIVE软件的安装
title: 脉冲星物理软件安装
description: 基于Linux系统PSRCHIVE软件的安装
sidebar_label: 基于Linux系统PSRCHIVE软件的安装
slug: /
---

## 基于 Linux 系统 PSRCHIVE 软件的安装

## 安装环境

1. Linux 虚拟机，deepin，ubuntu，centos 都可以，建议使用 deepin 或者 ubuntu。

2. 双系统，给电脑硬盘分区，在新的分区里安装新的系统，deepin，ubuntu，centos 都可以，建议使用 deepin 或者 ubuntu。

   大家可以在网上搜一搜相应的安装教程“bilibili”以及相应的博客，“Windows”系统使用虚拟机建议使用“VM”，“MAC”系统使用虚拟机建议使用 PD。由于使用虚拟机只能发挥出电脑性能的一部分，会损失一大部分性能，所以建议大家，前期安装使用摸索可以使用虚拟机，熟练以后可以使用双系统，双系统建议熟练以后再进行。

   下面我找一些教程，各自系统的官网也都有相应的教程，大家可以参考：

   虚拟机：

   http://www.epinv.com/post/12533.html

   https://blog.csdn.net/weixin_43465312/article/details/100233930

   双系统：

   https://www.deepin.org/installation/

   https://blog.csdn.net/baidu_36602427/article/details/86548203

### 安装前期准备

1. 由于 Linux 系统基本是通过命令终端来进行软件的安装、使用、文件的读取与修改，所以了解基本的 Linux 系统操作命令是非常别要的，如：“cd，rm，mkdir，vim，source，cp”等等一些基础命令。bilibili 上面有很多基础的入门视频，建议大家观看。

### 安装流程

安装 PSRCHIVE 需要联网，这是 psrchive 的官方网站：

http://psrchive.sourceforge.net/download.shtml

上面也有详细的教程，下面根据张颜容老师的博客

http://blog.sciencenet.cn/blog-640181-1228213.html

我补充一下 psrchive 的安装流程：

1. 首先打开，Linux 系统的终端。

2. 配置环境变量

   ```bash
   vim ~/.bashrc #用vim文本编译器 打开根目录下".bashrc"这个文件，这个文件是环境变量储存的地方
   dedit ~/.bashrc #vim文本编译器 有一定的门槛，也可以用dedit文本编译器，deepin系统自带的
   gedit ~/.bashrc #也可以用dedit文本编译器，ubuntu系统自带的
   ```

   在末尾添加以下以下文本：

   ```bash
   export PSRHOME=$HOME/Pulsar

   export PGPLOT_DIR=/usr/lib/pgplot5
   export PGPLOT_FONT=$PGPLOT_DIR/grfont.dat

   export TEMPO2=$PSRHOME/tempo2
   export PSRCAT_FILE=$PSRHOME/psrcat/psrcat.db

   export PATH=${PATH}:$PSRHOME/bin:$TEMPO2/bin

   # 上面就是讲这些软件的环境变量添加到系统中，简单讲就是告诉系统这个软件在哪，修改完后记得保存
   ```

   重启或：

   ```bash
   linux@psr:~$ source ~/.bashrc #"wsl@WIN"是你的用户名，这个是刷新环境变量，重置一下
   ```

3. 安装一些必要的库

   ```bash
   linux@psr:~$ sudo apt autoremove
   linux@psr:~$ sudo apt install -y autotools-dev autoconf libtool make g++ gfortran
   linux@psr:~$ sudo apt install -y csh libfftw3-dev pgplot5 libcfitsio-dev git

   #"sudo"是获取管理员权限，加上以后需要输入密码，linux输入密码的时候不可见，输完回车就可以
   ```

4. 安装 PSRChive

   ```bash
   linux@psr:~$ mkdir $HOME/Pulsar
   linux@psr:~$ cd $HOME/Pulsar
   linux@psr:Pulsar$ git clone git://git.code.sf.net/p/psrchive/code psrchive
   linux@psr:Pulsar$ cd psrchive/
   linux@psr:psrchive$ ./bootstrap
   linux@psr:psrchive$ ./configure
   linux@psr:psrchive$ make
   linux@psr:psrchive$ make check
   linux@psr:psrchive$ make install
   ```

5. 报错

   当 ./configure 运行失败，并报错为缺失一些安装包

   ```bash
   linux@psr:psrchive$ cd packages && make && cd - #添加可执行权限
   linux@psr:psrchive$ ./packages/fftw.csh #若缺失fftw 第2步已安装
   linux@psr:psrchive$ ./packages/cfitsio.csh #若缺失cfitsio 第2步已安装
   linux@psr:psrchive$ ./packages/pgplot.csh #若缺失pgplot 第2步已安装
   linux@psr:psrchive$ ./packages/epsic.csh #缺失epsic 此处需要安装
   linux@psr:psrchive$ ./packages/tempo2.csh #缺失tempo2 此处需要安装
   linux@psr:psrchive$ ./packages/psrcat.csh #缺失psrcat 此处需要安装
   linux@psr:psrchive$ ./configure
   linux@psr:psrchive$ make
   linux@psr:psrchive$ make check
   linux@psr:psrchive$ make install
   ```

6. 安装完成测试

   ```bash
   linux@psr:psrchive$ pazi -h
   #"pazi"是软件其中一个命令，"-h"是列出它的帮助文档，如果出现一列文档那就说明安装成功。
   ```
