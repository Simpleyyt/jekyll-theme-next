---
title: 使用PyOpenCV驱动树莓派CSICamera
description: 本篇文章主要介绍了我如何使用树莓派的PyOpenCV驱动CSI摄像头
categories:
 - raspberrypi
tags:
 - python
 - opencv
 - raspberrypi
keywords:
  - python
  - raspberrypi
  - csi
  - opencv
  - camera
  - pyopencv
  - 摄像头
  - 树莓派
date: 2018-03-07 00:00:00
updated: 2022-10-04 00:00:00
---

目前，物联网行业大热，机器视觉作为其中重要的一部分，被众多开发者喜爱。

Raspberry Pi(树莓派)是应用物联网的一大平台，与其摄像头相关技术很多，但OpenCV在使用树莓派摄像头的时候却出现无法读值的问题。

本文将使用Python+OpenCV+Rasp 3B 来完成树莓派CSI摄像头的配置使用

### 第一步 连接CSI摄像头

在购买Raspberry Pi自带的CSI摄像头时，一般会附送一套排线，我们可以根据引脚来进行物理安装。

如下图所示

![](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202210041101046.png)

### 第二步 开启摄像头功能

接下来我们需要开启摄像头的功能。一般有两种方法可供使用。

#### 方法1：使用可视化界面Raspberry Pi Config

这种方法适用于连接显示器、键盘鼠标，可进行可视化操作。或者远程桌面连接。

【Rasp】-【首选项】-【Rasp.. Config】-【Camera : Enable】

重启

#### 方法2：使用命令行

这种方法同样适用于可视化操作，也可以通过SSH或串口连接修改。

```shell
$ sudo raspi-config
```

然后我们再寻找【Enable Camera】，并将其设置为Enable

注：这一步我发现根据系统版本不同，设置位置不同，大家可以找一下。

退出

重启

### 第三步 调试摄像头

我们已经安装了摄像头并设置了摄像头，接下来我们要验证我们的设置，毕竟，万一你哪一步做错了呢~或者说，你摄像头买过来就是坏的呢？

这时候我们可以利用Raspberry Pi给我们的raspistill来验证是否能够使用。

```shell
$ raspistill -o test.jpg
```

这时候，我们的可视化界面会出现我们摄像头拍摄的画面，会存在几秒钟，同时，在/home/目录下会生成_test.jpg_文件，如果我们使用的是SSH之类的连接，我们可以通过第三方工具FileZilla来判断是否成功。

### 第四_步 安装picamera库

如果我们在第三步完成测试，那么我们就可以正常使用CSI摄像头了。

但是我们如果要使用OpenCV驱动这个摄像头，首先需要另一个库picamera

不过在此之前，我们得先安装好OpenCV不是么？验证OpenCV是否安装成功的方法。

```python
import cv2
```

且无报错，即为成功。然后我们需要安装picamera 了

```shell
pip install picamera
pip install "picamera[array]"
```

为什么有两个要安装的呢？根据一些网友给出的建议是，由于我们没有必要安装完整版的picamera，所以只安装numpy支持的那一部分就行了。不过呢，根据我的体验，直接安装完整版的，会包括第二行的那一部分。所以我也懒省事，以后不想折腾了。

### 第五步 利用Python OpenCV获取一个图像

接下来就是获取成果的时候了，我们当然要使用Python OpenCV来获取摄像头拍照的图片啦！

```python
#引用库
from picamera.array import PiRGBArray
from picamera import PiCamera
import time
import cv2

camera = PiCamera()
rawCapture = PiRGBArray(camera)
#给加载一个缓冲
time.sleep(0.1)
#抓取一个图片
camera.capture(rawCapture, format="bgr")
image = rawCapture.array
#显示一个图片
cv2.imshow("Image", image)
cv2.waitKey(0)
```

### 第六步 利用Python OpenCV获取视频流

同样，这就是我们的目的之一。

```python
#引用库
from picamera.array import PiRGBArray
from picamera import PiCamera
import time
import cv2
 
# 初始化摄像头
camera = PiCamera()
camera.resolution = (640, 480)
camera.framerate = 32
rawCapture = PiRGBArray(camera)
 
# 加载缓冲
time.sleep(0.1)
 
# 获取图片
for frame in camera.capture_continuous(rawCapture, format="bgr", use_video_port=True):
    image = frame.array
 
    # 显示
    cv2.imshow("Frame", image)
    key = cv2.waitKey(1) & 0xFF
 
    # 清空缓存
    rawCapture.truncate(0)
 
    # 如果按到键盘字母'q'就退出循环
    if key == ord("q"):
        break
```
