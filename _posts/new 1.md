---
layout: post
title: '什么毛斌'
date: 2019-06-26
author: JShuffle
categories: 多元统计
tags: 多元统计分析
---

## Multi Dimensional Scaling介绍

- MDS动机

在经典的MDS求解问题中，往往原始样本的特征维度的个数是未知甚至难以观测的，有时候只能知道这些样本间的相似距离。我们希望根据样本在高维空间中的相似距离在低维坐标轴中排序出来，同时这种样本间排布关系尽可能和高维空间保持一致。

- MDS基本思想

经过降维后，在低维空间中（通常是2维）样本点之间的距离与原始维度空间中样本点之间的距离接近。（这里的距离是可以是任意的metrics，可以是欧式距离，也可以是基于保序回归的估计值）

- 微生物组学中的应用

如果把测序建库看作是抽样的过程，那一个观测样本的完整特征数就难以得知。但是我们可以依据有限的观测特征计算样本间的相似性，并在二维坐标轴上排序。

- MDS优化目标

$$
\mathbf { D } ^ { Y } = \operatorname { argmin } _ { \operatorname { rank } \left( \mathbf { D } ^ { Y } \leq k \right) } \left\| \mathbf { D } ^ { X } - \mathbf { D } ^ { Y } \right\| _ { F } ^ { 2 }
\\
{ D } ^ { X }维原始空间中的距离矩阵，{ D } ^ { Y }为k维空间中的距离矩阵，k通常取2
$$

## MDS类别

MDS根据metrics的不同，可以分成不同的类别。

### **1.classic MDS(cMDS)**

样本间相似性距离采用欧式距离，此时可以求出解析解。优化的损失函数叫Strain。

假设我们有一个欧式距离作为metrics的相似矩阵，该相似矩阵描述了高维空间中的样本相似性：


$$
D = \left( d _ { i j } \right)
$$


D的每个元素可以表示成：


$$
\left( \mathbf { D } _ { i j }  \right) ^ { 2 } = \left( \mathbf { x } _ { i } - \mathbf { x } _ { j } \right) ^ { T } \left( \mathbf { x } _ { i } - \mathbf { x } _ { j } \right) = \left\| \mathbf { x } _ { i } \right\| ^ { 2 } - 2 \mathbf { x } _ { i } ^ { T } \mathbf { x } _ { j } + \left\| \mathbf { x } _ { j } \right\| ^ { 2 }
$$


如果以矩阵的形式表示D，D的每一项都可以看做由3个二次项构成，因此，以矩阵的形式表示D：


$$
\mathbf { D }= \mathbf { Z } - 2 \mathbf { X } ^ { T } \mathbf { X } + \mathbf { Z } ^ { T }
$$


其中Z可以写成：


$$
\mathbf { Z } = z e ^ { T } = \left[ \begin{array} { c } { \left\| x _ { 1 } \right\| ^ { 2 } } \\ { \left\| x _ { 2 } \right\| ^ { 2 } } \\ { \vdots } \\ { \left\| x _ { n } \right\| ^ { 2 } } \end{array} \right] [ 1,1 , \cdots , 1 ] = \left[ \begin{array} { c c c c } { \left\| x _ { 1 } \right\| ^ { 2 } } & { \left\| x _ { 1 } \right\| ^ { 2 } } & { \dots } & { \left\| x _ { 1 } \right\| ^ { 2 } } \\ { \left\| x _ { 2 } \right\| ^ { 2 } } & { \left\| x _ { 2 } \right\| ^ { 2 } } & { \dots } & { \left\| x _ { 2 } \right\| ^ { 2 } } \\ { \vdots } & { } & { \vdots } & {  \vdots } \\ { \left\| x _ { n } \right\| ^ { 2 } } & { \dots } & { \dots }  &{ \left\| x _ { n } \right\| ^ { 2 } } \end{array} \right]
$$



D矩阵的行、列均值均不为0，这样不具备很好的性质，于是通过左乘，再右乘矩阵H，得到"中心化"的矩阵A：


$$
\mathbf { A }  = \mathbf { H D } \mathbf { H }
\\
其中：\mathbf { H } = \mathbf { I } _ { N } - \frac { 1 } { N } \mathbf { e } \mathbf { e } ^ { T }\\
$$

e 是全一向量。矩阵H的作用就是令A的行（列）去均值化。因此A的行均值，列均值均为0。

把D展开，A可以表示成：


$$
\mathbf { A }  = \mathbf { H} （\mathbf { Z } - 2 \mathbf { X } ^ { T } \mathbf { X } + \mathbf { Z } ^ { T }）\mathbf { H } 
$$


但是矩阵A的性质仍然不够好（中间项系数为负，对后面的化简不友好）。因此乘以-1/2得到矩阵B:

$$
\mathbf { B } = - \frac { 1 } { 2 } \mathbf { A } = - \frac { 1 } { 2 } \mathbf { H D }  \mathbf { H }
$$


B矩阵是对阵矩阵，同时也是半正定矩阵，这就具备了足够好的性质——能够进行特征分解。

下面简单证明一下为什么B是对阵矩阵。

因为：


$$
HZH  = H \left( z e ^ { T } \right) H = H z \left( e ^ { T} \left( I - \frac { e e ^ { T} } { n } \right) \right) 
\\
\left( e ^ { T} \left( I - \frac { e e ^ { T} } { n } \right) \right) =0
\\
所以：HZH=0
$$


所以：


$$
\mathbf { B }  = - \frac { 1 } { 2 } \mathbf { H } \mathbf { D }\mathbf { H } = - \frac { 1 } { 2 } \mathbf { H } \left( \mathbf { Z } - 2 \mathbf { X } ^ { T } \mathbf { X } + \mathbf { Z } ^ { T } \right) \mathbf { H }\\ = \mathbf { H } \mathbf { X } ^ { T } \mathbf { X } \mathbf { H } = ( \mathbf { X } \mathbf { H } ) ^ { T } ( \mathbf { X } \mathbf { H } )
\\
$$


根据：


$$
B = ( \mathbf { X } \mathbf { H } ) ^ { T } ( \mathbf { X } \mathbf { H } )
$$


我们可以得知B是对称矩阵，同时也是**内积矩阵**。

回到我们的优化目标，MDS的优化目标是在低维空间中（通常是2维）样本点之间的距离与原始维度空间中样本点之间的距离接近，数学上表示如下：


$$
\mathbf { B } ^ { Y } = \operatorname { argmin } _ { \operatorname { rank } \left( \mathbf { B } ^ { Y } \leq k \right) } \left\| \mathbf { B } ^ { X } - \mathbf { B } ^ { Y } \right\| _ { F } ^ { 2 }
$$


这个问题可以用特征分解公式求解。

因此，对B进行特征值分解：


$$
B = V \Lambda V ^ { \prime }=V \Lambda^{1/2} (\Lambda^{1/2} V ^ { \prime }) = Y^{T}Y
$$


如果只取最大的两个特征值和特征向量（即V为nx2维，特征值矩阵为2x2），那么得到的Y^{T}就是nX2维，每一行表示一个二维坐标点。这一步本质上就是PCA的过程。

最终就得到了MDS降维的结果Y^{T}。



### **2.Metric MDS（mMDS）**

在classic MDS的基础上，损失函数变了；距离可能加上了权重等等。优化的损失函数叫Stress。此外，在Metric MDS中，用户可以根据需求对距离进行指数放缩。


$$
d _ { i j } ^ { p }  \quad or  - d _ { i j } ^ { 2 p }
$$


广义上，可以把损失函数写成:


$$
stress= \mathcal { L } \left( \hat { d } _ { i j } \right) = \left( \sum _ { i < j } \left( \hat { d } _ { i j } - f \left( d _ { i j } \right) \right) ^ { 2 } / \sum d _ { i j } ^ { 2 } \right) ^ { \frac { 1 } { 2 } }
$$


其中，f(d_{ij})可以自己定义。



### **3.Non-metric MDS（NMDS）**

之所以叫Non-metric是因为此时相似距离不再是定量的，而是定性的。比如可以是顺序关系。

Wiki百科介绍了一种基于单调性保序回归（isotonic regression）估计样本间距的NMDS方法。

> Non-metric scaling is defined by the use of isotonic regression to non-parametrically estimate a transformation of the dissimilarities.

样本之间的相似距离不再是观测值，是基于非参数单调回归估计而来。

> isotonic regression:也叫**monotonic regression**，保序回归或单调回归。拟合值在任何地方保持单调性，且必须尽可能接近观测值。当实际的业务观测值具有单调性时可以运用该回归，如：药物剂量与毒性的关系。
>
> isotonic regression可以用于拟合NMDS中sample之间的距离。

保序回归的优化目标：


$$
Given \quad x_1 \leq x_2\leq...\leq x_n
\\
\min  \sum _ { i = 1 } ^ { n } w _ { i } \left( g \left( x _ { i } \right) - f \left( x _ { i } \right) \right) ^ { 2 }
\\
s.t.g(x_1)\leq g(x_2)\leq...\leq g(x_n)
$$

其中f(xi)是观测值，g(xi)是回归估计值。

**回到主题，什么时候用NMDS?**

1.在有些应用中，并不关心或者难以计算具体样本间的间距大小，而是仅仅知道它们的排序关系。这个时候，就需要用NMDS了。

2.对于那些方差呈现梯度分布的数据，也适合用NMDS。

这是因为NMDS在低维坐标轴上的映射考虑了所有方差（比如：通过保序回归），而类似PCA等基于特征值大小的排序方法只能考虑前若干个特征值映射到低维空间上。





## 参考

一份不错的讲义：http://www.cs.umd.edu/~djacobs/CMSC828/MDSexplain.pdf

一篇博客：<https://blog.csdn.net/baimafujinji/article/details/79407478>

MDS词条：<https://en.wikipedia.org/wiki/Multidimensional_scaling>

保序回归词条：<https://en.wikipedia.org/wiki/Isotonic_regression>





