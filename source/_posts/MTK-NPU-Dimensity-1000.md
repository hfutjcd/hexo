---
title: MTK NPU Dimensity 1000
author: Hunter
top: false
cover: false
toc: true
mathjax: true
keywords: MTK NPU Dimensity 1000
date: 2020-04-06 15:00:34
img: image_20200317173924.png
password:
summary:
tags: [MTK,NPU,Dimensity 1000]
categories: NPU
reprintPolicy:
typora-root-url: MTK-NPU-Dimensity-1000
---

#  **A 3.4-to-13.3TOPS/W 3.6TOPS Dual-Core Deep-Learning Accelerator for Versatile AI Applications in 7nm 5G Smartphone SoC**

[*论文出处 ISSCC2020-07_visuals.pdf*](http://bbs.eetop.cn/forum.php?mod=attachment&aid=NzUxNzA4fGZjNTJhNGRlfDE1ODQ0Mzc3MTR8MHw4NzU1Mzk%3D)

Dimensity 1000是MTK的首款真正的AI芯片，通过ISSCC论文PPT看了一下架构，在这里做一下记录：

## 整体架构如下图所示

![](image_20200317173924.png?v=1&type=image&token=V1:IBK0ES23HbSMI2BTMl0yjB6oZizOdEg3nRX3ZSDaJBw)

从图上看，Dimensity1000采用双DLA核，与业界的趋势一致，采用双核提高算力。两个DLA可以直接通信，不需要AP_MCU的参与，提高了通信速度。

每个DLA中有：

四个处理单元： CE， 1DE，2DE，CMDE。

两个L1 Buffer，一个L2 Buffer。

两个DMA: CBLD,SBDMA

下面详细的看一下每个单元的功能：



## 主要的处理单元 Convolution Engine（CE）

![](image_20200317180543.png?v=1&type=image&token=V1:AfQUlxkJOZRq6H-vcG9V-6Mm7SQJFQN7VR3EV0bbiN4)	
1. Convolution Buffer Loader (CBLD)

   一个4D DMA引擎，支持stride的模式，主要是从L2搬运数据到CB，也可以从SB搬运到CB（应该是为layer-fusion特性做的）。支持weight压缩。

2. convolution Buffers(CB)

   每层输入数据的activation、weight。CB的大小对Data-reuse的性能影响较大，此处没有介绍具体大小。

3. CONV Data Dispatcher(CDD)

   将CB的数据搬入CE，除了数据搬运，应该还支持数据格式转换。同时还有Register Banks，减少对CB的访问，一些固定或变动少的数据可以放到register中。

4. Convolution Engine(CE)

   CE是DLA的主要网络计算单元，执行卷积计算，其中还有个小的AQU核作ASYMM-Q计算。提高了ASYMM-Q的性能。

## 非卷积处理器

![](image_20200317181655.png?v=1&type=image&token=V1:B6yrkRnOEqhyhE0F0bxJStTqkSAB5Faf965_xGP06go)



1. 1D Engine

   激活函数（ReLu,PReLu,…）

   数学计算（乘，加）

2. 2D Engine

   Pooling函数（Avg,Max),这里看不懂与1D分开的优势在哪里。在1D和2D CU之间没有直连通道。2D操作需要额外进出一次L1。个人猜测是为了减少2D CU的寄存器数量，2D操作需要缓存数据，1D到2D的数据需要先缓存足够的数据才可以计算。减少2D CU的寄存器，那就要利用L1，也就没必要增加1D到2D CU的直连通道了。

3. L1 用于1D 2D数据交换

4. 4D DMA数据传输，支持stride，外存和SB之间。一个双向DMA，输出计算结果，输入应该是1D,2D引擎的参数。

## 控制处理器

![](image_20200317182434.png?v=1&type=image&token=V1:QVaTT62aonJeOg6-PHx-lEdqZz1hveH9bCUBb3Q98r8)

1.Common Engine 整个DLA的控制单元，控制各个核的运行，数据的同步等。

1. 加载代码
2. 解码，发送指令到其他CE
3. 处理数据依赖和资源冲突

## 简单网络计算流程

![](image_20200317182723.png?v=1&type=image&token=V1:KyWTwouEy4X8JgKWDczOY1WHdzBjsRwZlLqLtHyj-jI)

一个简单的卷积流程实例，主要表达通过减少外存访问见到latency，提高性能。

## 数据复用

![](image_20200317183017.png?v=1&type=image&token=V1:QNZO0cEdaIdyKM8VGM2IyH8fxuMKS6Kbk_mFWRp7piY)



数据复用，CE规格： CU**CUG**Cores=32**16**2



数据复用可以减少50%的weight对CB的访问，减少16%的数据CB访问。（不太理解16%怎么算的）

## AQ 量化

![](image_20200317190626.png?v=1&type=image&token=V1:yaVWDIPmCgBtppOKaJsMjAOsdp7Cj3xVXP2Vn9BKOY0)



## 权重压缩和跳零

![](image_20200317190648.png?v=1&type=image&token=V1:zhmpoZ84k1-G-Fs1Zc7lnWqe03owPCkyvmmtNAO_wMc)



支持权值压缩和跳零，降低87%的带宽和71%的计算量。权重压缩是全网络支持。**跳零只是在FC层支持**。



## tile方式降低带宽

![](image_20200317190716.png?v=1&type=image&token=V1:lDvIPqTzchephrtdJEm1uUDV8s0hwy98Eba06t1JtlI)



通过输出Tile方向降低带宽

​	C Chanel 优先，可以data复用，weight reload

​	X-Y Chanel 优先，可以weight复用，data reload。

​	可以指定优先方式，也可以编译自动搜索。

## layer fusion 降低带宽



![](image_20200317190734.png?v=1&type=image&token=V1:IGvQCtukSIoQPGPHmlYe5dnfpXkdvDUK0hBJDKy0vaI)



多层融合降低带宽

先遍历层，利用网络中Featuremaps生命周期端的层。（看不懂，depthwise，pointwise）

DLA 本身支持两层融合

通过L2实现多层融合

## 多核计算

![](image_20200317190807.png?v=1&type=image&token=V1:rs1xCvxjYH4xp4dmTHrFA5EU8_NTfIUv9ILq0x1KBkE)



多核合作方式



处理不同操作(按网络或层划分任务)

1. 批处理，每个核单独跑网络

2. 按层划分每个核跑不同的层。

相同操作（层内划分任务）

1. 数据并行，权值复用

2. 数据复用，权值并行

## 总结

![](image_20200317190835.png?v=1&type=image&token=V1:qziprjSAeusJWdfQlgPF0w9bYePF7LQxm-nNFH4zqvQ)

![](image_20200317190939.png?v=1&type=image&token=V1:JAjlPMzYQA8uUUQfKRQjEqjXu1-TIC4t-AZMVkgn5Zs)

## 性能功耗对比

![](image_20200317190919.png?v=1&type=image&token=V1:bbhUVPMLbnUm0ZCiip0JXlOmI0oyQ-jnS9uMaG9z6-k)

