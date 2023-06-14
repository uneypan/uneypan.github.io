---
layout: article
title: EM算法
mathjax: true
---

## EM算法公式以及算法收敛性证明

MLE 问题：$P(x\mid \theta)$

$$\displaystyle \theta_{MLE} = \argmax_{\theta} \underbrace{\log P(x\mid \theta)}_{log-likelihood}$$

EM算法的公式：
$$
\theta^{(t+1)} = \argmax_{\theta} \int_{z} \underbrace{\log P(x,z\mid \theta)}_{E_{z\mid x,\theta^{(t)}}[\log P(x,z\mid \theta)]} \cdot \underbrace{P(z\mid x,\theta^{(t)})}_{后验} \mathrm{d} z
$$

其中$x$是给定数据，$z$是隐变量，$\theta^{(t)}$ 是t时刻的参数。

显然，EM是一个递推算法。EM算法用于解决具有隐变量的混合模型的极大似然估计问题。

不严谨证明：
从 $\theta^{(t)} \rightarrow \theta^{(t+1)}$ 必须满足条件：

$$\log P(x\mid \theta^{(t)}) \le \log P(x\mid \theta^{(t+1)})$$

先写通项，显然
$$\log P(x\mid \theta) = \log P(x,z\mid \theta) - \log P(z\mid x,\theta)$$

接下来有技巧性，两边同时对 $P(z\mid x,\theta^{(t)})$ 求积分

$$ \begin{split}
左边 = \log P(x\mid \theta) \int_z \underbrace{P(z\mid x,\theta^{(t)})}_{只跟z有关，积分等于1}\mathrm{d}z= \log P(x\mid \theta)
\end{split}$$

$$ \begin{split} 
右边 &=\underbrace{ \int_z \log P(x,z\mid \theta) P(z\mid x,\theta^{(t)}) \mathrm{d}z}_{Q(\theta,\theta^{(t)})} - \underbrace{\int_z P(z\mid x,\theta^{(t)}) \log P(z\mid x,\theta) \mathrm{d}z}_{H(\theta,\theta^{(t)})} 
\end{split} $$


由$\theta^{(t+1)}$的定义， $Q(\theta^{(t+1)},\theta^{(t)}) \ge Q(\theta^{(t)},\theta^{(t)})$. 

我们要证明 $H(\theta^{(t+1)},\theta^{(t)}) \le H(\theta^{(t)},\theta^{(t)})$:

$$ \begin{split} 
H(\theta^{(t+1)},\theta^{(t)}) - H(\theta^{(t)},\theta^{(t)})
\end{split} $$


