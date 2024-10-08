---
title: 随笔：Windows上的Docker管理
tags:
  - Docker
  - 云计算
  - 虚拟机
  - Windows
categories:
  - 云计算
abbrlink: 8ce277fd
date: 2023-06-17 14:06:32
---


## 随笔：Windows上的Docker管理
较新版本的Windows上的Docker可以运行在WSL Backend上，意思是*Docker服务运行在WSL支持的Linux“虚拟机”上“。这样的好处是接口完全统一了，但问题是Docker整个工作目录和所有数据目录都是WSL虚拟磁盘的一部分了。

默认的Docker WSL后端包含两个发行版：
- 运行Docker服务本身的发行版`docker-desktop-`
- 用于给容器使用的Linux内核`docker-desktop-data`

容器所挂载的卷默认存放在`docker-desktop-data`虚拟磁盘上。换句话说，**Windows上Docker容器的数据实际上全部集中在`docker-desktop-data`虚拟磁盘文件上**

问题是`docker-desktop-data`的默认路径是`C:\Users\99634\AppData\Local\Docker\wsl\data\ext4.vhdx`，因此数据默认全部在C盘。

如何修改这样行为，或者如何让容器能够挂载宿主文件系统中的目录：（待完善）