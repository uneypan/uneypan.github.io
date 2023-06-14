---
layout: article
title: EM算法
mathjax: true
mermaid: false
---

## EM算法公式

MLE 问题：$P(x\mid \theta)$

$$\displaystyle \theta_{MLE} = \arg\underset{\theta}{\max} \underbrace{\log P(x\mid \theta)}_{\mathrm{log-likelihood}}$$

EM算法的公式：

$$ \begin{split}
\theta^{(t+1)} &= \argmax_{\theta} \int_{z} \underbrace{\log P(x,z\mid \theta)}_{完整数据，对数联合概率} \cdot \underbrace{P(z\mid x,\theta^{(t)})}_{后验概率} \mathrm{d} z\\ 
&=\argmax_{\theta}\quad E_{z\mid x,\theta^{(t)}}[\log P(x,z\mid \theta)]
\end{split}$$

其中$x$是给定数据，$z$是隐变量，$\theta^{(t)}$ 是t时刻的参数。

这个公式包含了迭代的两步：

$1.$ E step：计算 $\log p(x,z\mid \theta)$ 在概率分布 $p(z\mid x,\theta^t)$ 下的期望
$2.$ M step：计算使这个期望最大化的参数得到下一个 EM 步骤的输入

显然，EM是一个递推算法。EM算法用于解决具有隐变量的混合模型的极大似然估计问题。


## EM算法的收敛性证明
收敛性：从 $\theta^{(t)} \rightarrow \theta^{(t+1)}$ 必须满足条件：

$$\log P(x\mid \theta^{(t)}) \le \log P(x\mid \theta^{(t+1)})$$

先写通项，显然
$$\log P(x\mid \theta) = \log P(x,z\mid \theta) - \log P(z\mid x,\theta)$$

接下来有技巧性，两边同时对 $P(z\mid x,\theta^{(t)})$ 求积分


$$ \begin{split}
左边 = \log P(x\mid \theta) \int_z \underbrace{P(z\mid x,\theta^{(t)})}_{只跟z有关，积分等于1}\mathrm{d}z= \log P(x\mid \theta)
\end{split}$$

等于什么都没做。


$$ \begin{split} 
右边 &=\underbrace{ \int_z \log P(x,z\mid \theta) P(z\mid x,\theta^{(t)}) \mathrm{d}z}_{定义为Q(\theta,\theta^{(t)})} - \underbrace{\int_z P(z\mid x,\theta^{(t)}) \log P(z\mid x,\theta) \mathrm{d}z}_{定义为H(\theta,\theta^{(t)})} 
\end{split} $$

因此证明 $\log P(x\mid \theta^{(t)}) \le \log P(x\mid \theta^{(t+1)})$

相当于证明 $Q(\theta^{(t)},\theta^{(t)})-H(\theta^{(t)},\theta^{(t)}) \le Q(\theta^{(t+1)},\theta^{(t)})-H(\theta^{(t+1)},\theta^{(t)})$

由 $\theta^{(t+1)}$ 的定义

$$ \begin{split} 
\theta^{(t+1)} &= \argmax_{\theta} \int_{z} \log P(x,z\mid \theta)\cdot P(z\mid x,\theta^{(t)}) \mathrm{d} z \\
&= \argmax_{\theta} Q(\theta,\theta^{(t)})
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

也可以使用 Jensen 不等式来解决，由于 $\log(x)$ 为凹函数，可以看出 $E[\log x] \le \log E[x]$

> Jensen 不等式: 若 $f(x)$ 是凸函数，则 $E[f(x)] \ge f(E[x])$

因此

$$\begin{split} &\ \ \ \ \ H(\theta^{(t+1)},\theta^{(t)}) - H(\theta^{(t)},\theta^{(t)})\\ &=\int_z P(z\mid x,\theta^{(t)})\cdot \log {P(z\mid x,\theta^{(t+1)})\over P(z\mid x,\theta^{(t)})}\mathrm{d}z\\ &\le \log \int_z P(z\mid x,\theta^{(t)})\cdot {P(z\mid x,\theta^{(t+1)})\over P(z\mid x,\theta^{(t)})}\mathrm{d}z\\ &=\log \int_z P(z\mid x,\theta^{(t+1)})\mathrm{d}z\\ &=0 \end{split}$$

所以 $H(\theta^{(t+1)},\theta^{(t)}) - H(\theta^{(t)},\theta^{(t)}) \le0$

因此得证 $Q(\theta^{(t)},\theta^{(t)})-H(\theta^{(t)},\theta^{(t)}) \le Q(\theta^{(t+1)},\theta^{(t)})-H(\theta^{(t+1)},\theta^{(t)})$

故 $\log P(x\mid \theta^{(t)}) \le \log P(x\mid \theta^{(t+1)})$