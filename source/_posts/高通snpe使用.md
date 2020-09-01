---
title: 高通snpe使用
date: 2020-08-26 16:35:02
tags: 边缘计算
categories: 深度学习
---

版本snpe1.32  
支持Python的版本为Python2.7 和 Python3.4  
1. 安装Python3.4  
由于ubuntu18.04默认的Python版本为Python3.6 所以要先安装Python3.4
* 下载Python3.4 source code
  ```bash
wget https://www.python.org/ftp/python/3.4.10/Python-3.4.10.tgz
  ```
* 解压
  ```bash
tar -xzf Python-3.4.10.tgz
  ```
* 进入解压之后的文件夹
  ```bash
cd Python-3.4.10
  ```
* 配置安装选项
如果想快速安装，只配置安装位置即可。例如，我要把 Python3.8.3 安装在这个目录下：`/usr/local/python3.4`
```bash
./configure --prefix=/usr/local/python3.4
```
如果不在意安装耗时，可以设置优化选项`--enable-optimizations`
```bash
./configure --prefix=/usr/local/python3.8 --enable-optimizations
```

* 编译安装
  ```bash
make && make install
  ```

2.  设置系统默认Python环境为Python3.4
这一步要在`/usr/bin`目录下设置两个软链接文件：

> *   /usr/bin/python3.4
> *   /usr/bin/python3.4\-config


按照ubuntu 官方的教程来
```bash
ln \-s /usr/local/python3.4/bin/python3.4 /usr/bin/python3.4
ln \-s /usr/local/python3.4/bin/python3.4\-config /usr/bin/python3.4\-config 
ln \-s /usr/bin/python3.4 /usr/bin/python
```
  
这样在终端运行python
![terminal](https://i.loli.net/2020/08/26/wjukt59nBs1IQya.png)
> 最新高通官方做了更新Python支持3.5 同时也可关注官方手册
[https://developer.qualcomm.com/docs/snpe/setup.html](https://developer.qualcomm.com/docs/snpe/setup.html)

3. snpe 使用
   * 下载对应版本snpe这个很简单不做赘述了下载地址：
   [https://developer.qualcomm.com/software/qualcomm\-neural\-processing\-sdk](https://developer.qualcomm.com/software/qualcomm-neural-processing-sdk)
   * 解压snpe到合适的地方
   * 安装相关依赖
  ```bash
  sudo apt-get install python-dev python-matplotlib python-numpy python-protobuf python-scipy python-skimage python-sphinx wget zip
  ```
  * 检测所有依赖是否已经安装(必须)
  ```bash
  source ~/snpe-sdk/bin/dependencies.sh
  ```
  这里可能会提示下载一些包，选择安装就好
  * 检测所有的python依赖是否已经安装
  ```bash
  source ~/snpe-sdk/bin/check_python_depends.sh
  ```
  出现图片中的字样则表示Python以来安装好了，如果显示没有与之相对应的包使用
  ```bash
  sudo apt-get install XXX
  ```
  如果使用conda进行管理的话可以使用
    ```bash
  sudo conda install XXX
  ```
![terminal](https://i.loli.net/2020/08/26/UsgSQkFBl5boN8Y.png)

4. 转换模型
```bash
snpe-tensorflow-to-dlc --input_network $SNPE_ROOT/models/inception_v3/tensorflow/inception_v3_2016_08_28_frozen.pb --input_dim input "1,299,299,3" --out_node "InceptionV3/Predictions/Reshape_1" --output_path inception_v3.dlc --allow_unconsumed_nodes
```





















  



