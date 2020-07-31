---
title: tfrecord 读取多个文件
date: 2020-07-13 15:39:52
tags:
---

```Python
tfrecord_file_path = '/train/*.tfrecords' #train是存放tfrecord的文件夹
filename_queue = tf.train.string_input_producer(
                              tf.train.match_filenames_once(tfrecord_file_path),
                              shuffle=True, num_epochs=None) #None表示没有限制

reader = tf.TFRecordReader()
_, serialized_example = reader.read(filename_queue)   #返回文件名和文件
features = tf.parse_single_example(serialized_example,
                                       features={XXXXXXX})  #取出XXXXXXX


```
