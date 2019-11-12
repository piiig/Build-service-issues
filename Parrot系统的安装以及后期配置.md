---
title: Parrot系统的安装以及后期配置
date: 2019-06-03 09:52:27
tags: parrot
categories: 搭建
---
# Parrot OS
Parrot OS是一款基于Debain的操作系统,他有需要的黑客工具,以及好看的UI出名
![](http://120.79.93.103/tp/1.jpg)
![](http://120.79.93.103/tp/3.jpg)
## Parrot系统的安装
安装的方法大致都一样，网上都有教程，我只说特殊的几点
1.制作U盘不能使用UltraISO因为Parrot系统写入方式不同，推荐使用Etcher写入
![](http://120.79.93.103/tp/4.jpg)
2.启动界面卡着的问题，电脑有N卡，需要禁用N卡，
请教了嘉神，在选择进入的界面按E，进入编辑，在linux开头的一行中，在quiet splash后加上nouveau.modeset=0 然后F10进入就可以了
因为上面那个样子不是永久禁用N卡，需要进去以后``vim /etc/modprobe.d/blacklist-nouveau.conf`` 把下面一段复制进去
```
blacklist nouveau
blacklist lbm-nouveau
options nouveau modeset=0
alias nouveau off
alias lbm-nouveau off
```
强制关闭 ``echo options nouveau modeset=0 | sudo tee -a /etc/modprobe.d/nouveau-kms.conf``
这之后，更新配置试生效：``update-initramfs -u``

## 进入系统后先换源
因为系统使用的源是他们自带的，我们下载速度会非常的慢
源的位置：``/etc/apt/sources.list.d/parrot.list``
备份源文件``$ sudo cp /etc/apt/sources.list.d/parrot.list /etc/apt/sources.list.d/parrot.list.bak``
更改源文件``$ sudo vim /etc/apt/sources.list.d/parrot.list``
清华大学镜像源``deb https://mirrors.tuna.tsinghua.edu.cn/parrot/ parrot main contrib non-free``
然后更新源``sudo apt-get update``

## 字体设置
系统中文显示问题,打开图形界面``dpkg-reconfigure locales``
空格键选中``en_US.UTF-8``,``zh_CN.UTF-8``，然后回车就可以了

## 搜狗输入法
parrot自带有fcitx,我们只用下载搜狗拼音的deb包就可以
https://pinyin.sogou.com/linux/?r=pinyin
安装包 ``sudo dpkg -i xxx.deb``(这里是自己下载的deb包,下面的xxx.deb都代表为自己下载的包)
安装依赖 ``sudo apt-get install -f``

## wps
下载 https://www.wps.cn/product/wpslinux/
安装包 ``sudo dpkg -i xxx.deb``

## ssr
在ssr用的是electron，github有资源,parrot用deb包就可以
下载 https://github.com/qingshuisiyuan/electron-ssr-backup/releases
``sudo apt install libcanberra-gtk-module libcanberra-gtk3-module gconf2 gconf-service libappindicator1``
``sudo dpkg -i xxx.deb``
配置教程在此:https://github.com/qingshuisiyuan/electron-ssr-backup/blob/master/Ubuntu.md

## wine64
wine是用来使用windows软件的，卸载原来的wine(自带wine是支持32位)
```bash
sudo apt remove wine
sudo apt autoremove
rm -rf /home/username/.wine/
```   
然后安装wine64 ``sudo apt-get install wine64``
安装后使用出现问题wine: ‘/home/zbw/.wine’ is a 64-bit installation, it cannot be used
解决``mv ~/.wine ~/.wine64 && winecfg``重新配置。
使用方法 ``wine64 xxx.exe``

## crossover 可以用来运行Windows软件
下载 https://www.codeweavers.com/products
安装包 ``sudo dpkg -i xxx.deb``
## 网易云音乐
下载 https://music.163.com/#/download
安装包 ``sudo dpkg -i xxx.deb``    

## 谷歌浏览器
下载 https://www.google.cn/chrome/
安装包 ``sudo dpkg -i xxx.deb``
安装所需依赖 ``sudo apt-get install -f``

## sublime text
下载压缩包 http://www.sublimetext.com/3
下载安装包解压即可

## C/C++使用clion
http://www.jetbrains.com/clion/download/#section=linux

## jdk配置
https://blog.csdn.net/xl_1851252/article/details/82053217

## java使用IDEA
http://www.jetbrains.com/idea/
