---
title: OpenCV实现多图片背景消除及合并叠加
description: 使用Python OpenCV实现多个纯色背景的消除以及将其中被消除背景的图片叠加到另一幅图片上。这篇文章被做成了我的B站[在下小乔大家好]的[闲视]系列视频中，可以到我的B站观看。
categories:
 - python
 - 闲视
tags:
 - python
 - opencv
keywords:
  - python
  - opencv
  - 闲视
  - 背景消除
  - 图片合并
  - 叠加
  - B站
  - 视频
  - 教学
---

使用Python和OpenCV来实现多个图片纯色背景的消除以及将其中被消除背景的图片叠加到另一幅图片上。

视频教程链接：[https://www.bilibili.com/video/BV1bA411h74C/](https://www.bilibili.com/video/BV1bA411h74C/)

欢迎点赞收藏硬币三连~
达到如图下所示效果

![最终效果](https://cdn.jsdelivr.net/gh/ZhengqiaoWang/blog_resources_1/202209051941001.png)

## 准备工作

我们首先准备两张图片，其中一张是需要扣除背景的图片，如下图

![](https://cdn.jsdelivr.net/gh/ZhengqiaoWang/blog_resources_1/202209051941003.png)

而要移植的的目标背景图如下:

![](https://cdn.jsdelivr.net/gh/ZhengqiaoWang/blog_resources_1/202209051941002.png)

以及我们所需要的Python和OpenCV工具包，我选择的版本分别如下：

- Python 3.7.1 64-bit

那到目前为止，我们的准备工作就结束了。

## 分析实现

接下来，我们就需要对图片进行分析，一般普遍情况下，被扣除背景的图片中背景一般是绿色的，这个是经过长期实验论证的，但由于学校提供的实验室背景为蓝色，且蓝色为一种常见的大众衣着颜色，所以实际效果并不好，因此可根据现实情况进行分析。

### 扣除颜色分析

扣除颜色在上图为蓝色，其范围应该是深绿色到紫色这一大致范围，我们知道，常用的颜色扣除方式是采用OpenCV的inRange获取HSV得到二进制范围，在这里称为mask，再通过mask的非运算结果和原图进行与运算得到人像，但这样的方式会存在较大的噪点，所以我们可以采取drawContours的方式对微小细节进行排除。

## 代码说明

基于Python3和OpenCV，具体安装方式可参考博客内文章即可

```python
import cv2
import numpy as np
```

### 全局变量

```python
#设置人所在的位置和人的大小
people_height = 1200     #人像缩放高度
people_width = 800      #人像缩放宽度
people_loc_x = 900      #人像定位（从左上角开始）
people_loc_y = 0     #代码来源于http://me.zhengqiao.wang/
#打开人像图片
img = cv2.imread('C:/Users/wren1/Desktop/1.jpg')
#打开背景图片
img_background = cv2.imread("C:/Users/wren1/Desktop/TIM20190518132817.jpg")
```

### 初始化

```python
result = np.zeros((int(img_background.shape[0]),int(img_background.shape[1]),3),np.uint8)
result[:,:,0],result[:,:,1],result[:,:,2] = 255,0,0  #代码来源于http://me.zhengqiao.wang/
```

### 执行代码

```python
hsv=cv2.cvtColor(img,cv2.COLOR_BGR2HSV)
blue=cv2.inRange(hsv,np.array([78,43,46]),np.array([155,255,255]))
contours, hierarchy = cv2.findContours(blue,cv2.RETR_TREE,cv2.CHAIN_APPROX_SIMPLE) #代码来源于http://me.zhengqiao.wang/
for i in range(len(contours)):
    area = cv2.contourArea(contours[i])#代码来源于http://me.zhengqiao.wang/
    if area > 200:
        #填充为蓝色
        cv2.drawContours(img,[contours[i]],-1,(255,0,0),-1)
        pass
people = cv2.resize(img,(people_width,people_height))
result[people_loc_y:people_loc_y+people_height,people_loc_x:people_loc_x+people_width] = people #代码来源于http://me.zhengqiao.wang/
background = cv2.inRange(result,np.array([255,0,0]),np.array([255,0,0]))
tmp_back = cv2.bitwise_and(img_background,img_background,mask=background) #代码来源于http://me.zhengqiao.wang/
tmp_people = cv2.bitwise_and(result,result,mask=cv2.bitwise_not(background))
result = cv2.add(tmp_back,tmp_people)
```

### 显示结果

```python
#为了显示方便缩放一下结果
img_result = cv2.resize(result,(600,800)) #代码来源于http://me.zhengqiao.wang/
cv2.imshow("hole",img_result)
cv2.waitKey(0)
```

## 总结

当时写代码的时候没写注释，这种颜色扣取的方式是我一个暑假琢磨出来的，原本是用于RoboCup做场地识别和球门识别定位分离的，后来采用了BHuman的框架所以没用得上，这个效果还是很不错的，就是效率有些低，但对于一般计算机来说是足够的了。

有什么问题可以下方留言或者发邮件给我~
