---
layout:             post
title:                 NVIDIA GPU driver
subtitle:          在 Ubuntu 16.04 上安装英伟达驱动
date:      	         2018-03-31
author:             Shaw
header-img:     img/Nvidia-GeForce.jpg
catalog: 	         true
tags:
        - 硬件驱动
---

> 最近自己买各种电脑硬件，给女票组装了一部台式机做 machine learning。硬件中包括 [NVIDIA GeForce GTX 1060 6GB](https://www.nvidia.com/en-us/geforce/products/10series/geforce-gtx-1060/) 独立显卡。各个零件大致根据 [这个视频教程](https://www.youtube.com/watch?v=0bUghCx9iso) 进行硬件组装。操作系统是 Ubuntu 16.04。


> 本文参考 [这篇](https://blog.csdn.net/lhx_998/article/details/76135936) 和 [这篇](https://gist.github.com/dangbiao1991/7825db1d17df9231f4101f034ecd5a2b) 中文文档

1、驱动安装文件下载
-
到英伟达的 [驱动下载](https://www.geforce.com/drivers) 下载相应的驱动安装包（.run格式）。比如针对我买的 GPU，我就依次选择 GeForce → 10 Series → GTX 1060 → Linux 64-bt 选项。当前最新版本的文件是`NVIDIA-Linux-x86_64-390.48.run`，我把它下载保存到`~/Downloads`路径。

2、准备工作
-
- 卸载可能存在的旧版本 nvidia 驱动（对没有安装过 nvidia 驱动的主机，这步可以省略）
```sh
sudo apt-get remove --purge nvidia*
```
- nouveau禁止命令写入文件

```sh
sudo nano /etc/modprobe.d/blacklist-nouveau.conf
# 在文件 blacklist-nouveau.conf 中加入如下内容：
blacklist nouveau
blacklist lbm-nouveau
options nouveau modeset=0
alias nouveau off
alias lbm-nouveau off
```

- 调用指令禁止nouveau

```sh
echo options nouveau modeset=0 | sudo tee -a /etc/modprobe.d/nouveau-kms.conf
```

- 建立新的内核
```sh
sudo update-initramfs -u
```
- 重启
```sh
sudo reboot
```

3、运行驱动安装文件
-
- 进入tty模式
```sh
ctrl + alt + F1
```
- 关闭 x server（图形界面）
```sh
sudo service lightdm stop
sudo init 3
```

- 切换到 NVIDIA 安装包指定目录，赋予权限并进行安装
```sh
cd Downloads/
chmod u+x NVIDIA-Linux-x86_64-390.48.run
sudo sh NVIDIA-Linux-x86_64-390.48.run --no-opengl-files 
```
- 重启
```sh
sudo reboot
```

4、安装完毕
-