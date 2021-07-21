---
title: 在Ubuntu 20.04.2 LTS操作系统下安装mendeley并解决相关问题
declare: true
tags:
  - 技术
  - ubuntu
  - mendeley
  - 问题解决
abbrlink: 25311
date: 2021-06-19 12:42:56
---
>本文介绍了在Ubuntu 20.04.2 LTS操作系统下安装mendeley的整个过程，并且解决了在mendeley中无法进行中文输入的问题
<!--more-->
- **全文大约450字**

mendeley是一款全平台的文献管理系统，对于ubuntu用户来说可能也是唯一的免费文献管理系统，所以这个博文将介绍在ubuntu系统上下载mendeley，以及解决无法中文输入的问题。

>相关软件参考配置：
>操作系统：Ubuntu 20.04.2 LTS
>mendeley：1.19.8
>fcitx：4.2.9.7

## mendeley的安装
对于mendeley的安装，如果源里面有的话自然是非常方便的，如果没有的话也不要紧。

首先更新源
```
sudo apt update
```

之后下载mendeley
```
sudo apt install mendeleydesktop
```
如果没有找到软件的话就需要在官网下载，[下载链接在这里](https://www.mendeley.com/download-desktop-new/)。如果源里面有的话就可以直接打开了。

之后cd到下载路径，使用安装软件包命令
```
dpkg -i mendeleydesktop_1.19.8-stable_amd64.deb（之后软件包更迭可能不是这个名字了）
```

之后就可以打开mendeley啦！

进入mendeley首先要注册一个账号，但是可能是由于网络服务器的问题，没有办法很及时的将注册的账号同步到mendeley桌面版，在桌面版可能需要稍等几个小时才可以正常登录。

## 解决无法中文输入的问题
对于ubuntu19之前的用户可能可以将系统自带的
```
/usr/lib/x86_64-linux-gnu/qt5/plugins/platforminputcontexts/libfcitxplatforminputcontextplugin.so
```
拷贝到mendeley安装目录下就可以支持中文输入，但是对于ubuntu20.04的用户，这个方法不再适用。

亲测有效的做法是将自行编译的 libfcitxplatforminputcontextplugin.so 文件直接拷贝到mendeley路径下，在此感谢yinflying的努力与无私共享。

首先安装git
```
sudo apt update
sudo apt install git 
```
打开[jinflying的BlogSource仓库](https://github.com/yinflying/BlogSource)，将仓库内容克隆到本地
```
git clone https://github.com/yinflying/BlogSource.git
```
将自行编译的 libfcitxplatforminputcontextplugin.so 文件直接拷贝到mendeley路径下
```
sudo cp lib-fcitx-plugin/debian.sid.20171223/libfcitxplatforminputcontextplugin.so.for.MendeleyDesktop /opt/mendeleydesktop/plugins/qt/plugins/platforminputcontexts/libfcitxplatforminputcontextplugin.so
```
重新启动mendeley

如此应该就可以正常进行中文的输入。

## 参考博客
[Ubuntu20.04 下使用基于 fcitx 的输入法时，Mendeley 无法输入中文的解决方案](https://www.cnblogs.com/ArrowKeys/p/13252299.html)
[解决 Debian 中 Mendeley Desktop 和 RStudio 无法使用 fcitx 输入中文的问题](https://jiangjun.link/post/debian-mendeley-rstudio-fcitx/)

