---
title: MOSAIC 数据增强
tags: [目标检测,数据增强]
categories: 算法剖析
---

mosaic数据增强理解起来十分方便，主要思想是将四张图片进行随机裁剪，再拼接到一张图上作为训练数据。原文中提到这样做的好处是丰富了图片的背景，并且四张图片拼接在一起变相地提高了batch_size，在进行BN的时候也会计算四张图片，所以对本身batch_size不是很依赖。

![](https://images1.freesion.com/21/82/8276ed46b207c70292e8eac3c6e17175.png)

YOLO-tensorflow的代码在[github](https://github.com/klauspa/Yolov4-tensorflow/blob/master/data.py)
简单的记录一下。