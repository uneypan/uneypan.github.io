---
layout: article
title: CSP的生成式模型
mathjax: True
key: Generative-View-of-CSP
# article_header:
#   type: overlay
#   theme: dark
#   background_color: '#203028'
#   background_image:
#     gradient: 'linear-gradient(0deg,rgba(0, 0, 0, .5),  rgba(255, 255, 255 , .0))'
#     # src:  /pictures/BCI/大脑皮层.png
---


## CSP的生成式模型[^Wu]

[^Wu]: Wu W, Chen Z, Gao S, et al. A probabilistic framework for learning robust common spatial patterns[C].2009 Annual International Conference of the IEEE Engineering in Medicine and Biology Society. IEEE, 2009: 4658-4661.

考虑两类脑电信号$\mathbf{X}^{(i)} \in \mathbb{R}^{C \times M_i}(i=1,2)$，其中$C$和$M_i$分别表示通道数和采样点数。为了简化问题，假设每个通道的信号均值为零。CSP的生成式模型将解决方案视为从概率模型中的极大似然估计。概率模型是两个因子分析模型[^factor]的混合，每个模型都对应一类模式

[^factor]: [https://www.math.pku.edu.cn/teachers/lidf/course/mvr/mvrnotes/html/_mvrnotes/mvr-factor.html](https://www.math.pku.edu.cn/teachers/lidf/course/mvr/mvrnotes/html/_mvrnotes/mvr-factor.html)

$$
\begin{aligned}
&\mathbf{X}^{(i)} = \mathbf{A} \mathbf{Z}^{(i)} + \mathbf{E}^{(i)} \quad (i=1,2) \\
\text{where} \quad &\mathbf{Z}^{(i)} \sim \mathcal{N}(\mathbf{0}, \mathbf{\Lambda}^{(i)} ), \quad \mathbf{E}^{(i)} \sim \mathcal{N}(\mathbf{0}, \mathbf{\Psi}^{(i)} )
\end{aligned}
$$

$\mathbf{A} \in \mathbb{R}^{C \times S}$是因子载荷矩阵（也称为混合矩阵，$S$ 是因子的数量），$\mathbf{Z}^{(i)} \in \mathbb{R}^{S \times M_i}$是因子，$\mathbf{E}^{(i)} \in \mathbb{R}^{C \times M_i}$是噪声信号，由于假定因子不相关，协方差矩阵$\mathbf{\Lambda}^{(i)},\mathbf{\Psi}^{(i)} \in \mathbb{R}^{C \times C}$都是对角阵。

CSP 算法中的转换矩阵 $\mathbf{W}$ 与混合矩阵 $\mathbf{A}$ 的最大似然估计 $\hat{\mathbf{A}}$ 之间的关系为  $\mathbf{W} = \hat{\mathbf{A}}^{-\top}$。

该定理附带两个额外的假设：

- （i）附加噪声趋近于零；
- （ii）混合矩阵 $\mathbf{A}$ 是一个方阵。

在实际应用中，上述两个假设很难满足，这正是使得CSP容易过拟合的原因。另一个需要解决的问题是CSP对观测数据中的异常值敏感，这是由于其基本的高斯假设导致的。

## 

已知学生 t 分布能够表示为高斯分布的无限混合，这些分布具有相同的均值，但方差由一个缩放因子 $\nu$ 控制。因此，可以将CSP的生成式模型重新表达为

$$
\begin{aligned}
&\mathbf{X}^{(i)} = \mathbf{A} \mathbf{Z}^{(i)} + \mathbf{E}^{(i)} \quad (i=1,2) \\
\text{where} \quad &\mathbf{Z}^{(i)} \sim \mathcal{N}(\mathbf{0}, u^{(i)-1} \mathbf{\Lambda}^{(i)} ), \quad \mathbf{E}^{(i)} \sim \mathcal{N}(\mathbf{0}, \mathbf{\Psi}^{(i)} )
\end{aligned}
$$


## $\mathbf{W} = \hat{\mathbf{A}}^{-\top}$ 的证明

1. 使用KL散度的定义将对数似然函数重新表达

    KL散度是两个概率分布之间的差异度量。通过引入KL散度，将对数似然函数表示为两个分布的KL散度的累加。  

    $$
    \begin{aligned}
    L & =\sum_{i=1}^2 \sum_{k=1}^{M_i} \ln p\left(\mathbf{X}_k^{(i)} \mid \mathbf{A}, \boldsymbol{\Lambda}^{(1)}, \boldsymbol{\Lambda}^{(2)}\right) \\
    & =-\sum_{i=1}^2 \frac{M_i}{2}\left[C \ln (2 \pi)+\ln \left|\mathbf{R}^{(i)}\right|+\operatorname{Tr}\left(\left(\mathbf{R}^{(i)}\right)^{-1} \hat{\mathbf{R}}^{(i)}\right)\right] \\
    & =-\sum_{i=1}^2 \frac{M_i}{2}\left[\operatorname{Tr}\left(\mathbf{R}^{(i)}\right)^{-1} \hat{\mathbf{R}}^{(i)}-\ln \left|\left(\mathbf{R}^{(i)}\right)^{-1} \hat{\mathbf{R}}^{(i)}\right|-C\right]+ \text { Const } \\
    & =-\sum_{i=1}^2{\operatorname{KL}}\left(\hat{\mathbf{R}}^{(i)} \| \mathbf{R}^{(i)}\right)+\text { Const }\\
    \text{where} & \quad \mathbf{R}^{(i)} =\mathbf{A} \boldsymbol{\Lambda}^{(i)} \mathbf{A}^{\top}
    \end{aligned}
    $$
    
2. 对数似然的重写

    因为 KL 散度对于可逆线性变换是不变的，根据高斯分布的Pythagorean分解，对数似然被重新写为关于混合矩阵逆的表达式  

    $$
    \begin{aligned}
    L= & -\sum_{i=1}^2 \operatorname{KL}\left(\mathbf{A}^{-1} \hat{\mathbf{R}}^{(i)} \mathbf{A}^{-\top} \| \boldsymbol{\Lambda}^{(i)}\right)+\text { Const } \\
    = & -\sum_{i=1}^2\left[\operatorname{KL}\left(\mathbf{A}^{-1} \hat{\mathbf{R}}^{(i)} \mathbf{A}^{-\top} \| \operatorname{diag}\left(\mathbf{A}^{-1} \hat{\mathbf{R}}^{(i)} \mathbf{A}^{-\top}\right)\right)\right. \\
    & \left.+\operatorname{KL}\left(\operatorname{diag}\left\{\mathbf{A}^{-1} \hat{\mathbf{R}}^{(i)} \mathbf{A}^{-\top}\right\} \| \boldsymbol{\Lambda}^{(i)}\right)\right]+ \text { Const }
    \end{aligned}
    $$

3. 对数似然的最大化

    第一个KL散度项的归零条件是 $\mathbf{A}^{-1} \hat{\mathbf{R}}^{(i)} \mathbf{A}^{-\top}$ 是一个对角矩阵。换句话说，为了最大化对数似然，需要确保通过混合矩阵的逆的转置对 $\hat{\mathbf{R}}^{(i)}$ 进行线性变换后得到的矩阵$\mathbf{A}^{-1} \hat{\mathbf{R}}^{(i)} \mathbf{A}^{-\top}$是对角的。由于 $\boldsymbol{\Lambda}^{(1)}$ 和 $\boldsymbol{\Lambda}^{(2)}$ 是对角矩阵，因此，不论混合矩阵 $\mathbf{A}$ 取什么值，第二个KL散度项恒等于零。换句话说，只有当 $\mathbf{A}^{-\top}$ 联合对角化 $\hat{\mathbf{R}}^{(1)}$ 和 $\hat{\mathbf{R}}^{(2)}$ 时，对数似然函数才能达到最大化。
