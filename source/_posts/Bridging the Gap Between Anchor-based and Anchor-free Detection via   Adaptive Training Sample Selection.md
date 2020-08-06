---
title: >-
  CVPR 论文Bridging the Gap Between Anchor-based and Anchor-free Detection via
  Adaptive Training Sample Selection
tags:
  - 深度学习
  - 目标检测
date: 2020-08-03 11:17:00
categories: 论文分析
---



&ensp;&ensp;在这篇文章中，作者首先指出了anchor-based和anchor-free方法最终训练结果的性能差异主要来来自于正负样本的选择。如果在训练的过程中采取相似的正负样本定义策略那么在最终的结果上是几乎相同的。也就是说如何选取正负样本对于目标检测任务是十分重要的。因此作者了提出一种自动选择正负样本的算法（ATSS） 


*   **论文地址**：[https://arxiv.org/abs/1912.02424](https://arxiv.org/abs/1912.02424)
*   **代码地址**：[https://github.com/sfzhang15/ATSS](https://github.com/sfzhang15/ATSS)  

### 主要工作

*   指出anchor\-free和anchor\-based方法的根本差异主要来源于正负样本的选择
*   提出ATSS( Adaptive Training Sample Selection)方法来根据对象的统计特征自动选择正负样本
*   证明每个位置设定多个anchor是无用的操作
*   不引入其它额外的开销，在MS COCO上达到SOTA

### anchor-based于anchor-free

1. anchor-based   
   主要分为两阶段(two-stage)与一阶段(one-stage)两阶段代表性的算法有Faster-RCNN其优点是准确率高缺点在于运行效率低,一阶段的代表性的算法为SSD其优点在于效率高但是相对而言准确率更低。
2. anchor-free  
    由于FPN和focal loss的提出 anchor-free取得了不俗的成绩，主要两种具有代表性的方法为keypoint-based method 和 center-based method.

### 工作1--比较RetinaNet和FCOS比较这两种算法的主要差异：

1.   RetinaNet在特征图上每个点铺设多个anchor，而FCOS在特征图上每个点只铺设一个中心点，这是数量上的差异。

2.  RetinaNet基于anchor和GT之间的IoU和设定的阈值来确定正负样本，而FCOS通过GT中心点和铺设点之间的距离和尺寸来确定正负样本。这1点可以从下图的对比中看到，牛这张图像中蓝色框和点表示GT，红色框表示RetinaNet铺设的anchor，红色点表示FCOS铺设的点，左右两边类似表格上的数值表示最终确定的正负样本，0表示负样本，1表示正样本。

![](https://img-blog.csdnimg.cn/20200305112436231.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMzc1NjA5,size_16,color_FFFFFF,t_70)

3. RetinaNet通过回归矩形框的2个角点偏置进行预测框位置和大小的预测，而FCOS是基于中心点预测四条边和中心点的距离进行预测框位置和大小的预测。这1点可以从下图的对比中看到，蓝色框和点表示GT，红色框表示RetinaNet的正样本，红色点表示FCOS

![](https://img-blog.csdnimg.cn/20200305112504214.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMzc1NjA5,size_16,color_FFFFFF,t_70)


为了降低差异性，对比正负样本定义和回归开始状态的差异，将RetinaNet的anchor数改为1，由于FCOS加入了很多trick，这里将RetinaNet与其进行对齐，包括GroupNorm、GIoU loss、限制正样本必须在GT内、Centerness branch以及添加可学习的标量控制FPN的各层的尺寸。

在经过对齐后，仅剩两个差异的地方：(i) 分类分支上的正负样本定义 (ii) 回归分支上的bbox精调初始状态(start from anchor box or anchor point)




[![实验结果](https://s1.ax1x.com/2020/08/04/a0A0kq.png)](https://imgchr.com/i/a0A0kq)




对上面的差异进行交叉实验，发现相同的正负样本定义下的RetinaNet和FCOS性能几乎一样，不同的定义方法性能差异较大，而回归初始状态对性能影响不大。所以，基本可以确定正负样本的确定方法是影响性能的重要一环。



### 工作2--ATSS(Adaptive Training Sample Selection)：


![ATSS伪代码](https://s1.ax1x.com/2020/08/04/a0Ay1U.png)

##### ATSS步骤

1. 对于每个输出的检测层，选计算每个anchor的中心点和目标的中心点的L2距离，选取K个anchor中心点离目标中心点最近的anchor为候选正样本（candidate positive samples）
2. 计算每个候选正样本和groundtruth之间的IOU，计算这组IOU的均值和方差
3. 根据方差和均值，设置选取正样本的阈值：t=m+g ；m为均值，g为方差
4. 根据每一层的t从其候选正样本中选出真正需要加入训练的正样本
5. 训练


### 算法设计原则：
1. 在RetinaNet中，anchor box与GT中心点越近一般IoU越高，而在FCOS中，中心点越近一般预测的质量越高

2. 均值$m_g$表示预设的anchor与GT的匹配程度，均值高则应当提高阈值来调整正样本，均值低则应当降低阈值来调整正样本。标准差$v_g$表示适合GT的FPN层数，标准差高则表示高质量的anchor box集中在一个层中，应将阈值加上标准差来过滤其他层的anchor box，低则表示多个层都适合该GT，将阈值加上标准差来选择合适的层的anchor box，均值和标准差结合作为IoU阈值能够很好地自动选择对应的特征层上合适的anchor box

3. 若anchor box的中心点不在GT区域内，则其会使用非GT区域的特征进行预测，这不利于训练，应该排除


3. 根据统计原理，大约16%的anchor box会落在$[m_g+v_g,1]$，尽管候选框的IoU不是标准正态分布，但统计下来每个GT大约有$[0.2 * k\mathcal{L}]$个正样本，与其大小和长宽比无关，而RetinaNet和FCOS则是偏向大目标有更多的正样本，导致训练不公平

4. ATSS仅有一个超参数k，后面的使用会表明ATSS的性能对k不敏感，所以ATSS几乎是hyperparameter\-free的。
![不同k值的AP](https://s1.ax1x.com/2020/08/04/a0AvNt.png)





### 结论以及实验结果

论文指出one-stage anchor-based和center-based anchor-free检测算法间的差异主要来自于正负样本的选择，基于此提出ATSS(Adaptive Training Sample Selection)方法，该方法能够自动根据GT的相关统计特征选择合适的anchor box作为正样本，在不带来额外计算量和参数的情况下，能够大幅提升模型的性能，十分有用

1. 将RetinaNet中的正负样本替换为ATSS，AP提升了2.9%，这样的性能提升几乎是没有任何额外消耗的
2. 在FCOS上的应用主要用两种：lite版本采用ATSS的思想，从选取GT内的anchor point改为选取每层离GT最近的top k个候选anchor point，提升了0.8%AP；full版本将FCOS的anchor point改为长宽为8S的anchor box来根据ATSS选择正负样本，但仍然使用原始的回归方法，提升了1.4%AP。两种方法找到的anchor point在空间位置上大致相同，但是在FPN层上的选择不太一样。从结果来看，自适应的选择方法比固定的方法更有效



