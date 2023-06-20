---
layout: article
title: 机器学习中的常见概率分布
mathjax: true
mermaid: false
---

## 概率分布总览

![](https://github.com/uneypan/distribution-is-all-you-need/raw/master/overview.png)

- `conjugate`（共轭）表示两者之间为 **conjugate distributions**（共轭分布）关系.

  > 在 [贝叶斯概率论](https://en.wikipedia.org/wiki/Bayesian_probability) 中，如果 [后验分布](https://en.wikipedia.org/wiki/Posterior_probability) $p(\theta\mid x)$ 和[先验分布](https://en.wikipedia.org/wiki/Prior_probability_distribution) $p(\theta)$在同一 [概率分布族](https://en.wikipedia.org/wiki/List_of_probability_distributions) 中，那么先验和后验分布称为**共轭分布**，先验分布被称为 [似然函数](https://en.wikipedia.org/wiki/Likelihood_function) 的 [共轭先验](https://en.wikipedia.org/wiki/Conjugate_prior)。
  
- `Multi-Class` 表示随机变量大于2。

- `N Times` 表示我们还考虑先验概率P(X)。


## 概率分布及其特点

1. **均匀分布（连续）**   [Python Code](https://github.com/uneypan/distribution-is-all-you-need/blob/master/uniform.py)
   - $\displaystyle\text{U}(x\mid a,b)=\frac{1}{b-a}$.
   - 均匀分布在[a,b]上具有相同的概率值，最简单的概率分布。
<p align="center"><img width="400" src="https://github.com/uneypan/distribution-is-all-you-need/raw/master/graph/uniform.png" /></p>

2. **伯努利分布（离散）**   [Python Code](https://github.com/uneypan/distribution-is-all-you-need/blob/master/bernoulli.py)
   - $\text{Bern}(x\mu)=\mu^x(1-\mu)^{1-x}$.
   - 伯努利分布不考虑先验概率 P(X)，因此，使用最大似然将容易受到过拟合的影响。
   - 我们使用**二进制交叉熵**对二进制分类进行分类。它的形式和取伯努利分布的负对数是一样的。
<p align="center"><img width="400" src="https://github.com/uneypan/distribution-is-all-you-need/raw/master/graph/bernoulli.png" /></p>

3. **二项式分布（离散）**   [Python Code](https://github.com/uneypan/distribution-is-all-you-need/blob/master/binomial.py)
   - $\displaystyle\text{Bin}(m\mid N,\mu)= \binom{N}{m}\mu^m(1-\mu)^{N-m}$.
   - 其中，$\displaystyle\binom{N}{m} = \frac{N!}{(N-m)!m!}$.
   - 二项式分布是n个独立实验序列中成功次数p的离散概率分布。
   - 二项式分布是通过预先指定要选取的数量来考虑先验概率的分布。
<p align="center"><img width="400" src="https://github.com/uneypan/distribution-is-all-you-need/raw/master/graph/binomial.png" /></p>

4. **多维伯努利分布，分类分布（离散）**   [Python Code](https://github.com/uneypan/distribution-is-all-you-need/blob/master/categorical.py)
   - 多伯努利称为分类分布，是一种维度大于2的分布。
   - **交叉熵**具有相同的形式，类似于多伯努利分布的负对数。
<p align="center"><img width="400" src="https://github.com/uneypan/distribution-is-all-you-need/raw/master/graph/categorical.png" /></p>

5. **多项式分布（离散）**   [Python Code](https://github.com/uneypan/distribution-is-all-you-need/blob/master/multinomial.py)
   - $\displaystyle\mathrm{Mult}(m_1,m_2,\dots,m_K \mid \boldsymbol{\mu},N)= \binom{N}{m_1m_2\dots m_K}\prod_{k=1}^{K} \mu_k^{m_k}$.
   - 其中，$\displaystyle\binom{N}{m_1m_2\dots m_K} = \frac{N!}{m_1!m_2!\dots m_K!}$.
   - 多项式分布与分类分布的关系与伯努利分布和二项分布之间的关系相同。
<p align="center"><img width="400" src="https://github.com/uneypan/distribution-is-all-you-need/raw/master/graph/multinomial.png" /></p>

6. **Beta分布（连续）**   [Python Code](https://github.com/uneypan/distribution-is-all-you-need/blob/master/beta.py)
   - $\displaystyle\mathrm{Beta}(\mu\mid a,b) = \frac{\Gamma(a+b)}{\Gamma(a)\Gamma(b)}\mu^{a-1}(1-\mu)^{b-1}$.
   - Beta分布与二项分布和伯努利分布共轭。
   - 当Beta分布满足特殊情况(α=1,β=1)时为均匀分布。
<p align="center"><img width="400" src="https://github.com/uneypan/distribution-is-all-you-need/raw/master/graph/beta.png" /></p>

7. **狄利克雷分布（连续）**   [Python Code](https://github.com/uneypan/distribution-is-all-you-need/blob/master/dirichlet.py)
   - $\displaystyle\mathrm{Dir}(\boldsymbol{\mu\mid \alpha}) = \frac{\Gamma(\alpha_0)}{\Gamma(\alpha_1)\cdots\Gamma(\alpha_K)} \prod_{k=1}^{K} \mu_k^{\alpha_k-1}$.
   - 狄利克雷分布是多项式分布的共轭先验，是Beta分布的推⼴。
   - k=2 时变成Beta分布.
<p align="center"><img width="400" src="https://github.com/uneypan/distribution-is-all-you-need/raw/master/graph/dirichlet.png" /></p>

8. **Gamma分布（连续）**   [Python Code](https://github.com/uneypan/distribution-is-all-you-need/blob/master/gamma.py)
   - $\displaystyle\text{Gam}(\lambda\mid a,b)=\frac{1}{\Gamma(a)}b^a\lambda^{a-1}\exp\{-b\lambda\}$
   - $\text{Gam}(a,1) / \text{Gam}(a,1) + \text{Gam}(b,1)$转化为 $\text{Beta}(a,b)$.
   - 指数分布和卡方分布是Gamma分布的特例。
<p align="center"><img width="400" src="https://github.com/uneypan/distribution-is-all-you-need/raw/master/graph/gamma.png" /></p>

9. **指数分布（连续）**   [Python Code](https://github.com/uneypan/distribution-is-all-you-need/blob/master/exponential.py)
   - $\displaystyle \text{Exp}(x\mid\lambda )={\begin{cases}\lambda e^{-\lambda x}&x\geq 0,\\0&x<0.\end{cases}}$
   - 指数分布是 Gamma分布当a为1时的特殊情况。
<p align="center"><img width="400" src="https://github.com/uneypan/distribution-is-all-you-need/raw/master/graph/exponential.png" /></p>

10. **高斯分布（连续）**   [Python Code](https://github.com/uneypan/distribution-is-all-you-need/blob/master/gaussian.py)
    - 一元高斯分布  
    $\displaystyle \mathcal{N}(x\mid\mu,\sigma^2)=\frac{1}{(2\pi\sigma)^{1/2}}\exp \left\{-\frac{1}{2\sigma^2}(x-\mu)^2\right\}$
    - 多元高斯分布（$D$维）   
    $\displaystyle\mathcal{N}(\boldsymbol{x} \mid \boldsymbol{\mu}, \boldsymbol{\Sigma})=\frac{1}{(2 \pi)^{\frac{D}{2}}} \frac{1}{\mid \boldsymbol{\Sigma}\mid ^{\frac{1}{2}}} \exp \left\{-\frac{1}{2}(\boldsymbol{x}-\boldsymbol{\mu})^{T} \boldsymbol{\Sigma}^{-1}(\boldsymbol{x}-\boldsymbol{\mu})\right\} $.
    - 一元⾼斯分布的均值$\mu$的共轭先验是⾼斯分布，精度$\sigma^{-1}$的共轭先验是Gamma分布.
    - 多元⾼斯分布 $\boldsymbol{\mu}$ 的共轭先验仍然是⾼斯分布，精度 $\boldsymbol{\Lambda}$ 的共轭先验是⼀个Wishart分布，$(\boldsymbol{\mu},\boldsymbol{\Lambda})$ 的共轭先验是⾼斯-Wishart分布 $p(\mu,\Lambda\mid \mu_0,\beta,W,\nu) = \mathcal{N}(\mu\mid \mu_0,(\beta\Lambda)^{-1})\mathcal{W}(\Lambda\mid W,\nu)$。
<p align="center"><img width="400" src="https://github.com/uneypan/distribution-is-all-you-need/raw/master/graph/gaussian.png" /></p>

11. **正态分布（连续）**   [Python Code](https://github.com/uneypan/distribution-is-all-you-need/blob/master/normal.py)
    - $\displaystyle \mathcal{N}(x)=\frac{1}{(2\pi)^{1/2}}\exp \left(-\frac{x^2}{2}\right)$.
    - 正态分布是标准化的高斯分布，平均值为0，标准值为1。
<p align="center"><img width="400" src="https://github.com/uneypan/distribution-is-all-you-need/raw/master/graph/normal.png" /></p>

12. **卡方分布（连续）**   [Python Code](https://github.com/uneypan/distribution-is-all-you-need/blob/master/chi-squared.py)
    - $\displaystyle  \chi^2(x\mid k)=\frac{x^{\frac{k}{2}-1}}{2^{\frac{k}{2}}\Gamma({\frac{k}{2}})}  \exp\left(-\frac{x}{2}\right)$.
    - 具有k个自由度的卡方分布是k个独立标准正态随机变量的平方和的分布。
    - 卡方分布是Beta分布的特例。
<p align="center"><img width="400" src="https://github.com/uneypan/distribution-is-all-you-need/raw/master/graph/chi-squared.png" /></p>

13. **学生-t分布（连续）**   [Python Code](https://github.com/uneypan/distribution-is-all-you-need/blob/master/student-t.py)
    - $\displaystyle \text{St}(x\mid\mu,\lambda,\nu) = \frac{\Gamma(\frac{\nu}{2}+\frac{1}{2})}{\Gamma(\frac{\nu}{2})}\left(\frac{\lambda}{\pi\nu}\right)^{\frac{1}{2}}\left[ 1+ \frac{\lambda(x-\mu)^2}{\nu}\right]^{-\frac{\nu}{2}-\frac{1}{2}}$
    - 与正态分布一样，t分布是对称的钟形分布，但尾部较重，这意味着它更容易产生远低于平均值的值。
    - 对于ν = 1的情况，t分布变为了柯西分布（Cauchy distribution）， ⽽在极限ν → ∞ 的情况 下，t分布 $\text{St}(x\mid\mu,\lambda,\nu)$ 变成了⾼斯分布 $\mathcal{N}(x\mid\mu,\lambda-1)$。
    - 在⼀元变量的形式下，学⽣t分布可以通过下列⽅式获得：拿出⼀元⾼斯分布的精度的共轭先验，然后把精度变量积分出来。因此这个分布可以看成⽆限多个有着相同均值不同⽅差的⾼斯分布的混合。
<p align="center"><img width="400" src="https://github.com/uneypan/distribution-is-all-you-need/raw/master/graph/student_t.png" /></p>

14. **Wishart分布（连续）**
   - $\mathcal{W}(\Lambda\mid W, \nu) = B(W, \nu)\mid \Lambda\mid ^{(\nu-D-1)/2}\exp\left(-\frac{1}{2}\text{Tr}(W^{-1}\Lambda)\right)$ 
   - 其中，归一化系数为 $B(W,\nu) = \mid W\mid ^{-\nu/2}\left(2^{\nu D/2}\pi^{D(D-1)/4}\prod\limits_{i=1}^D\Gamma\left(\frac{\nu+1-i}{2}\right)\right)^{-1}$

## Reference

[1] [Tae Hwan Jung - Distribution is All You Need](https://github.com/graykode/distribution-is-all-you-need)

[2] [Wikipedia - Relationships among Probability Distributions](https://en.wikipedia.org/wiki/Relationships_among_probability_distributions)

[3] [Bishop - Pattern Recognition and Machine Learning, 2006](https://www.microsoft.com/en-us/research/uploads/prod/2006/01/Bishop-Pattern-Recognition-and-Machine-Learning-2006.pdf)