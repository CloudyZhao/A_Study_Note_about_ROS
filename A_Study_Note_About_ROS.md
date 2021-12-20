<!--
本文档由CloudyZhao编写
开始日期：2021年12月19日

编写方式：中文
特殊格式：首行缩进两字符( &emsp;&emsp; )
超链接格式：[Correct link syntax](https://www.example.com/)
-->

# ROS学习笔记

Written by **CloudyZhao**

## 1. ROS初识

### 1.1.1 ROS的特点

&emsp;&emsp;
ROS的核心——分布式网络，使用了 **TCP/IP** 的通信方式，实现了模块间点对点的松耦合链接，可以执行若干种类型的通信，包括基于 **话题(Topic)** 的 **异步** 通信，基于 **服务(Service)** 的 **同步** 通信，还有参数服务器上的数据存储等。总的来说，ROS主要有以下几个特点：

(1) 点对点的设计

&emsp;&emsp;
在ROS中，**每一个进程都以一个节点的形式运行**，可以分布于多个不同的主机。节点间的通信消息通过一个带有发布和订阅功能的RPC传输系统，从发布节点传送到接收节点。这种点对点的设计可以分散定位、导航等功能带来的实时计算压力，适应多机器人的协同工作。

(2) 多语言支持

&emsp;&emsp;
为了支持更多应用的移植与开发，ROS被设计成一种语言弱相关的框架结构。目前已经支持C++、Python、Java等多种不同的语言，也**可以同时使用不同语言来完成不同模块的编程**。

(3) 架构精简、集成度高

&emsp;&emsp;
机器人应用软件的复用性一直是一个巨大的问题。而ROS框架具有的模块化特点是**每个功能节点都可以进行单独编译**，并且使用**统一的消息接口**让模块的移植与复用更加便捷。同时，ROS中集成了大量已有开源项目的代码，例如OpenCV(Open Source Computer Vision Library)、PCL(Point Cloud Library)等。

(4)丰富的组件化工具包

&emsp;&emsp;
ROS采用组件化的方法将可视化工具与仿真软件集成到系统中并作为一个**组件直接使用**，例如3D可视化工具rviz(Robot Visualizer)、消息查看工具、物理仿真环境等组件。

(5)免费且开源

&emsp;&emsp;
ROS遵照的**BSD许可证**(Berkeley Software Distribution license)可给使用者较大的自由，允许其修改与重新发布其中的应用代码，甚至可以进行商业化的开发与销售。

### 1.1.2 ROS安装

#### (1) 操作系统与版本选择

&emsp;&emsp;
ROS目前只能在基于Unix的平台上运行，主要支持**Ubuntu操作系统**。与此同时，Microsoft Windows端口的ROS已经实现，但并未完全开发完成，笔者也很期待。

&emsp;&emsp;
此书中的ROS学习是基于**Ubuntu 16.04**操作系统进行的，采用的发行版本是**ROS Kinetic Kame**，发行日期为2016年5月23日。需要注意的是：这个发行版目前在 [ROS官网](https://wiki.ros.org/) 中已被标记为“**unsupported release**” ，生命结束(End of Life, EOL)时间为2021年4月。

#### (2) 安装流程与注意事项

ROS的安装过程在[ROS官网](<https://wiki.ros.org/cn/kinetic/Installation/Ubuntu>)中有详细的讲述，一步一步按照指示走就可以了，此处不赘述了。需要注意的是，即使按照官网步骤安装仍然会遇到一些问题，主要是网络连接问题，此处我推荐的方法是利用**全局代理**解决网络问题。

由于相关网络问题，采用官方源(packages.ros.org)会出现下载速度缓慢甚至无法下载的情况，从而导致无法顺利安装ROS。此处也可以按照官网中的指示选择镜像源（推荐 [TUNA](http://mirrors.tuna.tsinghua.edu.cn/ros/) 或 [USTC](http://mirrors.ustc.edu.cn/ros/) ）。如果仍然遇到连接问题，可尝试为`Ubuntu apt`换源，官网推荐的Ubuntu镜像源为 <https://mirrors.aliyun.com> ，但换源后依然会出现某些问题（测试时间：2021年12月20日），所以此处为 Ubuntu apt 换源并不推荐使用。具体问题如下所示。

更换**mirrors.aliyun.com**后，安装ROS时出现的问题：

```text
E: Failed to fetch http://mirrors.aliyun.com/ubuntu/pool/universe/l/lyx/fonts-lyx_2.1.4-2_all.deb  Connection timed out [IP: 221.231.81.242 80]
E: Failed to fetch http://mirrors.aliyun.com/ubuntu/pool/main/g/gcc-defaults/gfortran_5.3.1-1ubuntu1_amd64.deb  Connection timed out [IP: 221.231.81.242 80]
E: Failed to fetch http://mirrors.aliyun.com/ubuntu/pool/universe/o/openmpi/openmpi-bin_1.10.2-8ubuntu1_amd64.deb  Hash Sum mismatch
E: Failed to fetch http://mirrors.aliyun.com/ubuntu/pool/universe/s/sdformat/libsdformat4-dev_4.0.0-1ubuntu2_amd64.deb  Connection timed out [IP: 221.231.81.242 80]
E: Failed to fetch http://mirrors.aliyun.com/ubuntu/pool/main/z/zope.interface/python-zope.interface_4.1.3-1build1_amd64.deb  Connection timed out [IP: 221.231.81.242 80]
E: Failed to fetch http://mirrors.aliyun.com/ubuntu/pool/universe/p/pcl/libpcl-recognition1.7_1.7.2-14ubuntu0.1_amd64.deb  Connection timed out [IP: 221.231.81.242 80]
E: Unable to fetch some archives, maybe run apt-get update or try with --fix-missing?
```

解决方法及步骤如下：

1. 打开Ubuntu系统设置中的"软件和更新"，选择下载服务器为"Main server";

2. 打开代理，并且设置为**全局代理**;

3. 在先前的终端中重新依次输入以下两条命令：

```text
sudo apt-get update

sudo apt-get install ros-kinetic-desktop-full
```

此处利用代理不仅能够直接从官方源下载安装ROS，而且也能够顺利的完成初始化rosdep，不会因为网络连接问题而导致相关错误。

如果在输入`rosdep update`后，出现如下问题：

```text
ERROR: error loading sources list:
 <urlopen error <urlopen error EOF occurred in violation of protocol (_ssl.c:590)> (https://raw.githubusercontent.com/ros/rosdistro/master/galactic/distribution.yaml)>
```

不用惊慌，这是由于代理网络不稳定的原因，只要在终端中重新输入`rosdep update`即可。

## 2. ROS架构

## 3. ROS基础

## 4. ROS常用组件

## 5. ROS机器人平台搭建与建模仿真

## 6. ROS中的机器视觉

## 7. ROS中的机器语音

## 8. ROS中的机器学习

## 9. ROS机器人SLAM与自主导航

## 10. ROS进阶功能

## 11. ROS机器人实例

## 12. ROS 2
