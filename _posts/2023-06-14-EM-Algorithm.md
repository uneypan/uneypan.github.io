---
layout: article
title: EM算法
mathjax: true
mermaid: false
---

如果我们定义观测变量和潜在变量的⼀个联合概率分布，那么对应的观测变量本⾝的概率分 布可以通过求边缘概率的⽅法得到。**EM算法用于解决具有隐变量的混合模型的极大似然估计问题**。

## EM算法公式

MLE 问题：$P(x\mid \theta)$

$$\displaystyle \theta_{MLE} = \arg\max_{\theta} \underbrace{\log P(x\mid \theta)}_{\mathrm{log-likelihood}}$$

EM算法的公式：

$$ \begin{split}
\theta^{(t+1)} &= \arg\max_{\theta} \int_{z} \underbrace{\log P(x,z\mid \theta)}_{完整数据，对数联合概率} \cdot \underbrace{P(z\mid x,\theta^{(t)})}_{后验概率} \mathrm{d} z\\ 
&=\arg\max_{\theta}\quad E_{z\mid x,\theta^{(t)}}[\log P(x,z\mid \theta)]
\end{split}$$

其中$x$是给定数据，$z$是隐变量，$\theta^{(t)}$ 是t时刻的参数。

这个公式包含了迭代的两步：

$1.$ E step：计算 $\log p(x,z\mid \theta)$ 在概率分布 $p(z\mid x,\theta^t)$ 下的期望
$2.$ M step：计算使这个期望最大化的参数得到下一个 EM 步骤的输入

显然，EM是一个递推算法。


## EM算法的收敛性证明
收敛性：从 $\theta^{(t)} \rightarrow \theta^{(t+1)}$ 必须满足条件：

$$\log P(x\mid \theta^{(t)}) \le \log P(x\mid \theta^{(t+1)})$$

先写通项，显然
$$\log P(x\mid \theta) = \log P(x,z\mid \theta) - \log P(z\mid x,\theta)$$

接下来有技巧性，两边同时对 $P(z\mid x,\theta^{(t)})$ 求积分


$$ \begin{split}
左边 = \log P(x\mid \theta) \underbrace{\int_z P(z\mid x,\theta^{(t)})\mathrm{d}z}_{只跟z有关，积分等于1}= \log P(x\mid \theta)
\end{split}$$

等于什么都没做。


$$ \begin{split} 
右边 &=\underbrace{ \int_z \log P(x,z\mid \theta) P(z\mid x,\theta^{(t)}) \mathrm{d}z}_{定义为Q(\theta,\theta^{(t)})} - \underbrace{\int_z P(z\mid x,\theta^{(t)}) \log P(z\mid x,\theta) \mathrm{d}z}_{定义为H(\theta,\theta^{(t)})} 
\end{split} $$

因此证明 $\log P(x\mid \theta^{(t)}) \le \log P(x\mid \theta^{(t+1)})$

相当于证明 $Q(\theta^{(t)},\theta^{(t)})-H(\theta^{(t)},\theta^{(t)}) \le Q(\theta^{(t+1)},\theta^{(t)})-H(\theta^{(t+1)},\theta^{(t)})$

由 $\theta^{(t+1)}$ 的定义

$$ \begin{split} 
\theta^{(t+1)} &= \arg\max_{\theta} \int_{z} \log P(x,z\mid \theta)\cdot P(z\mid x,\theta^{(t)}) \mathrm{d} z \\
&= \arg\max_{\theta} Q(\theta,\theta^{(t)})
\end{split} $$

很明显，$Q(\theta^{(t+1)},\theta^{(t)}) \ge Q(\theta^{(t)},\theta^{(t)})$. 

然后证明 $H(\theta^{(t+1)},\theta^{(t)}) \le H(\theta^{(t)},\theta^{(t)})$, 两项相减：

$$
\begin{split} &\ \ \ \ \ \ H(\theta^{(t+1)},\theta^{(t)}) - H(\theta^{(t)},\theta^{(t)})\\&=\int_z P(z\mid x,\theta^{(t)})\cdot \log P(z\mid x,\theta^{(t+1)})dz-\int_z P(z\mid x,\theta^{(t)})\cdot \log P(z\mid x,\theta^{(t)})dz\\ &=\int_z P(z\mid x,\theta^{(t)})\cdot \log {P(z\mid x,\theta^{(t+1)})\over P(z\mid x,\theta^{(t)})}dz\\ \end{split}
$$

这里如果对 KL 散度熟悉的话，可以直接看出此式等于 $-KL(P(z\mid x,\theta^{(t)}) \Vert P(z\mid x,\theta^{(t+1)}))$

> Kullback–Leibler divergence: ${\displaystyle KL(P\parallel Q)=\int _{-\infty }^{\infty }p(x)\log \left({\frac {p(x)}{q(x)}}\right)\,\mathrm{d}x} $

而 $KL(\cdot) \ge 0$ 故 $-KL(P(z\mid x,\theta^{(t)}) \Vert P(z\mid x,\theta^{(t+1)})) \le 0$

所以可知 $H(\theta^{(t+1)},\theta^{(t)}) - H(\theta^{(t)},\theta^{(t)}) \le 0$

也可以使用 Jensen 不等式来解决，由于 $\log(x)$ 为凹函数，可以看出 $E[\log x] \le \log E[x]$.

> Jensen 不等式: 若 $f(x)$ 是凸函数，则 $E[f(x)] \ge f(E[x])$

因此

$$\begin{split} &\ \ \ \ \ H(\theta^{(t+1)},\theta^{(t)}) - H(\theta^{(t)},\theta^{(t)})\\ &=\int_z P(z\mid x,\theta^{(t)})\cdot \log {P(z\mid x,\theta^{(t+1)})\over P(z\mid x,\theta^{(t)})}\mathrm{d}z\\ &\le \log \int_z P(z\mid x,\theta^{(t)})\cdot {P(z\mid x,\theta^{(t+1)})\over P(z\mid x,\theta^{(t)})}\mathrm{d}z\\ &=\log \int_z P(z\mid x,\theta^{(t+1)})\mathrm{d}z\\ &=0 \end{split}$$

所以 $H(\theta^{(t+1)},\theta^{(t)}) - H(\theta^{(t)},\theta^{(t)}) \le0$.

因此得证 $Q(\theta^{(t)},\theta^{(t)})-H(\theta^{(t)},\theta^{(t)}) \le Q(\theta^{(t+1)},\theta^{(t)})-H(\theta^{(t+1)},\theta^{(t)})$.

故 $\log P(x\mid \theta^{(t)}) \le \log P(x\mid \theta^{(t+1)})$.


## 混合⾼斯模型

![](/pictures/prml/图2.21.png)

这个数据集被称为“⽼忠实间歇喷泉”数据集，由美国黄⽯国家公园的⽼忠实间歇喷泉的272次喷发的测量数据组成。

可以看到使用最⼤似然法（左）拟合的单一高斯分布不能描述数据中两个聚集区域，峰值区域内数据稀疏。而使用两个高斯分布线性组合得到的概率分布（右）给出了数据的一个更好的表示。

通过将更基本的概率分布（例如⾼斯分布）进⾏线性组合的这样的叠加⽅法，可以被形式化为概率模型，被称为混合模型（mixture distributions）。

⼀维⾼斯混合分布的例⼦

![](/pictures/prml/图2.22.png)

⼆维空间中3个⾼斯分布混合的例⼦

![](/pictures/prml/图2.23.png)

K 个⾼斯概率密度的叠加，形式为

$$
p(x) = \sum_{k=1}^{K} \pi_k \cdot \mathrm{N}(x \mid \mu_k,\Sigma_k)\\\quad \sum_{k=1}^{K} \pi_k =1, \quad 0 \le \pi_k \le 1.
$$

把 $\pi_k = p(k)$ 看成第k个成分的先验概率，密度 $\mathrm{N}(x \mid \mu_k,\Sigma_k)$ 看成以k为条件的x的概率。边缘概率密度为

$$
p(x)= \sum_{k=1}^{K} p(k)p(x\mid k)
$$

根据贝叶斯定理，后验概率 $\gamma_k(x)$ 可以表⽰为

$$\begin{split}
\gamma_k(x) &= p(k\mid x)\\
&=\frac{p(k)p(x\mid k)}{\sum_l p(l)p(x\mid l)}\\
&=\frac{\pi_k \mathrm{N}(x\mid \mu_k,\Sigma_k)}{\sum_l \pi_l \mathrm{N}(x\mid\mu_l,\Sigma_l)}
\end{split}$$

对数似然函数为

$$
\ln p(\mathbf{X}\mid\mathbf{\pi},\mathbf{\mu},\mathbf{\Sigma}) = \sum_{n=1}^{N} \ln \left\{\sum_{k=1}^{K} \pi_k \mathrm{N}(x_n\mid\mu_k,\Sigma_k) \right\}
$$

因为对数中存在⼀个求和式，导致参数的最⼤似然解不再有⼀个封闭形式的解析解。⼀种最⼤化这个似然函数的⽅法是使⽤迭代数值优化⽅法，另⼀种⽅法便是EM算法。

## K 均值聚类

K 均值算法对应于⽤于⾼斯混合模型的EM算法的⼀个特定的⾮概率极限。