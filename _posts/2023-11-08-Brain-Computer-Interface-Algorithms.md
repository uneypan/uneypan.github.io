---
layout: article
title: 脑机接口常用算法
mathjax: True
key: Brain-Computer-Interface-Algorithms
revised: 2024-01-19
---

<!--more-->

## 典型相关分析(Canonical Correlation Analysis, CCA)

CCA 主要用于在两组多元数据中寻找其内在隐含关系，被广泛应用与 SSVEP 检测。假设多导 EEG 脑电信号 $$\mathbf{X}\in\mathbb{R}^{L_1\times N}$$，SSVEP 模板信号为 $$\mathbf{Y}\in\mathbb{R}^{L_2\times N}$$，其中 $N$ 为样本个数，而 $L_1, L_2$ 分别为 $\mathbf{X}$ 和 $\mathbf{Y}$ 的特征维度。模板信号定义为刺激频率的正弦和余弦信号，及其高次谐波信号组成的矩阵，即

$$
\mathbf{Y}=\frac{1}{\sqrt{N}}\begin{bmatrix}\sin\left(\omega \cdot 0\right)&\cdots&\sin\left(\omega(N-1)\right)\\\cos\left(\omega \cdot 0\right)&\cdots&\cos\left(\omega(N-1)\right)\\\sin\left(2\omega \cdot 0\right)&\cdots&\sin\left(2\omega(N-1)\right)\\\cos\left(2\omega \cdot 0\right)&\cdots&\cos\left(2\omega(N-1)\right)\\\vdots&\vdots&\vdots\\\sin\left(\frac{L_2}{2}\omega \cdot 0\right)&\cdots&\sin\left(\frac{L_2}{2}\omega(N-1)\right)\\\cos\left(\frac{L_2}{2}\omega \cdot 0\right)&\cdots&\cos\left(\frac{L_2}{2}\omega(N-1)\right)\end{bmatrix}
$$

其中 $\omega$ 为刺激频率 $f$ 和采样频率 $f_s$ 下的刺激角频率，等于 $$ 2\pi f / f_s$$；$\frac{L_2}{2}$ 为刺激频率的最高谐波次数或模板倍频数。

**以 SVD 方法为例，CCA 算法的流程为**

- 计算$\mathbf{X}$和$\mathbf{Y}$的方差，以及它们的协方差：

  $$
  \boldsymbol{\Sigma}_{11} = \text{cov}(\mathbf{X}, \mathbf{X}) = \frac{1}{N} \mathbf{X}\mathbf{X}^\top\\
  \boldsymbol{\Sigma}_{22} = \text{cov}(\mathbf{Y}, \mathbf{Y}) = \frac{1}{N} \mathbf{Y}\mathbf{Y}^\top\\
  \boldsymbol{\Sigma}_{12} = \text{cov}(\mathbf{X}, \mathbf{Y}) = \frac{1}{N} \mathbf{X}\mathbf{Y}^\top
  $$

- 计算矩阵 

  $$ \mathbf{M} = \boldsymbol{\Sigma}_{11}^{-1/2}\boldsymbol{\Sigma}_{12}\boldsymbol{\Sigma}_{22}^{-1/2}$$

- 对矩阵 $\mathbf{M}$ 进行 SVD 分解，最大的奇异值 $\rho$ 和对应的奇异向量为 $\mathbf{u}$ 和 $\mathbf{v}$，即 

  $$\displaystyle \rho = \max_{\mathbf{u},\mathbf{v}}\mathbf{u}^\top\mathbf{M}\mathbf{v}$$

- 计算 $\mathbf{X}$ 和 $\mathbf{Y}$ 的线性系数向量 

  $$\mathbf{a} = \boldsymbol{\Sigma}_{11}^{-1/2}\mathbf{u}\\
  \mathbf{b} = \boldsymbol{\Sigma}_{22}^{-1/2}\mathbf{v}$$ 

 
可见算法流程并不复杂，但是要理解这个算法需要了解一些背景知识。

### CCA 算法推导 [^pku][^CCA]

[^CCA]: 典型关联分析(CCA)原理总结：[https://www.cnblogs.com/pinard/p/6288716.html](https://www.cnblogs.com/pinard/p/6288716.html)

[^pku]: [https://www.math.pku.edu.cn/teachers/lidf/course/mvr/mvrnotes/html/_mvrnotes/mvr-cancor.html#cancor-intro](https://www.math.pku.edu.cn/teachers/lidf/course/mvr/mvrnotes/html/_mvrnotes/mvr-cancor.html#cancor-intro)

在统计学中，**皮尔逊积矩相关系数**（英语：Pearson product-moment correlation coefficient，缩写：PPMCC 或 PCCs，有时简称相关系数）**用于度量两组数据的变量之间的线性相关的程度**。假设有两组一维的数据集 $\mathbf{x}$ 和 $\mathbf{y}$，则 PCCs 定义为

$$
\rho(\mathbf{x},\mathbf{y}) = \frac{\text{cov}(\mathbf{x},\mathbf{y})}{\sqrt{\operatorname{Var}(\mathbf{x})}\sqrt{\operatorname{Var}(\mathbf{y})}}
$$

但是，**PCCs不能直接用于高维数据**[^631380041]，高维数据难以计算矩阵相关性的原因有以下几点：

[^631380041]: [论文笔记] 高维数据矩阵相关性：[https://zhuanlan.zhihu.com/p/631380041](https://zhuanlan.zhihu.com/p/631380041)

- **维度灾难**：随着维度的增加，数据的样本量通常远小于变量数，样本点在高维空间中变得稀疏。假设有两个变量之间存在一些相关性，但是由于维度灾难，很可能只有少数几个样本点实际上能够展现这种关系。这会导致计算协方差时的样本点不足，产生不准确的估计。而且，高维空间中的任意两个向量之间的夹角较大，相关性的度量也变得不敏感，导致矩阵相关性的估计不稳定，容易受到异常值的影响，而且在低样本量时存在过高偏倚；
- **计算量**：高维数据的矩阵相关性的计算量很大，需要对两个矩阵进行交叉乘积，然后求解特征值和特征向量，这在维数很高时会非常耗时，目前稀疏矩阵的计算仍然是难题。

**CCA 的主要思想是将高维的两组数据分别降维到1维，然后用相关系数分析相关性**。但是有一个问题是，降维的标准是如何选择的呢？回想下主成分分析 PCA，降维的原则是投影方差最大；再回想下线性判别分析 LDA，降维的原则是同类的投影方差小，异类间的投影方差大。**CCA 选择的投影标准是降维后两组数据的 PCCs 最大**。

假设多导 EEG 脑电信号 $$\mathbf{X}\in\mathbb{R}^{L_1\times N}$$，SSVEP 模板信号为 $$\mathbf{Y}\in\mathbb{R}^{L_2\times N}$$，其中 $N$ 为样本个数，而 $L_1, L_2$ 分别为 $\mathbf{X}$ 和 $\mathbf{Y}$ 的特征维度。使用线性系数向量 $\mathbf{a}$ 和 $\mathbf{b}$ 将 $\mathbf{X}$ 和 $\mathbf{Y}$ 投影为一维向量

$$
\mathbf{x} = \mathbf{a}^\top \mathbf{X} \\ 
\mathbf{y} = \mathbf{b}^\top \mathbf{Y}
$$

CCA 的优化目标是最大化相关系数 $$\rho(\mathbf{x}, \mathbf{y})$$ 得到对应的投影向量 $\mathbf{a}$ 和 $\mathbf{b}$

$$
\arg\max_{\mathbf{a},\mathbf{b}}\rho(\mathbf{x}, \mathbf{y})
$$


在投影前，我们一般会把原始数据进行标准化，对于每个特征，计算其平均值和标准差。然后，对每个特征的每个数据点减去平均值后除以标准差，得到 $\mathbf{X}$ 和 $\mathbf{Y}$ 为每个特征的均值为 0 而方差为 1 的数据，即 $E(\mathbf{X}) = \mathbf{0}$， $D(\mathbf{X}) = \mathbf{I}$。

协方差矩阵为

$$
\Sigma=\left(\begin{array}{cc}
\operatorname{Var}(\mathbf{X}) & \operatorname{Cov}(\mathbf{X}, \mathbf{Y}) \\
\operatorname{Cov}(\mathbf{Y}, \mathbf{X}) & \operatorname{Var}(\mathbf{Y})
\end{array}\right)=\left(\begin{array}{cc}
\boldsymbol{\Sigma}_{11} & \boldsymbol{\Sigma}_{12} \\
\boldsymbol{\Sigma}_{21} & \boldsymbol{\Sigma}_{22}
\end{array}\right)
$$

有：

$$
\boldsymbol{\Sigma}_{11} = \text{cov}(\mathbf{X},\mathbf{X}) = E(\mathbf{X}\mathbf{X}^\top) - E(\mathbf{X})E(\mathbf{X})^\top = E(\mathbf{X}\mathbf{X}^\top)，\\
\boldsymbol{\Sigma}_{22} = E(\mathbf{Y}\mathbf{Y}^\top),\quad \boldsymbol{\Sigma}_{12} = E(\mathbf{X}\mathbf{Y}^\top),\quad
\boldsymbol{\Sigma}_{21} = \boldsymbol{\Sigma}_{12}^\top
$$

于是，可以得到 $\mathbf{x},\mathbf{y}$ 的方差和协方差为

$$
\text{Var}(\mathbf{x}) = \text{Var}(\mathbf{a}^\top \mathbf{X}) = \mathbf{a}^\top E(\mathbf{X} \mathbf{X}^\top) \mathbf{a}  = \mathbf{a}^\top \boldsymbol{\Sigma}_{11} \mathbf{a} \\
\text{Var}(\mathbf{y}) = \text{Var}(\mathbf{b}^\top \mathbf{Y}) = \mathbf{b}^\top E(\mathbf{Y} \mathbf{Y}^\top) \mathbf{b}  = \mathbf{b}^\top \boldsymbol{\Sigma}_{22} \mathbf{b}
$$

$$ 
\begin{aligned}
\text{cov} (\mathbf{x}, \mathbf{y}) &= \text{cov}(\mathbf{a}^\top \mathbf{X}, \mathbf{b}^\top \mathbf{Y})\\
&= E[<\mathbf{a}^\top \mathbf{X}, \mathbf{b}^\top \mathbf{Y}>] 
= E[ (\mathbf{a}^\top\mathbf{X}) (\mathbf{b}^\top \mathbf{Y})^\top] \\
&= \mathbf{a}^\top E(\mathbf{X} \mathbf{Y}^\top) \mathbf{b} 
= \mathbf{a}^\top \boldsymbol{\Sigma}_{12} \mathbf{b}
\end{aligned}
$$

则优化目标可以转化为：

$$
\arg\max_{\mathbf{a},\mathbf{b}}\quad\frac{\mathbf{a}^\top \boldsymbol{\Sigma}_{12} \mathbf{b}}{\sqrt{(\mathbf{a}^\top \boldsymbol{\Sigma}_{11} \mathbf{a}) (\mathbf{b}^\top \boldsymbol{\Sigma}_{22} \mathbf{b})}}
$$

可以采用和SVM类似的优化方法，固定分母，优化分子，具体的转化为：

$$
\begin{aligned}
\arg\max_{\mathbf{a},\mathbf{b}} \quad &\mathbf{a}^\top \boldsymbol{\Sigma}_{12} \mathbf{b} \\
\text{s.t.}\quad & \mathbf{a}^\top \boldsymbol{\Sigma}_{11} \mathbf{a} = 1,\\ &\mathbf{b}^\top \boldsymbol{\Sigma}_{22} \mathbf{b} = 1
\end{aligned}
$$

也就是说，算法的目标最终转化为一个凸优化过程，只要我们求出了这个优化目标的最大值，就是我们前面提到的多维 $\mathbf{X}$ 和 $\mathbf{Y}$ 的相关性度量，而对应的 $\mathbf{a},\mathbf{b}$ 则为降维时的投影向量，或者说线性系数。


对于上面的优化目标，我们可以做一次矩阵标准化，就可以用SVD来求解了。首先，我们令 $$\mathbf{a}=\boldsymbol{\Sigma}_{11}^{-1/2}\mathbf{u}, \mathbf{b}=\boldsymbol{\Sigma}_{22}^{-1/2}\mathbf{v}$$，有

$$
\mathbf{a}^\top\boldsymbol{\Sigma}_{11}\mathbf{a} =1 \Rightarrow \mathbf{u}^\top\boldsymbol{\Sigma}_{11}^{-1/2}\boldsymbol{\Sigma}_{11}\boldsymbol{\Sigma}_{11}^{-1/2}\mathbf{u} = 1  \Rightarrow  \mathbf{u}^\top\mathbf{u}=1 \\ 
\mathbf{b}^\top\boldsymbol{\Sigma}_{22}\mathbf{b} =1 \Rightarrow \mathbf{v}^\top\boldsymbol{\Sigma}_{22}^{-1/2}\boldsymbol{\Sigma}_{22}\boldsymbol{\Sigma}_{22}^{-1/2}\mathbf{v} = 1  \Rightarrow  \mathbf{v}^\top\mathbf{v}=1 \\ 
\mathbf{a}^\top\boldsymbol{\Sigma}_{12}\mathbf{b} = \mathbf{u}^\top\boldsymbol{\Sigma}_{11}^{-1/2}\boldsymbol{\Sigma}_{12}\boldsymbol{\Sigma}_{22}^{-1/2}\mathbf{v}
$$


也就是说，我们的优化目标变成

$$
\arg\max_{\mathbf{u},\mathbf{v}} \mathbf{u}^\top\boldsymbol{\Sigma}_{11}^{-1/2}\boldsymbol{\Sigma}_{12}\boldsymbol{\Sigma}_{22}^{-1/2}\mathbf{v} \\
\text{s.t.} \quad \mathbf{u}^\top\mathbf{u}=1, \mathbf{v}^\top\mathbf{v}=1
$$

如果将 $\mathbf{u},\mathbf{v}$ 看成矩阵 $$\mathbf{M} =\boldsymbol{\Sigma}_{11}^{-1/2}\boldsymbol{\Sigma}_{12}\boldsymbol{\Sigma}_{22}^{-1/2} $$ 的某一个奇异值左右奇异向量，那么利用奇异值分解，我们可以得到 

$$
 \mathbf{M} = \mathbf{U}\mathbf{\Sigma}\mathbf{V}^\top
$$

其中 $\mathbf{U},\mathbf{V}$ 为正交矩阵，$\mathbf{\Sigma}$ 为对角矩阵，对角线上的元素为奇异值。由于 $\mathbf{U},\mathbf{V}$ 的每一列都是标准正交基，因此 $ \mathbf{u}^\top\mathbf{U} $ 和 $ \mathbf{v}^\top\mathbf{V} $ 会得到一个只有一个标量值为1，其余标量值为0的向量。此时我们有

$$
\mathbf{u}^\top\mathbf{M}\mathbf{v} = \mathbf{u}^\top\mathbf{U}\mathbf{\Sigma}\mathbf{V}^\top\mathbf{v} = \sigma_{uv}
$$

也就是说，将矩阵 $\mathbf{M}$ 做了奇异值分解后，最大的奇异值就是我们优化目标的最大值，或者说我们的 $\mathbf{X}$ 和 $\mathbf{Y}$ 之间的最大相关系数 $\rho$。利用对应的左右奇异向量 $\mathbf{u},\mathbf{v}$ 可以求出原始 $\mathbf{X},\mathbf{Y}$ 线性系数 $$\mathbf{a}=\boldsymbol{\Sigma}_{11}^{-1/2}\mathbf{u}, \mathbf{b}=\boldsymbol{\Sigma}_{22}^{-1/2}\mathbf{v}$$。 

## 共空间模式（Common Spatial Pattern, CSP）

在脑电信号处理的背景下，考虑两类脑电信号$\mathbf{X}^{(i)} \in \mathbb{R}^{C \times M_i}(i=1,2)$，其中$C$和$M_i$分别表示通道数和采样点数。为了简化问题，假设每个通道的信号均值为零。然后，可以计算两类信号的空间协方差矩阵

$$\mathbf{\hat{R}}^{(i)}=\frac{1}{M_i} \mathbf{X}^{(i)} \mathbf{X}^{(i)^\top}(i=1,2)$$

CSP的任务是找到一种线性变换 $\mathbf{w}$，使得两类之间的方差比最大化。
<!-- $$
\max _{\mathbf{w}} \frac{\mathbf{w}^\top\hat{R}^{(1)} \mathbf{w}}{\mathbf{w}^\top\hat{R}^{(2)} \mathbf{w}} \quad \text { s.t. } \quad\|\mathbf{w}\|=1
$$ -->

数学上，这可以表示为以下优化问题：

$$
\begin{aligned}
\max _{\mathbf{w}} &\quad  \mathbf{w}^\top \mathbf{\hat{R}}^{(1)} \mathbf{w}\\
\text { s.t. } &\quad \mathbf{w}^\top \mathbf{\hat{R}}^{(2)} \mathbf{w} = 1
\end{aligned}
$$


我们可以引入拉格朗日乘子，定义拉格朗日函数：

$$
L(\mathbf{w}, \lambda) = \mathbf{w}^\top \mathbf{\hat{R}}^{(1)} \mathbf{w} - \lambda(\mathbf{w}^\top \mathbf{\hat{R}}^{(2)} \mathbf{w} - 1)
$$

这里，$\lambda$ 是拉格朗日乘子。接下来，我们要最大化这个拉格朗日函数。对于最大化问题，可以通过对拉格朗日函数对 $\mathbf{w}$ 求偏导，然后令其等于零来找到最优解：

$$
\frac{\partial L}{\partial \mathbf{w}} = 2\mathbf{\hat{R}}^{(1)}\mathbf{w} - 2\lambda\mathbf{\hat{R}}^{(2)}\mathbf{w} = 0
$$

整理得到：

$$
\mathbf{\hat{R}}^{(1)}\mathbf{w} = \lambda \mathbf{\hat{R}}^{(2)} \mathbf{w}
$$

有的时候约束条件会写成 $\mathbf{w}^\top \left[\mathbf{\hat{R}}^{(1)}+\mathbf{\hat{R}}^{(2)}\right] \mathbf{w} = 1$，这两个约束条件是等价的，因为方程可以写成

$$
\mathbf{\hat{R}}^{(1)} \mathbf{w} = \lambda' \left[\mathbf{\hat{R}}^{(1)}+\mathbf{\hat{R}}^{(2)}\right] \mathbf{w}, \quad \lambda' = \frac{\lambda}{1+\lambda}
$$

定义矩阵 $\mathbf{W}$ 为包含最优解（特征向量）的列矩阵 $\mathbf{w}$，$\boldsymbol{\Lambda}$为对应的对角矩阵，包含特征值 $\lambda$。上述方程可以写成矩阵形式[^GeneralizedEigenvalueProblem]：

$$
\mathbf{\hat{R}}^{(1)} \mathbf{W} =  \mathbf{\hat{R}}^{(2)}  \mathbf{W} \boldsymbol{\Lambda}
$$

这就是广义特征值问题的标准形式。因此，原始的优化问题可以通过求解这个广义特征值问题来得到最优解 $\mathbf{W}$ 和对应的 $\boldsymbol{\Lambda}$。特征向量 $\mathbf{w}_i$ 变化下两类的方差比等于其相应特征值的分量：

[^GeneralizedEigenvalueProblem]: [https://web.archive.org/web/20180427193359/http://fourier.eng.hmc.edu:80/e161/lectures/algebra/node7.html](https://web.archive.org/web/20180427193359/http://fourier.eng.hmc.edu:80/e161/lectures/algebra/node7.html)

$$
\lambda_i = \frac{\|\mathbf{w}_i^\top \mathbf{X}^{(1)}\|^2}{\|\mathbf{w}_i^\top \mathbf{X}^{(2)}\|^2}
$$


注意，由于不能保证 $\mathbf{\hat{R}}^{(i)}$ 非奇异，因此 $\mathbf{\hat{R}}^{(i)}$ 的逆矩阵可能不存在。因此，我们不能直接将上述方程写成 $\mathbf{\hat{R}}^{(2)-1} \mathbf{\hat{R}}^{(1)}\mathbf{W}= \mathbf{W} \boldsymbol{\Lambda}$，也就是说，不能对$\mathbf{\hat{R}}^{(2)-1} \mathbf{\hat{R}}^{(1)}$进行特征值分解。


等价地，协方差矩阵 $\mathbf{\hat{R}}^{(1)}$和 $\mathbf{\hat{R}}^{(2)}$可以被同一个$\mathbf{W}$对角化，具有相同的特征向量（这也是共空间模式名称的由来），证明见[^CSP]：

[^CSP]: CSP共空间模式一些公式的证明：[https://zhuanlan.zhihu.com/p/142635961](https://zhuanlan.zhihu.com/p/142635961)

$$
\mathbf{W}^\top \mathbf{\hat{R}}^{(i)} \mathbf{W}=\boldsymbol{\Lambda}^{(i)}(i=1,2)
$$


在得到空间滤波器矩阵 $\mathbf{W}$ 后（$\mathbf{W}$的每一列都是一个空间滤波器），选择前后各 $m$ 个空间滤波器构建特征向量 $\tilde{\mathbf{x}}$ 如下[^swolf]：

[^swolf]: 共空间模式CSP: [https://mrswolf.github.io/common-spatial-pattern/](https://mrswolf.github.io/common-spatial-pattern/)

$$
\begin{aligned}
\tilde{\mathbf{W}} & =\left[\mathbf{w}_1, \cdots, \mathbf{w}_m, \quad \mathbf{w}_{N_c-m+1}, \cdots, \mathbf{w}_{N_c}\right] \\
\tilde{\mathbf{x}} & =\log \left(\frac{\operatorname{diag}\left(\tilde{\mathbf{W}}^T \mathbf{X} \mathbf{X}^T \tilde{\mathbf{W}}\right)}{\operatorname{tr}\left(\tilde{\mathbf{W}}^T \mathbf{X} \mathbf{X}^T \tilde{\mathbf{W}}\right)}\right)
\end{aligned}
$$

其中 $\mathbf{w}_m$ 表示 $\mathbf{W}$ 的第 $m$ 列， $\tilde{\mathbf{W}}$ 是最终选定的空间滤波器组， $\operatorname{diag}(\cdot)$ 是矩阵主对角线上的元素， $\log (\cdot)$ 对每个元素做对数变换，其主要目的是使数据近似正态分布。获得特征向量 $\tilde{\mathbf{x}}$ 后，则可以使用线性判别分析、支持向量机等常见的机器学习模型构建分类器。
