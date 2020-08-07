---
title: Python 偏函数
date: 2020-07-15 14:43:09
tags: Python
categories: 编程学习
---

https://baijiahao.baidu.com/s?id=1613459698249266824&wfr=spider&for=pc

偏函数（Partial function）是通过将一个函数的部分参数预先绑定为某些值，从而得到一个新的具有较少可变参数的函数。在Python中，可以通过functools中的partial高阶函数来实现偏函数功能。

![](https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=2495948681,1851434534&fm=173&app=25&f=JPEG?w=640&h=360&s=F21C7E8657A3D8E45A2B826E03007078)

目前，在网上可以找到很多关于functools.partial用法的文章和例子。比如下面这个：

 <!-- more -->

这个例子比较好地展示了functools.partial的用法，但是并没有讲清楚偏函数究竟应该用在什么样的场景中，总给人一种屠龙之术，华而不实的感觉。

今天，小编就带大家通过几个实用的例子，来分析一下，善用functools.partial将会给我们的代码带来怎样的变化。

实例1：用functools.partial生成自己的专属函数

我们在编码时经常会遇到这样的场景，即根据一个字符串的内容而采取不同的处理逻辑，就像下面这样：

![](https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=627977678,4143844896&fm=173&app=25&f=JPEG?w=640&h=187&s=FD9CED1A9BE44D031A6440DE0000C0B2)

初看之下，这种写法也许还过得去。但是时间一长，你可能就忘了这些正则表达式究竟是干什么的了。于是，我们做了下面的重构：

![](https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=4134725729,877781352&fm=173&app=25&f=JPEG?w=640&h=394&s=6890ED1A591EC4CE10FC85DA000080B0)

这样看起来感觉好多了。事实上，如果只有这三个函数的话，我是可以接受目前的写法的。但是，如果你的代码中有几十个类似的用于判断字符串模式的函数，那么就需要在一个地方把它们统一管理起来，于是就有了下面的写法：

![](https://ss1.baidu.com/6ONXsjip0QIZ8tyhnq/it/u=3431265541,1855429895&fm=173&app=25&f=JPEG?w=640&h=249&s=ED8AED1A93C84D434A4528DA000050B2)

在这段代码中，我们通过functools.partial将re.search函数与不同的正则表达式绑定，从而得到了一系列供我们使用的专属函数。通过这种方法，不但使得代码更加简练，而且提高了可读性。

实例2：用partial生成具有继承关系的辅助对象

假设我们现在要写一段处理ajax请求的代码，重构前的代码是长这个样子的：

![](https://ss1.baidu.com/6ONXsjip0QIZ8tyhnq/it/u=1945502743,509136055&fm=173&app=25&f=JPEG?w=640&h=254&s=CD90ED1A9F18404350D195DA000080B1)

这段代码主要有以下几个问题：

每次构造HttpResponse对象时，都需要传入"application/json"作为参数

每次都需要调用json.dumps()

重复出现的状态码

以上问题使得这段代码看起来不够精炼，占用了较大篇幅但实际上没有做太多事情。

所以，我们重构的第一步是要抽象出一个JsonResponse对象来承载返回值：

![](https://ss1.baidu.com/6ONXsjip0QIZ8tyhnq/it/u=1171901338,1190005407&fm=173&app=25&f=JPEG?w=640&h=100)

经过第一步重构后的代码如下：

![](https://ss0.baidu.com/6ONWsjip0QIZ8tyhnq/it/u=1945544406,2844630315&fm=173&app=25&f=JPEG?w=639&h=221&s=CD92ED1A9F8048414A74A0DA0000C0B1)

所有返回HttpResponse 的地方都被我们新引入的JsonResponse所替代。

接下来，通过functools.partial，我们可以对Response做进一步的抽象，生成一系列JsonResponse的“子类”：

![](https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=3363152156,2753786029&fm=173&app=25&f=JPEG?w=640&h=174&s=CD82ED1ACD6549035EC1C9DB0000C0B0)

最终，重构后的代码如下：

![](https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=2288168420,2515930840&fm=173&app=25&f=JPEG?w=640&h=224&s=CD98ED129BF85C035A7420DA0000C0B1)

这样，我们最大限度地减少了冗余代码，使代码精炼易读。

我们再来看最后一个例子，看看partial是如何让代码变得简练的。

实例3：Django emails

![](https://ss1.baidu.com/6ONXsjip0QIZ8tyhnq/it/u=2126231191,3446601500&fm=173&app=25&f=JPEG?w=640&h=364&s=AD82ED121D9DCCCE10F90DDE0000C0B2)

看了今天的例子，大家是不是觉得Python提供的partial工具非常的好用呢？不如赶快打开电脑试一下吧。