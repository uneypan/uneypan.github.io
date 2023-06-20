---
layout: article
title: 机器学习中的常见概率分布
mathjax: true
mermaid: false
---

## 各种概率分布之间的关系

![](https://github.com/uneypan/distribution-is-all-you-need/raw/master/overview.png)

- `conjugate`（共轭）表示两者之间为 **conjugate distributions**（共轭分布）关系.

  > 在 [贝叶斯概率论](https://en.wikipedia.org/wiki/Bayesian_probability) 中，如果 [后验分布](https://en.wikipedia.org/wiki/Posterior_probability) $p(\theta\mid x)$ 和[先验分布](https://en.wikipedia.org/wiki/Prior_probability_distribution) $p(\theta)$在同一 [概率分布族](https://en.wikipedia.org/wiki/List_of_probability_distributions) 中，那么先验和后验分布称为**共轭分布**，先验分布被称为 [似然函数](https://en.wikipedia.org/wiki/Likelihood_function) 的 [共轭先验](https://en.wikipedia.org/wiki/Conjugate_prior)。
  
- `Multi-Class` 表示随机变量的数量大于2。

- `N Times` 表示我们还考虑先验概率P(X)。


## 概率分布及其特点

### $1$. **均匀分布（连续）**   [Python Code](https://github.com/uneypan/distribution-is-all-you-need/blob/master/uniform.py)
   - $\displaystyle\text{U}(x\mid a,b)=\frac{1}{b-a}$.
   - 均匀分布在[a,b]上具有相同的概率值，最简单的概率分布。
<p align="center"><img width="400" src="https://github.com/uneypan/distribution-is-all-you-need/raw/master/graph/uniform.png" /></p>

### $2$. **伯努利分布（离散）**   [Python Code](https://github.com/uneypan/distribution-is-all-you-need/blob/master/bernoulli.py)
   - $\text{Bern}(x\mid\mu)=\mu^x(1-\mu)^{1-x}$.
   - ⼆元变量$x \in \{0,1\}$ 的分布，例如，抛硬币的结果。
   - 参数 $\mu \in [0,1]$ 控制了 $x=1$ 的概率。
   - 伯努利分布是⼆项分布对于单⼀观测的特殊情况。它对于 $\mu$ 的共轭先验是Beta分布。
<p align="center"><img width="400" src="https://github.com/uneypan/distribution-is-all-you-need/raw/master/graph/bernoulli.png" /></p>

### $3$. **二项式分布（离散）**   [Python Code](https://github.com/uneypan/distribution-is-all-you-need/blob/master/binomial.py)
   - $\displaystyle\text{Bin}(m\mid N,\mu)= \binom{N}{m}\mu^m(1-\mu)^{N-m}$, 其中，$\displaystyle\binom{N}{m} = \frac{N!}{(N-m)!m!}$.
   - ⼆项分布给出了来⾃伯努利分布的 $N$ 个样本中观察到 $m$ 次 $x = 1$ 的概率。
   - ⼆项分布中 $N = 1$ 这⼀特殊情形被称为伯努利分布。
   - 对于⼤的 $N$ 值，⼆项分布近似于⾼斯分布。
   - $\mu$ 的共轭先验是Beta分布。
<p align="center"><img width="400" src="https://github.com/uneypan/distribution-is-all-you-need/raw/master/graph/binomial.png" /></p>

### $4$. **分类分布/多伯努利分布（离散）**   [Python Code](https://github.com/uneypan/distribution-is-all-you-need/blob/master/categorical.py)
   - $\mathrm{Cat}(x\mid\boldsymbol{\mu})=\prod_{k=1}^{C}\mu_c^{\mathbb{I}(x=k)},x\in\{1,...,K\}$ 0-1编码.
   - 多伯努利也称为分类分布，是一种维度大于2的伯努利分布。
<p align="center"><img width="400" src="https://github.com/uneypan/distribution-is-all-you-need/raw/master/graph/categorical.png" /></p>

### $5$. **多项式分布（离散）**   [Python Code](https://github.com/uneypan/distribution-is-all-you-need/blob/master/multinomial.py)
   - $\displaystyle\mathrm{Mult}(m_1,m_2,\dots,m_K \mid \boldsymbol{\mu},N)= \binom{N}{m_1m_2\dots m_K}\prod_{k=1}^{K} \mu_k^{m_k}$.
   - 其中，$\displaystyle\binom{N}{m_1m_2\dots m_K} = \frac{N!}{m_1!m_2!\dots m_K!}$，指 $N$ 个相同的物体中的 $m_k$ 个放到箱⼦ $k$ 中的⽅案总数。
   - 多项式分布是⼆项分布对于多元变量的推⼴， 给出了⼀个具有 $K$ 个状态的离散变量在总计 $N$ 次观测中处于状态 $k$ 的次数 $m_k$ 的分布。
   - 参数 $\{\mu_k\}$ 的共轭先验是狄利克雷分布。
<p align="center"><img width="400" src="https://github.com/uneypan/distribution-is-all-you-need/raw/master/graph/multinomial.png" /></p>

### $6$. **Beta分布（连续）**   [Python Code](https://github.com/uneypan/distribution-is-all-you-need/blob/master/beta.py)
   - $\displaystyle\mathrm{Beta}(\mu\mid a,b) = \frac{\Gamma(a+b)}{\Gamma(a)\Gamma(b)}\mu^{a-1}(1-\mu)^{b-1}$.
   - 其中 $a$ 和 $b$ 可以分别表⽰为$x=1$和$x=0$的观测的有效先验数量。
   - 对于$a = b = 1$的情形， 它就简化成了均匀分布。
   - Beta分布是伯努利分布的共轭先验。
<p align="center"><img width="400" src="https://github.com/uneypan/distribution-is-all-you-need/raw/master/graph/beta.png" /></p>

### $7$. **狄利克雷分布（连续）**   [Python Code](https://github.com/uneypan/distribution-is-all-you-need/blob/master/dirichlet.py)
   - $\displaystyle\mathrm{Dir}(\boldsymbol{\mu\mid \alpha}) = \frac{\Gamma(\alpha_0)}{\Gamma(\alpha_1)\cdots\Gamma(\alpha_K)} \prod_{k=1}^{K} \mu_k^{\alpha_k-1}$.
   - 狄利克雷分布是 $K$ 个随机变量的多变量分布，是Beta分布的推⼴。
   - 这种情况下，参数 $\alpha_k$ 是 $K$ 维⼆元观测向量 $x$ 对应值的有效观测数量。
   - $K=2$ 时变成Beta分布。
   - 狄利克雷分布是多项式分布的共轭先验。
<p align="center"><img width="400" src="https://github.com/uneypan/distribution-is-all-you-need/raw/master/graph/dirichlet.png" /></p>

### $8$. **Gamma分布（连续）**   [Python Code](https://github.com/uneypan/distribution-is-all-you-need/blob/master/gamma.py)
   - $\displaystyle\text{Gam}(\lambda\mid a,b)=\frac{1}{\Gamma(a)}b^a\lambda^{a-1}\exp\{-b\lambda\}$
   - $\displaystyle \frac{\text{Gam}(a,1)}{\text{Gam}(a,1) + \text{Gam}(b,1)}$ 转化为 $\text{Beta}(a,b)$.
   - 指数分布和卡方分布是Gamma分布的特例。
<p align="center"><img width="400" src="https://github.com/uneypan/distribution-is-all-you-need/raw/master/graph/gamma.png" /></p>

### $9$. **指数分布（连续）**   [Python Code](https://github.com/uneypan/distribution-is-all-you-need/blob/master/exponential.py)
   - $\displaystyle \text{Exp}(x\mid\lambda )=\lambda e^{-\lambda x},x\geq 0$
   - 指数分布是 Gamma分布当a为1时的特殊情况。
<p align="center"><img width="400" src="https://github.com/uneypan/distribution-is-all-you-need/raw/master/graph/exponential.png" /></p>

### $10$. **高斯分布（连续）**   [Python Code](https://github.com/uneypan/distribution-is-all-you-need/blob/master/gaussian.py)
   - 一元高斯分布  
   $$\displaystyle \mathcal{N}(x\mid\mu,\sigma^2)=\frac{1}{(2\pi\sigma)^{1/2}}\exp \left\{-\frac{1}{2\sigma^2}(x-\mu)^2\right\}$$
   - 多元高斯分布（$D$维）   
   $$\displaystyle\mathcal{N}(\boldsymbol{x} \mid \boldsymbol{\mu}, \boldsymbol{\Sigma})=\frac{1}{(2 \pi)^{\frac{D}{2}}} \frac{1}{\mid \boldsymbol{\Sigma}\mid ^{\frac{1}{2}}} \exp \left\{-\frac{1}{2}(\boldsymbol{x}-\boldsymbol{\mu})^{T} \boldsymbol{\Sigma}^{-1}(\boldsymbol{x}-\boldsymbol{\mu})\right\} $$
   - 一元⾼斯分布的均值$\mu$的共轭先验是⾼斯分布，精度$\sigma^{-1}$的共轭先验是Gamma分布.
   - 多元⾼斯分布的均值 $\boldsymbol{\mu}$ 的共轭先验仍然是⾼斯分布，精度 $\boldsymbol{\Lambda}$ 的共轭先验是⼀个Wishart分布，$(\boldsymbol{\mu},\boldsymbol{\Lambda})$ 的共轭先验是⾼斯-Wishart分布 $$p(\mu,\Lambda\mid \mu_0,\beta,W,\nu) = \mathcal{N}(\mu\mid \mu_0,(\beta\Lambda)^{-1})\mathcal{W}(\Lambda\mid W,\nu)$$
<p align="center"><img width="400" src="https://github.com/uneypan/distribution-is-all-you-need/raw/master/graph/gaussian.png" /></p>

### $11$. **正态分布（连续）**   [Python Code](https://github.com/uneypan/distribution-is-all-you-need/blob/master/normal.py)
   - $\displaystyle \mathcal{N}(x)=\frac{1}{(2\pi)^{1/2}}\exp \left(-\frac{x^2}{2}\right)$
   - 正态分布是标准化的高斯分布，平均值为0，标准差为1。
<p align="center"><img width="400" src="https://github.com/uneypan/distribution-is-all-you-need/raw/master/graph/normal.png" /></p>

### $12$. **卡方分布（连续）**   [Python Code](https://github.com/uneypan/distribution-is-all-you-need/blob/master/chi-squared.py)
   - $\displaystyle  \chi^2(x\mid k)=\frac{x^{\frac{k}{2}-1}}{2^{\frac{k}{2}}\Gamma({\frac{k}{2}})}  \exp\left(-\frac{x}{2}\right)$.
   - 具有k个自由度的卡方分布是k个独立标准正态随机变量的平方和的分布。
   - 卡方分布是Beta分布的特例。
<p align="center"><img width="400" src="https://github.com/uneypan/distribution-is-all-you-need/raw/master/graph/chi-squared.png" /></p>

### $13$. **学生-t分布（连续）**   [Python Code](https://github.com/uneypan/distribution-is-all-you-need/blob/master/student-t.py)
   - 在⼀元变量的形式下，学⽣t分布可以通过下列⽅式获得：拿出⼀元⾼斯分布的精度的共轭先验，然后把精度变量积分出来。因此这个分布可以看成⽆限多个有着相同 $\mu$，但 $\sigma^2$ 不同的⾼斯分布的混合。     
   $$\displaystyle \text{St}(x\mid\mu,\lambda,\nu) = \frac{\Gamma(\frac{\nu}{2}+\frac{1}{2})}{\Gamma(\frac{\nu}{2})}\left(\frac{\lambda}{\pi\nu}\right)^{\frac{1}{2}}\left[ 1+ \frac{\lambda(x-\mu)^2}{\nu}\right]^{-\frac{\nu}{2}-\frac{1}{2}}$$
   - 在D维变量的形式下，学⽣t分布是将多元⾼斯的精度矩阵关于共轭 Wishart 先验积分的结果，形式为   
   $$\text{St}(\boldsymbol{x}\mid\boldsymbol{\mu},\boldsymbol{\Lambda},\nu) = \frac{\Gamma(\frac{\nu}{2}+\frac{1}{2})}{\Gamma(\frac{\nu}{2})}\frac{\mid\boldsymbol{\Lambda}\mid^{\frac{1}{2}}}{(\pi\nu)^{\frac{D}{2}}}\left[ 1+ \frac{(\boldsymbol{x}-\boldsymbol{\mu})^T\boldsymbol{\Lambda}(\boldsymbol{x}-\boldsymbol{\mu})}{\nu}\right]^{-\frac{\nu}{2}-\frac{D}{2}}$$
   - 与高斯分布一样，t分布是对称的钟形分布，但通常有着⽐⾼斯分布更长的“尾巴”，这意味着它更容易产生远低于平均值的值。
   - 学⽣t分布提供了对⾼斯分布泛化的⼀种形式，这种分布的最⼤似然参数值对离群点⽐较鲁棒。
   - $\nu > 0$ 被称为分布的⾃由度数量。
   - 对于 $\nu = 1$ 的情况，t分布变为了柯西分布（Cauchy distribution）。
   - 在极限 $\nu \rightarrow\infty $的情况下，t分布 $\text{St}(x\mid\mu,\lambda,\nu)$ 变成了⾼斯分布  $\mathcal{N}(x\mid\mu,\lambda-1)$。
   

<p align="center"><img width="400" src="https://github.com/uneypan/distribution-is-all-you-need/raw/master/graph/student_t.png" /></p>

### $14$. **Wishart 分布（连续）**
   - $\mathcal{W}(\Lambda\mid W, \nu) = B(W, \nu)\mid \Lambda\mid ^{(\nu-D-1)/2}\exp\left(-\frac{1}{2}\text{Tr}(W^{-1}\Lambda)\right)$ 
   - 其中，归一化系数为 $B(W,\nu) = \mid W\mid ^{-\nu/2}\left(2^{\nu D/2}\pi^{D(D-1)/4}\prod\limits_{i=1}^D\Gamma\left(\frac{\nu+1-i}{2}\right)\right)^{-1}$

## Reference

1. [Tae Hwan Jung - Distribution is All You Need](https://github.com/graykode/distribution-is-all-you-need)  
2. [Wikipedia - Relationships among Probability Distributions](https://en.wikipedia.org/wiki/Relationships_among_probability_distributions)  
3. [Bishop - Pattern Recognition and Machine Learning, 2006](https://www.microsoft.com/en-us/research/uploads/prod/2006/01/Bishop-Pattern-Recognition-and-Machine-Learning-2006.pdf)