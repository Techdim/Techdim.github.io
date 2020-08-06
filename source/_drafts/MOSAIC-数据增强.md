---
title: MOSAIC 数据增强
tags:
---

mosaic数据增强理解起来十分方便，主要思想是将四张图片进行随机裁剪，再拼接到一张图上作为训练数据。原文中提到这样做的好处是丰富了图片的背景，并且四张图片拼接在一起变相地提高了batch_size，在进行batch normalization的时候也会计算四张图片，所以对本身batch_size不是很依赖，单块GPU就可以训练YOLOV4。
以下是我根据pytorch YOLOV4的代码对Mosaic数据增强进行的整理。