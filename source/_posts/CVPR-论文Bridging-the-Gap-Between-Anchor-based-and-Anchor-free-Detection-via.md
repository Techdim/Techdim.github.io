---
title: CVPR 论文Bridging the Gap Between Anchor-based and Anchor-free Detection via Adaptive Training Sample Selection
date: 2020-08-03 11:17:00
tags: [深度学习,目标检测]
---
在这篇文章中，作者首先指出了anchor-based和anchor-free方法在训练的过程中如何定义正负样本的。如果在训练的过程中采取相似的正负样本定义策略那么在最终的结果上是几乎相同的。也就是说如何选取正负样本对于目标检测任务是十分重要的。提出一种自动选择正负样本的算法（ATSS）

anchor bansed 
两阶段与一阶段的优缺点

由于FPN和focal loss的提出 

anchor  free 的两种说法


文章提出一个观点在特征图上进行均匀采样并不是必须的

指出anchor based 方法和anchor free 方法的本质区别是在如何选择正样本和负样本身上