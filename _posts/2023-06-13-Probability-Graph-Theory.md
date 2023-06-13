---
layout: article
title: 概率图理论
mathjax: true
---

# 概率图模型 PGM

## 知识结构

![概率图-知识结构](/pictures/概率图-知识结构.jpg)


## 概率图模型基础
高维随机变量 $P(x_1,x_2,\dots,x_p)$:
- 边缘概率 $P(x_i)$
- 条件概率 $P(x_j\mid x_i)$

Sum Rule: $ P(x_1) = \int P(x_1,x_2)\mathrm{d} x_2 $

Product Rule: $P(x_1,x_2) = P(x_1) \cdot P(x_2\mid x_1) = P(x_2) \cdot P(x_1\mid x_2)$

Chain Rule: $P(x_1,x_2, \dots,x_p) = \displaystyle\prod_{i=1}^{p} P(x_i\mid x_1,x_2, \dots,x_{i-1})$

Bayesian Rule: $P(x_2\mid x_1) = \dfrac{P(x_1,x_2)}{P(x_1)}=\dfrac{P(x_1,x_2)}{\displaystyle\int P(x_1,x_2)\mathrm{d} x_2} = \dfrac{P(x_1,x_2)}{\displaystyle\int P(x_2) \cdot P(x_1\mid x_2)\mathrm{d} x_2} $

困境：维度高，求解复杂。$P(x_1,x_2, \dots,x_p)$ 计算量太大。

假设：互相独立

Naive Bayes:

$$ P(x_1,x_2, \dots,x_p) = \prod_{i=1}^{P} P(x_i)$$

假设： Markov Property

HMM （齐次 Markov 假设）：

$$ x_j \perp x_{i+1} \mid  x_i, \quad j<i  $$

假设： 条件独立性

$$ x_A \perp x_B \mid  x_c,\quad x_A,x_B,x_C\ 是集合，且不相交$$


## 贝叶斯网络 - 条件独立性

因子分解

$$
P(x_1,x_2,\dots,x_p) = \prod_{i=1}^{p} P(x_i\mid x_{pa(i)}) 
$$

$ x_{pa(i)} $ 是 $ x_i $ 的父亲集合。

![三种节点类型](/pictures/三种节点类型.jpg)

*1. tail to tail*  
若a被观测，则路径被阻塞。  
证明：  
因子分解 $P(a,b,c) =P(a)P(b\mid a)P(c\mid a)$  
链式法则 $P(a,b,c) =P(a)P(b\mid a)P(c\mid a,b)$  
消元 $P(c\mid a) = P(c\mid a,b)$   
由于 $P(c\mid a)\cdot P(b\mid a) = P(c\mid a,b)\cdot P(b\mid a) = P(b,c\mid a)$   
推出 $P(c\mid a)\cdot P(b\mid a) = P(b,c\mid a)$，即 $c \perp b \mid a$。 

*2. tail to head*  

若b被观测，则路径被阻塞。

*3. head to head*  

默认情况下，a $\perp$ b，路径是阻塞的，若 c（或其后继节点）被观测，则路径是通的。  

head to head 比较费解，举例：  
- a  酒量小
- b 心情不好
- c 喝醉 

显然 $P(a) = P(a \mid b)$。已知小明喝醉的情况下酒量小的概率 $P(a\mid c)$，明显大于已知小明喝醉且心情不好的情况下，酒量小的概率 $P(a\mid b,c)$ ，即 $P(a\mid c) > P(a\mid b,c)$，说明 a 与 b 在给定 c 时不独立。


## 贝叶斯网络 - D-Seperation

$$
P(x_i \mid x_{-i}) = \frac{P(x_i,x_{-i})}{P(x_{-i})} = \frac{P(x_i,x_{-i})}{\int_{x_i} P(x)\mathrm{d}x_i }=\frac{ \prod_{j=1}^{p}P(x_j\mid x_{pa(j)}) }{ \int_{x_i} \prod_{j=1}^{p}P(x_j\mid x_{pa(j)})\mathrm{d}x_i  }
$$

<!-- $$
= P(x_i\mid x_{pa(i)}) + P(x_{ch(i)} \mid x_i,x_{pa[ch(i)]}))
$$ -->
(马尔科夫毯)

## 贝叶斯网络 - 知识结构

![Bayesian_Network](/pictures/Bayesian_Network.png)

## Markov Network（Markov Random Field 马尔可夫随机场）

条件独立性体现在三个方面：

$1$. Global Markov Property：如果 $A,B,C,$ 存在 $ \mathrm{Sep}(A,C\mid B)$，那么 $x_A \perp x_c \mid x_B$.

$2$. Local Markov Property：$x_i \perp {x_{-i-nb(i)}} \mid x_{nb(i)}$.

$3$. Pair Markov Property：$x_i \perp x_j \mid x_{-i-j},\quad x_i,x_j$ 没有边连接.

根据Hammesley-Clifford定理， 可得条件独立性的三种情况与基于最大团的因子分解等价 $1\Leftrightarrow 2\Leftrightarrow 3\Leftrightarrow $因子分解



> 团：一个关于节点的集合，集合中的节点之间相互都是连通的。    
最大团：一个团内不能再添加节点的团。

基于最大团的因子分解：
$$ P(x) = \frac{1}{Z} \prod_{i=1}^{K} \psi(x_{C_i}) $$
$$ Z = \sum_{x_1,x_2,\dots,x_p} \prod_{i=1}^{K} \psi(x_{c_i})$$

$C_1,C_2,\dots,C_K$: K个最大团  
$x_{C_i}$: 最大团随机变量的集合    
$Z$: 归一化因子   
$\psi(x_{C_i})$: 势函数，必须为正  

$$\psi(x_{C_i}) = \exp{\{-E(x_{C_i})\}} > 0$$

$P(x)$ 称为 Gibbs Distribution (Boltzmann Distribution)，

$$
P(x) = \dfrac{1}{Z} \prod_{i=1}^{K} \psi(x_{C_i}) = \dfrac{1}{Z} \prod_{i=1}^{K} \exp{\{-E(x_{C_i})\}} = \dfrac{1}{Z}\exp{\{-\sum_{i=1}^{K}E(x_{C_i})\}}
$$

形式上为指数族分布 

$$
P(x) = h(x)\exp \{\eta^{\mathrm{T}}\phi(x)-A(\eta) \} = \dfrac{1}{Z(\eta)} h(x) \exp \{{ \eta^{\mathrm{T}}\phi(x) \}}
$$


Markov Random Field $\Leftrightarrow$ Gibbs Distribution，证明略。

## Inference - 知识结构

Tasks: 给定 $P(x) = P(x_1,x_2,\dots,x_p)$ 求：

$1.$边缘概率： $P(x_i) = \displaystyle\sum_{x_1}\dots\sum_{x_{i-1}}\sum_{x_{i+1}}\dots\sum_{x_p}P(x)$;

$2.$条件概率（后验概率）：$P(x_A\mid x_B),\quad x = x_A \cup x_B$;   

$3.$最大后验 MAP: $\hat{z} = \displaystyle \argmax_{z} P(z\mid x) \propto \argmax P(z,x)$ ;    

![Inferece知识结构](/pictures/Inferece知识结构.png)


HMM Inference 中的三个问题:

$1.$ Evalution 边际化: $P(O) = \sum_{I} P(I,O) \Rightarrow$ 解决方法：前向、后向；

$2.$ Learning: $\hat{\lambda}$；

$3.$ Decoding: $\tilde{I} = \arg\max_{I} P(I\mid O)$ $\Rightarrow$ Vitebi Algorithm(动态规划).

HMM $\Rightarrow$ Dynamic Bayesian Network

## Inference - Variable Elimination 变分推断
(VE的核心思想：乘法分配律)
![hmm](/pictures/hmm.png)
$$
P(d) = \sum_{a,b,c} P(a,b,c,d) = \sum_{a,b,c}P(a)P(b\mid a)P(c\mid b) P(d\mid c)
$$
$$
= \sum_{b,c} P(c\mid b)P(d\mid c) \underbrace{\sum_a P(a)P(b\mid a)}_{\phi_a(b)} 
= \sum_{c} P(d\mid c) \underbrace{\sum_b P(c\mid b) \phi_a(b)}_{\phi_b(c)}= \phi_c(d)
$$


推广到无向图，使用最大团分解，$K$ 是最大团的个数： 

$$P(a,b,c,d) = \dfrac{1}{Z}\prod_{i=1}^{K}\phi_{C_i}(x_{Ci})$$

由于是最大团，不能再添加其他结点，因此团与团的联系较小，同时每一个结点几乎不可能同时存在于所有团，因此同样可以使用Variable Elimination方法进行计算。

VE算法的局限性：

- 上述例子只计算了结点 d ，若需要计算其他结点，又要重新计算，每次计算的中间结果没有存储，导致重复计算。
- 变量消除的次序会决定时间复杂度，已被证明找到最优次序是一个NP-Hard问题，因此也可以使用一些启发式思想进行次序选择。

## Inference-Belief Propagation

以5节点链为例，先进行因子分解：
$$
P(a,b,c,d,e)=P(a)P(b|a)P(c|b)P(d|c)P(e|d)
$$

$$
\begin{split} P(e)&=\sum_{a,b,c,d} P(a,b,c,d,e)\\ &=\sum_dP(e|d)\sum_cP(d|c)\sum_bP(c|b)\sum_aP(b|a)P(a)\\ &=\sum_dP(e|d)\sum_cP(d|c)\sum_bP(c|b) \color{red}{m_{a\to b}(b)}\\ &=\sum_dP(e|d)\sum_cP(d|c) \color{red}{m_{b\to c}(c)}\\ &=\sum_dP(e|d) \color{red}{m_{c\to d}(d)}\\ &= \color{red}{m_{d\to e}(e)}\\ \end{split}
$$

上述是一个前向传播过程（Forward Algorithm），类似于HMM的过程。

若我们需要求 $P(c)$ ,则：

$$
P(c)=(\sum_{b}P(c|b)\sum_aP(b|a)P(a))\cdot(\sum_{d}P(d|c)\sum_eP(e|d))
$$

先从 a 到 b , 再从 e 到 d ，先前向传播，再反向传播（Forward-Backward Algorithm）
其中前向传播的计算与求 $P(e)$ 的部分计算一样，重复计算。

从这个例子可以看出上节讲的VE算法的缺点：重复计算。
本节引入Belief Propagation算法，其基本思想与VE一致  
不同的是：将中间过程存储起来，简化计算，避免重复计算。

以 P(a) 为例：
$$
\begin{split} P(a)&=\sum_{b,c,d}P(a,b,c,d)\\ &=\psi_a\sum_b((\sum_c\psi_c\psi_{b,c})\cdot \psi_b\cdot(\sum_d\psi_d\psi_{b,d})\cdot \psi_{ab})\\ &=\psi_a\sum_b(m_{c\to b}(b)\cdot \psi_b\cdot m_{d\to b}(b)\cdot \psi_{ab})\\ &=\psi_a m_{b\to a}(a) \end{split}
$$

此式共有两个部分：
$$
\begin{cases} m_{b \to a}(a)=\displaystyle\sum_b \psi_{a,b} \psi_b m_{c\to b}(b)m_{d\to b}(b)\\ P(a)=\psi_a m_{b \to a}(a) \end{cases}
$$

进行简化( $NB(b)$ 为 b 结点的邻居结点所组成的集合)：
$$
\begin{cases} m_{b \to a}(a)=\displaystyle\sum_b \psi_{a,b} \psi_b \prod_{k\in NB(b)}m_{k \to b}(b)\\ P(a)=\psi_a m_{b \to a}(a) \end{cases}
$$

再进行归纳（generalize），得到边缘概率的递推公式：
$$
\begin{cases} m_{j \to i}(i)=\displaystyle\sum_j \psi_{i,j} \psi_j \prod_{k\in NB(j)}m_{k \to j}(j)\\ P(i)=\psi_i \displaystyle\prod _{k\in NB(i)}m_{k \to i}(i) \end{cases}
$$

其中 $\psi_j \prod_{k\in NB(j)}m_{k \to j}(j)$ 称为 $Belief(j) m_{k \to j}$ 称为 $j$ 的Child.
$$
\begin{cases} belief(j)=\psi(j)\cdot Children(j)\\ m_{j \to i } = \displaystyle\sum_j\psi_{i,j}\cdot belief(j) \end{cases}
$$

根据避免重复计算的思路，可得出结论：

不要直接求边缘概率 $(P(a),P(b),\cdots)$ ，只需求 $m_{i\to j}$

相当于做了一个cach，存储中间结果

那么如何求 $m_{i\to j}$ ，就需要Belief Propagation.


