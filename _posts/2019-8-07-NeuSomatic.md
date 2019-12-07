---
layout: post
title: '深度学习应用系列——NeuSomatic：鉴定体细胞突变'
date: 2019-08-07
author: JShuffle
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
categories: 深度学习
tags: 深度学习
---

# NeuSomatic



## 论文介绍

​	本次阅读的论文是19年4月发表在nature communications上的一篇文章。论文题目：**Deep convolutional neural networks for accurate somatic mutation detection**
​	简介：在癌症分析中，准确检测体细胞突变仍然是一个挑战。本文提出了一种基于卷积神经网络的体突变识别方法——NeuSomatic[1]。该方法在不同的测序平台、测序策略、肿瘤纯化等方面均优于以往的方法。NeuSomatic把序列拆分并汇总成小矩阵 ,运用超过一百个特性来学习突变信号。它可以作为一个独立模型检测体细胞突变，也可以结合现有的方法来实现高精度识别。

## 背景

​	突变类型分为体细胞突变和生殖系突变。

​	生殖系突变是遗传父母基因组得到的，而体细胞突变则是因环境因素等改变自发产生的。如果体细胞突变发生在原/抑癌基因等一些影响肿瘤细胞发生的因素，很可能会产生肿瘤。此外，精确地识别体细胞突变对肿瘤的精准用药治疗也有重要的指导。因此，高灵敏度/特异性的算法用于肿瘤体细胞突变检测非常重要。

​	一些因素使得肿瘤的体细胞突变检测变得很困难：

1.肿瘤与正常组织细胞的交叉污染。

2.肿瘤异质性。

3.测序误差。

4.测序覆盖深度。这些因素导致了最终假阳性很高。

​	现有的模型：MuTect2,VarScan2,Strelka2等，大多数基于一些统计机器学习算法。这些方法只能对某些特定类型的样本和测序手段起到不错的效果，缺少泛化能力；此外，传统的机器学习方法依赖于手工提取上百种特征特征，工作量较大。

​	本文提出用CNN算法来识别体细胞突变，由于CNN能够自动提取特征，从而可能会在原有算法效能的基础上有更大提升。

## 本文工作

- 数据来源

Platinum sample mixture datasets

ICGC-TCGA DREAM challenge datasets

Platinum tumor spike dataset

PacBio dataset（三代测序数据）

whole-exome and targeted panels（靶向测序数据）

different INDEL sizes

- 建模策略

![1571466678(1).png](https://raw.githubusercontent.com/JShuffle/picGo/master/1571466678(1).png)


​									图1 NeuSomatic workflow

**input**

​	NeuSomatic网络的输入是通过扫描序列比对情况后初步确定的候选体细胞突变，包括正常组织与肿瘤组织的体细胞突变。用其他现有算法识别的潜在突变位点也可以一并作为输入。

​	对于每个候选位点，构造一个3维特征矩阵M (k×5×32)，k个通道，每个通道大小为5×32。5用于编码A/T/C/G/-(gap)，32则是以潜在突变位点为中心展开的长为32的序列。

​	k个通道中：前 三 个分别是参考序列, 肿瘤序列和正常组织序列的频数（参考图1） , 总结 候选轨迹周围的参考基，以及 这个区域不同碱基的频率。参考序列通道增加了gap。其他剩余通道：主要包含覆盖度、碱基质量、比对质量、链偏移和剪切情况等信息。如果需要和其他现有算法ensemble的话，还可以把其他算法的结果作为一个独立的channel。

**layer**

​	受到ResNet的启发，选用了9个卷积层,共4个block。

**output**

​	两个softmax分类器和最后一层的一个回归器。第一个分类器用于识别该突变是否是体细胞的SNV/Indel。

第二个分类器用于预测突变发生长度（人为划分了4个等级：0表示非体细胞突变，1/2/>2表示表示突变长度）。

回归器用于预测体细胞突变的坐标。

## 结果

![1571466774(1).png](https://raw.githubusercontent.com/JShuffle/picGo/master/1571466774(1).png)

​	这里只展示了在Platinum sample mixture datasets上的算法效果。

该数据集按照不同比例混合肿瘤细胞和正常细胞（70:30；50:50；25:75；5:95）。NeuSomatic都明显优于所有其他方法。随着肿瘤纯度（25:75混合物）的降低，NeuSomatic的性能提升远远超过其他方法。NeuSomatic可以达到SNV和INDEL分别获得99.6％和97.2％的F1分数。与最佳方法相比，最低样本纯度的识别改进率高达7.2％。

## 结论

​	NeuSomatic是第一个基于深度学习的框架在使用相同的CNN架构时，它都实现了跨多个数据集最佳准确度。无论是合成还是真实数据，全基因组还是靶向，跨越的多种测序策略，多种测序技术，从短读取到高错误长读，它都实现了跨多个数据集最佳准确度。对于低肿瘤纯度和低等位基因频率，NeuSomatic明显优于其他算法。

## 参考

[1] Sayed Mohammad Ebrahim Sahraeian et al. “Deep convolutional neural networks for accurate somatic mutation detection”. In:Nature Communications 10.1 (Mar. 2019). DOI:10.1038/s41467-019-09027-x.

URL:[https://doi.org/10.1038%2Fs41467-019-09027-x](https://doi.org/10.1038%2Fs41467-019-09027-x)