---
layout: article
title: CSP的生成式模型
mathjax: True
key: Generative-View-of-CSP
---


## CSP的生成式模型

考虑两类脑电信号$\mathbf{X}^{(i)} \in \mathbb{R}^{C \times M_i}(i=1,2)$，其中$C$和$M_i$分别表示通道数和采样点数。为了简化问题，假设每个通道的信号均值为零。CSP的原始形式最终得到特征近似于正态分布[^csp]，CSP的生成式模型[^Wu]将解决方案视为从概率模型中的极大似然估计。概率模型是两个因子分析模型[^factor]的混合，每个模型都对应一类模式

[^csp]: [https://uneypan.github.io/2023/11/08/Brain-Computer-Interface-Algorithms.html#共空间模式common-spatial-pattern-csp](https://uneypan.github.io/2023/11/08/Brain-Computer-Interface-Algorithms.html#%E5%85%B1%E7%A9%BA%E9%97%B4%E6%A8%A1%E5%BC%8Fcommon-spatial-pattern-csp)

[^factor]: [https://www.math.pku.edu.cn/teachers/lidf/course/mvr/mvrnotes/html/_mvrnotes/mvr-factor.html](https://www.math.pku.edu.cn/teachers/lidf/course/mvr/mvrnotes/html/_mvrnotes/mvr-factor.html)

[^Wu]: Wu W, Chen Z, Gao S, et al. A probabilistic framework for learning robust common spatial patterns[C].2009 Annual International Conference of the IEEE Engineering in Medicine and Biology Society. IEEE, 2009: 4658-4661.

$$
\begin{aligned}
&\mathbf{X}^{(i)} = \mathbf{A} \mathbf{Z}^{(i)} + \mathbf{E}^{(i)} \quad (i=1,2) \\
\text{where} \quad &\mathbf{Z}^{(i)} \sim \mathcal{N}(\mathbf{0}, \mathbf{\Lambda}^{(i)} ), \quad \mathbf{E}^{(i)} \sim \mathcal{N}(\mathbf{0}, \mathbf{\Psi}^{(i)} )
\end{aligned}
$$

$\mathbf{A} \in \mathbb{R}^{C \times S}$是因子载荷矩阵（也称为混合矩阵，$S$ 是因子的数量），$\mathbf{Z}^{(i)} \in \mathbb{R}^{S \times M_i}$是因子，$\mathbf{E}^{(i)} \in \mathbb{R}^{C \times M_i}$是噪声信号，由于假定因子不相关，协方差矩阵$\mathbf{\Lambda}^{(i)},\mathbf{\Psi}^{(i)} \in \mathbb{R}^{C \times C}$都是对角阵。

CSP 算法中的转换矩阵 $\mathbf{W}$ 与混合矩阵 $\mathbf{A}$ 的极大似然估计 $\hat{\mathbf{A}}$ 之间的关系为  $\mathbf{W} = \hat{\mathbf{A}}^{-\top}$。该定理附带两个额外的假设：

- （i）附加噪声趋近于零；
- （ii）混合矩阵 $\mathbf{A}$ 是一个方阵。

## $\mathbf{A}$ 的极大似然估计

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


在实际应用中，上述两个假设很难满足，这正是使得CSP容易过拟合的原因。另一个需要解决的问题是CSP对观测数据中的异常值敏感，这是由于其基本的高斯假设导致的。



## Student's t-分布表示下的鲁棒CSP

已知Student's t-分布[^student-t]能够表示为高斯分布的无限混合，这些分布具有相同的均值，但方差由一个缩放因子 $\nu$ 控制。因此，可以将CSP的生成式模型重新表达为

[^student-t]: [https://uneypan.github.io/2023/06/20/Distribution.html#13-学生-t分布连续---python-code](https://uneypan.github.io/2023/06/20/Distribution.html#13-学生-t分布连续---python-code)

$$
\begin{aligned}
&\mathbf{X}^{(i)} = \mathbf{A} \mathbf{Z}^{(i)} + \mathbf{E}^{(i)} \quad (i=1,2) \\
\text{where} \quad &\mathbf{Z}^{(i)} \sim \mathcal{N}(\mathbf{0}, u^{(i)-1} \mathbf{\Lambda}^{(i)} ), \quad \mathbf{E}^{(i)} \sim \mathcal{N}(\mathbf{0}, u^{(i)-1}\mathbf{\Psi}^{(i)})\\
&u^{(i)} \sim \text{Gam}(\frac{\nu}{2}, \frac{\nu}{2} )
\end{aligned}
$$

这样一来，就将 $\mathbf{Z}^{(i)}$ 和 $\mathbf{E}^{(i)}$ 变为了Student's t-分布。当 $\nu \rightarrow \infty$ 时，Student's t-分布趋近于高斯分布。因此，当 $\nu$ 趋近于无穷大时，模型的表现将与原始的CSP模型相同。


## 贝叶斯生成式模型框架


在贝叶斯生成式模型框架下，所有的先验假设嵌入在 $p(Z)$ 中，如果 $p(Z)$ 部分或完全已知，则后验分布 $p(Z\mid X)$ 为

$$
p(Z \mid X)=\frac{p(X \mid Z) p(Z)}{p(X)}
$$

- 对于大多数 $p(Z)$，无法通过积分 $p(X) = \int p(X \mid Z) p(Z) d Z$ 写出 $p(X)$ 的解析解；
- 如果只是需要 $p(Z)$ 的点估计，那么计算 $p(X)$ 不是必须的。比如最大后验估计 $Z_{\text{MAP}} = \arg \max_z p (Z \mid X)$。

分层贝叶斯（hierarchical Bayesian）模型假设 $p(Z)$ 由超参数 $\gamma$ 控制，超先验分布 $p(\gamma)$ 描述超参数的概率分布

$$ p(Z) = \int p(Z \mid \gamma) p(\gamma) d \gamma $$

这种方法的一个优点是，它允许潜在变量的先验分布由数据本身决定。这种方法的一个优点是，它可以通过在数据上进行推断来自动确定模型的复杂度。

在矩阵 $A$ 未知的情况下，还需要指定一个先验分布。 $p(A)$ 这用于脑成像和脑状态分类的时空分解问题。在EEG/MEG时空分解中，数据被分解为不同成分的空间和时间模式的乘积之和，每个成分代表一个独特的神经生理过程。在这种情况下，$A$ 是空间模式矩阵，$Z$ 是时间模式矩阵。时空分解包括从信号中找到未知的 $A$ 和 $Z$，通常通过一些统计约束，例如分量的统计独立性、稀疏性或非负性。

## 贝叶斯推断

如果事先已知 $\gamma$ 和 $\Sigma_z$，则条件分布 $p(Z \mid X, \gamma) \propto p(X \mid Z) p(Z \mid \gamma)$ 是一个完全确定的高斯分布，其均值和协方差为

$$
\begin{aligned}
& \mathbb{E}_{p\left(z_t \mid X, \gamma\right)}\left[z_t\right]=\gamma \mathrm{A}^{\mathrm{T}}\left(\Sigma_e+\mathrm{A} \Sigma_z \mathrm{~A}^{\mathrm{T}}\right)^{-1} x_t \\
& \mathbb{C}_{p\left(z_t \mid X, \gamma\right)}\left[z_t\right]=\Sigma_z-\Sigma_z \mathrm{~A}^{\mathrm{T}}\left(\Sigma_e+\mathrm{A} \Sigma_z \mathrm{~A}^{\mathrm{T}}\right)^{-1} \mathrm{A} \Sigma_z,
\end{aligned}
$$

其中 $z_t$ 表示 $Z$ 的第 $t$ 列，并且与时间无关。常用的潜在源估计器是使用后验均值 $\hat{Z}=\mathbb{E}_{p(Z \mid X, \gamma)}[Z]$。然而，由于 $\gamma$ 是未知的，首先需要找到一个近似解 $\hat{\gamma}$。一种合理的方法是积分掉源 $Z$，然后解决以下问题

$$
\hat{\gamma}=\arg \max_\gamma \int p(X \mid Z) p(Z \mid \gamma) p(\gamma) \mathrm{d} Z
$$

这种处理有时被称为**经验贝叶斯 (EB)**，因为先验 $p(Z\mid \gamma)$ 是根据经验从数据中学习的，通常使用 EM 算法。此外，边缘化提供了一种自然的正则化，可以将 $\gamma$ 的许多元素缩小到零，实际上从模型中修剪掉不相关的协方差分量。


第二种选择是积分掉未知的 $\gamma$ 将 $p(Z)$ 视为有效先验，并计算 MAP 估计 $Z_{\text{MAP}}$ ：

$$
\begin{aligned}
Z_{\text{MAP}} &= \arg \max_Z \int p(Z\mid X) p(Z\mid \gamma) p(\gamma) d\gamma \\
&= \arg \max_Z  p(X\mid Z) p(Z)
\end{aligned}
$$

这种方法被称为 **惩罚似然（PL）** 方法。

第三种选择是使用 **变分贝叶斯（VB）** 方法，它提供了对 EB 和惩罚似然方法的替代方案。其中两种最流行的变分逼近形式是均场逼近（VB-MF）和拉普拉斯逼近（VB-LA）。
- 均场逼近假设未知量 $Z$ 和 $\gamma$ 上的联合分布是因子分布的：$p(Z, \gamma \mid X) \approx q(Z \mid X) q(\gamma \mid X)$，其中 $q(Z \mid X)$ 和 $q(\gamma \mid X)$ 被选择为最小化因子化后验和完整后验之间的KL散度。这通过类似于EM的迭代过程来实现，使用2个E步（一个用于 $Z$，一个用于 $\gamma$）。它还产生了与 EB 方法类似的 $\log p(X)$ 的严格下界。
- 二阶拉普拉斯逼近假设超参数的后验（在边际化 $Z$ 后）是高斯分布，然后迭代地将其与真实后验匹配；结果可以用于近似 $p(Z \mid X)$ 和 $\log p(X)$。

可以证明，当先验选择为非因子、前导场和噪声相关时，EB 方法与惩罚似然方法等效；VB方法 等价于给定适当超先验的 EB 方法。

抽样方法与 VB 方法（近似贝叶斯推断的确定性版本）相反，抽样方法可以被视为近似贝叶斯推断的随机版本。基本思想很简单：从分布中抽取随机样本并使用插件样本进行计算，而不是在后验计算中计算棘手的积分。尽管如此，从精确的目标分布 P 中提取蒙特卡罗样本仍然很重要。







### EEG/MEG信号过程

EEG/MEG信号过程可在一个潜在变量生成式模型框架下进行建模：

$$
X = f(Z,\theta) + E
$$

$f$ 是 $Z$ 的函数，参数 $\theta$ 可以是已知的，也可以未知。当 $f$ 近似为线性时，可以使用因子分析模型来建模 $f$：

$$
X = AZ + E \text{ or } x_t = A z_t + e_t  
$$

$X = [x_1,...,x_T] \in \mathbb{R}^{N\times T}$ 是 $N$ 传感器观测数据的 $T$ 个采样点，$Z = [z_1, ..., z_T] \in \mathbb{R}^{M\times T}$ 与 $M$ 个潜在脑源相关，$A \in \mathbb{R}^{N\times M}$ 是观测矩阵，$E$ 是与噪声误差项。

分析目标是推断（infer）潜在变量和未知参数以进行数据解释。
- 当数据似然性符合高斯分布时，目标变为回归问题。
- 当数据似然性包含类别标签 $c$ 时，例如在脑状态分类问题中 $p(X \mid Z, \boldsymbol{\theta}, c)$，推断问题表达为

$$ p(c \mid X)= \int p(c, Z \mid X) d Z=\int p(c \mid X, Z) p(Z \mid X) d Z $$

- 推断问题分解为两步：估计 $p(Z \mid X)$ 和估计 $p(c \mid Z)$


一种对于静态模型的拓展是动态因子分析模型（也称为马尔可夫状态空间模型），它将潜在变量建模为随时间变化的过程：

$$
\begin{aligned}
&z_{t} = F z_{t-1} + \nu_t \\
&x_t = A z_t + e_t
\end{aligned}
$$

$F$ 是时不变的状态转移矩阵，$\nu_t \sim \mathcal{N}(\mathbf{0,I})$ 是状态转移噪声，$e_t\sim \mathcal{N}(\mathbf{0},\Sigma_e)$ 是观测噪声。假设 $\Sigma_z$ 是 $z_t$ 的协方差矩阵，那么 $x_t$ 的协方差矩阵为 $\Sigma_x = A \Sigma_z A^\top + \Sigma_e$，均值为 $\mathbf{0}$。


