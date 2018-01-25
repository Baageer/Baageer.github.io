---
title: MobileNet论文
description: MobileNet论文.
categories:
 - tutorial
tags:
---

> 新建博客。

### MobileNet

MobileNet提出了depth-wise separable convolutions的卷积结构来构建轻量的网络结构和两个全局参数来控制网络的大小。MobileNet对于机器性能和精确度做出折中，让神经网络可以更好的在移动设备和嵌入式设备上更好的运行。

##### Depth-wise separable convolutions

一般的卷积操作,输入为$D_F\times{D_F}\times{M}$，用N个$D_K\times{D_K}$的卷积核对输入进行卷积操作，输出$D_G\times{D_G}\times{N}$的特征图。

depth-wise separable convolutions的做法是用$D_K\times{D_K}$的卷积核对输入的M个通道分别做卷积操作，将M个卷积结果合并起来，在用N个1X1的卷积核对合并结果进行卷积，得到$D_G\times{D_G}\times{N}$的特征图。

一般卷积的计算量是$D_K\cdot{D_K}\cdot{M}\cdot{N}\cdot{D_F}\cdot{D_F}$

depth-wise separable convolutions的计算量是$D_K\cdot{D_K}\cdot{M}\cdot{D_F}\cdot{D_F}+M\cdot{N}\cdot{D_F}\cdot{D_F}$

$\frac{D_K\cdot{D_K}\cdot{M}\cdot{D_F}\cdot{D_F}+M\cdot{N}\cdot{D_F}\cdot{D_F}}{D_K\cdot{D_K}\cdot{M}\cdot{N}\cdot{D_F}\cdot{D_F}}=\frac{1}{N}+\frac{1}{D_K^2}$

3X3 Conv结构

3X3 Depthwise Conv结构

MobileNet 结构

##### Width Multiplier 宽度因子

Width Multiplier α控制每一层的网络结构，改变α 可以控制网络的宽度。α是控制输入特征的通道数和输出特征的通道数，depth-wise separable convolutions在加入α因子后，计算量变为

$D_K\cdot{D_K}\cdot{αM}\cdot{D_F}\cdot{D_F}+αM\cdot{αN}\cdot{D_F}\cdot{D_F}$

α的取值为(0,1]，一般为1、0.75、0.5和0.25，使用了α后会产生新的网络，不同α值的网络预训练模型不能共用，需要重新训练模型。

##### Resolution Multiplier 分辨率因子

Resolution Multiplier ρ控制每层输入特征的分辨率，从而减少计算量。depth-wise separable convolutions在加入α和ρ因子后，计算量变为

$D_K\cdot{D_K}\cdot{αM}\cdot{ρD_F}\cdot{ρD_F}+αM\cdot{αN}\cdot{ρD_F}\cdot{ρD_F}$

ρ的取值为(0,1]。

