---
layout: article
title: 节拍跟踪相关算法
mathjax: true
# mathjax_autoNumber: true
---

## 卡尔曼滤波

### 节拍跟踪器的动力学方程描述

这里讨论在一段较短的时间内，假设音乐中节拍进行的速度是恒定的。设 $t$ 是节拍时刻，$\Delta$ 是节拍周期。已知 $k-1$ 步的节拍时刻和周期，如果不考虑噪声，在 $k$ 时刻有

节拍时间  

$$ \hat{t}(k)= t(k-1)+\Delta(k-1)$$   

节拍周期

$$\hat{\Delta}(k)= \Delta(k-1)$$

于是，可以取 2 维状态向量   

$$\mathrm{x} = \begin{bmatrix} t\\\Delta \end{bmatrix}$$  

在此基础上考虑过程噪声 $\mathrm{v}$，得到状态方程  

$$\mathrm{x}(k)= \mathrm{F}\mathrm{x}(k-1)+\mathrm{v},\quad \mathrm{F}=\begin{bmatrix} 1&1\\0&1\end{bmatrix},\quad k=1,2,3, \ldots $$  

如果考虑观测噪声 $\mathrm{w}$，假设对节拍时刻 $t$ 进行观测，得到 1 维的观测值 $\mathrm{y}(k)=t(k)+\mathrm{w}$，得到观测方程

$$\mathrm{y}(k)=\mathrm{H}\mathrm{x}(k)+\mathrm{w},\quad \mathrm{H}=\begin{bmatrix} 1&0\end{bmatrix} ,\quad k=1,2,3, \ldots$$

在这里，我们假设噪声是高斯的，且其分布不随时间变化。过程噪声 $\mathrm{v}$ 服从均值为零，协方差为 $\mathrm{Q}$ 的高斯分布。测量噪声 $\mathrm{w}$ 服从均值为零，方差为 $\mathrm{R}$ 的高斯分布。

### 最优估计

预测步：先验状态估计、先验估计协方差、先验观测

$$
\hat{\mathrm{x}}(k) = \mathrm{F}{\mathrm{x}}(k-1)
$$

$$
\hat{\mathrm{P}}(k) = \mathrm{F}\mathrm{P}(k-1)\mathrm{F}^{\mathrm{T}}+\mathrm{Q}
$$

$$
\hat{\mathrm{y}}(k) = \mathrm{H}\hat{\mathrm{x}}(k-1)
$$

更新步：计算卡尔曼增益、后验状态估计及对应协方差

$$
\mathrm{S}(k)= \mathrm{H}\mathrm{P}(k)\mathrm{H}^{\mathrm{T}}+\mathrm{R}
$$

$$
\mathrm{K}(k)= \hat{\mathrm{P}}(k)\mathrm{H}^{\mathrm{T}}\mathrm{S}(k)^{-1}
$$

$$
\mathrm{x}(k)=\hat{\mathrm{x}}(k) + \mathrm{K}(k)[\mathrm{y}(k)-\hat{\mathrm{y}}({k})]
$$

$$
\mathrm{P}(k) =\left[\mathrm{I}-\mathrm{K}(k)\mathrm{H}\right]\hat{\mathrm{P}}(k)
$$

### 扩展

> A. T. Cemgil, H. Kappen, P. Desain, and H. Honing. On tempo tracking: Tempogram Representation and Kalman filtering. Journal of New Music Research, 28:4:259–273, 2001.

节拍跟踪器的动力学基本模型可以在几个方向上扩展：

A.放松卡尔曼滤波器的线性约束。在一个由 $w = \log_2 \Delta$ 映射的对数空间中定义状态方程。对数映射使得周期 $\hat{\Delta}$ 永远是正的。对数映射也使得音乐速度的变化在听觉上更佳合理，例如 $\Delta \rightarrow 2\Delta$ 和 $\Delta \rightarrow \Delta/2$ 会产生相同的变化。于是

$$
\hat{\tau}_j = \hat{\tau}_{j-1} + 2^{\hat{w}_{j-1}}
$$

B.引入惯性状态变量 $\hat{a}_j$。将状态变量 $\hat{\mathrm{x}}_j $ 扩展为 $\begin{bmatrix} \hat{\mathrm{x}}_j  & \hat{a}_j \end{bmatrix}^T$。附加变量存储关于过去状态的信息，例如加速度。给系统引入惯性。惯性减少了状态空间中的随机行走行为，并使平滑的状态轨迹更有可能，并可以产生更准确的预测。

C.混合高斯建模的噪声。观测值中的离群值（Outliers）可以通过使用高斯混合来建模。例如，一个“窄”高斯用于建模正常观测，一个为“宽”高斯用于建模离群值。离散变量 $c_j$ 用于指示第 $j$ 个观测是否为离群值。这样，离群值噪声的分布为 

$$\mathrm{w}_j\mid c_j \sim \mathcal{N}(0,\mathrm{R_c})$$ 

由于 $c_j$ 不能被观测，定义先验 $c_j \sim p(c)$，然后对 $c_j$ 进行积分，即

$$ \displaystyle p(\mathrm{w}_j) = \sum_{c_j}p(c_j)p(\mathrm{w}_j\mid c_j) $$

于是，动力学模型变为：

$$
\begin{bmatrix} \hat{\mathrm{x}}_j \\ \hat{a}_j \end{bmatrix} = \mathrm{F} \begin{bmatrix} \hat{\mathrm{x}}_{j-1} \\ \hat{a}_{j-1} \end{bmatrix}+\mathrm{v}_j
$$

$$
\begin{bmatrix} \tau_{j} \\ w_{j} \end{bmatrix} = \begin{bmatrix} \hat{\tau}_{j} \\ \hat{w}_{j} \end{bmatrix} + \mathrm{w}_j
$$

其中，$c_j$ 选取为离散的开关变量，$$\mathrm{v}_j \sim \mathcal{N}(0,\mathrm{Q})$$，$$\mathrm{w}_j\mid c_j \sim \mathcal{N}(0,\mathrm{R_c})$$，且 $$c_j \sim p(c)$$。注意到，这样给出的观测值变为2维的（包括 $\tau_{j}$ 和 $w_{j}$）。

## 节拍图表示

> A. T. Cemgil, H. Kappen, P. Desain, and H. Honing. On tempo tracking: Tempogram Representation and Kalman filtering. Journal of New Music Research, 28:4:259–273, 2001.

> Grosche, Peter, Meinard Müller, and Frank Kurth. “Cyclic tempogram - A mid-level tempo representation for music signals.” ICASSP, 2010.

为了从局部的起始点序列 $ \mathrm{t} = [t_i]$ 中推断出节拍轨 $(\tau,\Delta)$，定义一个贝叶斯概率模型

$$
p(\tau,\Delta,\mid \mathrm{t}) \propto p( \mathrm{t}\mid \tau,\Delta)p(\tau,\Delta)
$$

其中 $ \mathrm{t} = [t_i]$ 是起始点序列，局部节拍 $\tau$ 和局部周期 $\Delta$ 由卡尔曼滤波器给出。公式的第一项 $p( \mathrm{t} \mid \tau,\Delta)$ 是似然函数，可以看成给定节拍轨 $(\tau,\Delta)$ 下观测到起始点序列 $t$ 的概率，第二项 $p(\tau,\Delta)$ 是节拍的先验分布。

一个合理的假设是当节拍轨 $(\tau,\Delta)$ 与观测序列 $[t_i]$ 匹配时，似然函数 $p( \mathrm{t} \mid \tau,\Delta)$ 较大，由此可以构造观测数据与局部节拍轨的相似性度量函数。

首先定义一个连续时域信号 $$x(t) = \sum_{i=1}^I G(t-t_i)$$，其中 $G(t)$ 是一个方差为 $\sigma_x^2$ 的高斯函数 $$G(t)=\exp(-t^2 / 2\sigma_x^2)$$ 。

局部节拍轨由一个脉冲序列 $$\phi(t;\tau,\Delta) = \sum_m \alpha_m \delta(t-\tau - m \Delta)$$表示，其中 $$\delta(t-t_0)$$是一个转换后的 Dirac delta 函数，表示位于 $t$ 处的脉冲。$\alpha_m$ 是脉冲的幅度，是一个正的常数，且满足 $$\sum_m \alpha_m = 1$$。 如果需要计算的因果性，可以在 $m > 0$ 限制 $\alpha_m = 0$ 。当 $\alpha_m$ 是指数响应序列，即 $$\alpha_m = \alpha_0^m $$ 时，它变成无限脉冲响应（IIR）梳状滤波器。定义 $x(t)$ 在 $(\tau,\Delta)$ 处的节拍图（Tempogram）为 $x(t)$ 与脉冲序列的内积

$$
\mathrm{Tg_x}(\tau,\Delta) = \int_{-\infty}^{\infty} x(t)\phi(t;\tau,\Delta)dt
$$

<div align="center">
<img src="/pictures/CemgilTempogramCalculat.png" width=600px>
</div>

节拍图表示可以被解释为梳状滤波器组的响应，并且类似于多尺度表示（例如小波变换），其中 $\tau,\Delta$ 对应于转换和缩放参数。将似然函数定义为节拍图的对数

$$p( \mathrm{t}\mid \tau,\Delta) \propto \exp (\mathrm{Tg_x(\tau,\Delta)})$$

假设 $\tau$ 和 $\Delta$ 的先验概率分布是平坦的，将 $\tau$ 边缘化得到 $\Delta$ 的概率分布 

$$
p(\Delta\mid \mathrm{t}) \propto \int \exp\left( \mathrm{Tg_x}(\tau,\Delta)\right) \mathrm{d} \tau
$$

<img src="/pictures/CemgilTempogram1.png" width=400px> <img src="/pictures/CemgilTempogram2.png" width=400px>

## 流式速度估计

> Percival, Graham, and George Tzanetakis. "Streamlined tempo estimation based on autocorrelation and cross-correlation with pulses." IEEE/ACM Transactions on Audio, Speech, and Language Processing 22.12 (2014): 1765-1776.

Percival 等人提出一种针对音频流的实时速度估计算法，算法利用速度谱图（Tempogram）和累加器分步估计速度，用“广义自相关”和快速傅里叶变换加快自相关运算，在计算量和精度上取得不错的平衡。

### 起始点强度信号

1）分帧：将源音频（44100 Hz）按每帧 1024 个采样点、步长128 点分割，产生一系列采样率约为 344.5 Hz 的帧。  
2）对数能量谱：每一帧都乘以汉宁窗，然后用离散傅里叶变换计算对数幅值谱 $L_P$。假设幅值谱用$|{X(k,n)}|$表示，那么

$$
L_P (k,n) = \ln{(1+1000.0 \cdot |X(k,n)|)}
$$

3）谱通量：对对数幅值谱所有具有正向差分的频率进行求和，得到谱通量。设 $N$ 是傅里叶变换得到离散频谱中正频率点的数量，这里 $N = 513$，计算中忽略离散频谱的直流分量（FFT 第 0 点）。$I_{PF}(k,n)$ 是一个指示函数，对频率谱对数幅度中的正差分进行选择。

$$
\mathrm{Flux}(n) = \sum_{k=1}^{N-1}(L_P(k,n)-L_P(k,n-1)) \cdot  I_{PF}(k,n)
$$

4）低通滤波：用 14 阶 FIR 滤波器对信号进行低通滤波，截止频率为 7 Hz ，汉明窗方法。

### 节拍周期检测

1）分帧：取 2048 个 $\mathrm{Flux}$ 帧  （ 2048 / 344.5 ≈ 6 s）进行分析，步长 128 点（128 / 344.5 ≈  0.37 s）。假设总共会产生 M 个这种分析帧，采样率为 1 / 0.37 ≈ 2.7 Hz。

2）自相关：采用“广义自相关”，对于第 m 个分析帧，首先补零使帧长度加倍，然后计算其离散傅里叶变换，取压缩后的频谱幅值，然后进行离散傅里叶反变换:

$$
A_m(t) = \text{IDFT}(|{\text{DFT}(\mathrm{Flux}(m))}|^c)
$$

其中 $c$ 控制了幅度的压缩度，$ c = 0.5 $ 能较好平衡自相关分辨率和对噪声的敏感度。


3）增强谐波：增强自相关 $EAC$ 由 $A_m$ 和其自身的 2 倍和 4 倍拉伸相加构成：

$$
{EAC}_m(t) = A_m(t) + A_m(2\cdot t) + A_m(4 \cdot t)
$$

4）峰值提取：提取 $EAC$ 中最高的 10 个峰，舍弃 50 ~ 210 BPM 之外的候选速度，即保留滞后在 98 ~ 414 点之间峰的集合 $\{P\}$。


5）评估脉冲序列：给定一个候选自相关滞后 $P$ 和候选相位 $\phi$ (节拍出现的时刻)，用参数 $v = 1,\ 1.5,\ 2$ 来生成三个脉冲序列：

$$
I_{P,\phi,v} = \phi+vBP \quad B = 0,1,2,3
$$

三个序列按照 1.0 : 0.5 : 0.5 加权求和，变成单一的脉冲序列 $I_{P,\phi}$，索引超过 $\mathrm{Flux}$ 帧长度的脉冲忽略，然后与 $\mathrm{Flux}$ 做互相关运算

考虑所有可能的相位 $\phi$，定义

$$
\rho_P(\phi,m) = \mathrm{Flux}(m) \star I_{P,\phi} \quad \phi = 0,1,...(P-1)
$$

然后计算两个评分函数：关于相位的自相关函数中最大的值 $SC_x$ ，和它们的方差 $SC_v$

$$
SC_v(P,m) = \mathrm{var}_{\phi}(\rho_P(\phi,m))\\
SC_x(P,m) = \mathrm{max}_{\phi}(\rho_P(\phi,m))
$$

这两个评分归一化后相加得到第 m 帧的候选速度 P 的最终得分：

$$
SC(P,m) = \frac{SC_x(P,m)}{\sum_c SC_x(P,m)} + \frac{SC_v(P,m)}{\sum_c SC_v(P,m)}
$$

 最高评分的候选速度作为第 m 帧的速度
 
$$
L_{\text{local}}(m) = \arg \max SC(P,m)
$$

高斯化累加：根据第 m 帧局部主导速度生成一个高斯曲线 $G(m)$，对于不同的 $L_{\text{local}}(m)$，使用固定的 $\sigma=10$ 会在 BPM 域中产生不同的的速度估计范围，速度越快，对应的范围越宽。

$$
G(m) = \frac{1}{\sigma\sqrt{2\pi}} \exp\{-\frac{[x-L_{\mathrm{local}}(m)]^2}{2\sigma^2}\}
$$

每一帧产生的 $G(m)$ 都被累加到一个累加器 $C$ 中，$C$ 是足够容纳所有可能自相关滞后的向量。对历史数据使用指数衰减予以削弱，通过系数 $\alpha$ 来调节历史数据的衰减速度和新数据的累加速度，$\alpha$ 取 0.97。

$$
C(m) = \alpha \cdot C(m-1) + (1-\alpha) \cdot G(m)
$$

峰值提取：从 $C$ 中选取最高的峰，得到当前时刻最佳估计速度

$$
L(m) = \arg \max C(m)
$$

$$
\Delta_{I} =\frac{L(m)}{F_s},  \quad \text{BPM} = \frac{60\cdot F_s}{L(m)},\quad F_s ≈ 344.5 \mathrm{Hz}
$$


## 动态规划
> Ellis, Daniel PW. "Beat tracking by dynamic programming." Journal of New Music Research 36.1 (2007): 51-60.

目标函数定义为节拍点检测函数的值O求和，加节拍间距定义的惩罚项F

$$
C = \sum_{i=1}^{N} O(t_i) + \alpha \sum_{i=2}^{N}F(t_i - t_{i-1},\tau_p)
$$

$$
F(\Delta t,\tau) = -\left( \log \frac{\Delta t}{\tau} \right)^2
$$

求极值点得到推断节拍序列 $\{t_i\} = \arg\max C $。
使用动态规划法，将目标函数分解为递推式：

$$
C^*(t) = O(t) + \max_{\tau=0\dots t} \{\alpha F(t - \tau,\tau_p)+C^*(t)\}
$$

实际只搜索 $\tau=t-2\tau_p\dots t-\tau_p/2$，记录对应的极值点

$$
P^*(t) = \arg \max \left\{   \alpha F(t-\tau,\tau_p) + C^*(\tau)  \right\}
$$

找到最后一个极值点$t_N$之后，使用 $t_{N-1} = P^*(t_N)$ 回溯所有极值点。


## 隐马尔可夫模型
> A. Klapuri, A. Eronen, and J. Astola. Analysis of the meter of acoustic musical signals. IEEE Transactions on Audio, Speech, and Language Processing, 14(1):342–355, January 2006.

> N. Degara, E. Argones-R ́ ua, A. Pena, S. Torres-Guijarro, M. E. P. Davies, and M. D. Plumbley. Reliability-informed beat tracking of musical signals. IEEE Transactions on Audio, Speech and Language Processing, 20(1):290–301, January 2012.

> G. Peeters and H. Papadopoulos. Simultaneous beat and downbeat-tracking using a probabilistic framework: Theory and large-scale evaluation. IEEE Transactions on Audio, Speech, and Language Processing, 19(6):1754–1769, 2011. 

## 粒子滤波

> S. Hainsworth and M. Macleod. Particle filtering applied to musical tempo tracking. EURASIP J. Appl. Signal Process., 15:2385–2395, January 2004. 

> Heydari, Mojtaba & Cwitkowitz, Frank & Duan, Zhiyao. (2021). BeatNet: CRNN and Particle Filtering for Online Joint Beat Downbeat and Meter Tracking. 


## 动态贝叶斯网络



> F. Krebs, S. B ̈ ock, and G. Widmer. Rhythmic pattern modeling for beat and downbeat tracking in musical audio. In Proceedings of the 14th International Society for Music Information Retrieval Conference (ISMIR 2013), pages 227–232, Curitiba, Brazil, November 2013. 

<!-- 然后，大多数系统从这些周期性中确定最主要的节奏，然后使用多种代理方法 [8,12]、动态规划 [6,10]、隐马尔可夫模型 (HMM) [7,16,18] 或循环神经网络（RNN）[2]。其他系统直接对输入特征进行操作，并使用动态贝叶斯网络（DBN）共同确定节拍的节奏和相位[3,14,17,21]。

[1] MIREX 2013 beat tracking results. http://nema.lis. illinois.edu/nema_out/mirex2013/results/ abt/, 2013. 
[2] S. B ̈ ock and M. Schedl. Enhanced Beat Tracking with Context-Aware Neural Networks. In Proceedings of the 14th International Conference on Digital Audio Effects (DAFx11), pages 135–139, Paris, France, September 2011. 
[3] A. T. Cemgil, H. Kappen, P. Desain, and H. Honing. On tempo tracking: Tempogram Representation and Kalman filtering. Journal of New Music Research, 28:4:259–273, 2001.
[4] N. Collins. Towards a style-specific basis for computational beat tracking. In Proceedings of the 9th International Conference on Music Perception and Cognition (ICMPC9), pages 461–467, Bologna, Italy, 2006. 
[5] M. E. P. Davies, N. Degara, and M. D. Plumbley. Evaluation methods for musical audio beat tracking algorithms. Technical Report C4DM-TR-09-06, Centre for Digital Music, Queen Mary University of London, 2009. 
[6] M. E. P. Davies and M. D. Plumbley. Context-dependent beat tracking of musical audio. IEEE Transactions on Audio, Speech, and Language Processing, 15(3):1009–1020, March 2007. 
[7] N. Degara, E. Argones-R ́ ua, A. Pena, S. Torres-Guijarro, M. E. P. Davies, and M. D. Plumbley. Reliability-informed beat tracking of musical signals. IEEE Transactions on Audio, Speech and Language Processing, 20(1):290–301, January 2012.
[8] S. Dixon. Automatic extraction of tempo and beat from expressive performances. Journal of New Music Research, 30:39–58, 2001. 
[9] D. Eck. Beat tracking using an autocorrelation phase matrix. In Proceedings of the IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP 2007), volume 4, pages 1313–1316, Honolulu, Hawaii, USA, April 2007. 
[10] D. P. W. Ellis. Beat tracking by dynamic programming. Journal of New Music Research, 2007:51–60, 2007. 
[11] A. Gkiokas, V. Katsouros, G. Carayannis, and T. Stafylakis. Music tempo estimation and beat tracking by applying source separation and metrical relations. In Proceedings of the 37th International Conference on Acoustics, Speech and Signal Processing (ICASSP 2012), pages 421–424, Kyoto, Japan, March 2012.
[12] M. Goto and Y. Muraoka. Beat tracking based on multipleagent architecture a real-time beat tracking system for audio signals. In Proceedings of the International Conference on Multiagent Systems, pages 103–110, 1996. 
[13] F. Gouyon, A. Klapuri, S. Dixon, M. Alonso, G. Tzanetakis, C. Uhle, and P. Cano. An experimental comparison of audio tempo induction algorithms. IEEE Transactions on Audio, Speech, and Language Processing, 14(5):1832–1844, September 2006. 
[14] S. Hainsworth and M. Macleod. Particle filtering applied to musical tempo tracking. EURASIP J. Appl. Signal Process., 15:2385–2395, January 2004. 
[15] A. Holzapfel, M. E. P. Davies, J. R. Zapata, J. Oliveira, and F. Gouyon. Selective sampling for beat tracking evaluation. IEEE Transactions on Audio, Speech, and Language Processing, 20(9):2539–2548, November 2012. 
*[16] A. Klapuri, A. Eronen, and J. Astola. Analysis of the meter of acoustic musical signals. IEEE Transactions on Audio, Speech, and Language Processing, 14(1):342–355, January 2006. 
*[17] F. Krebs, S. B ̈ ock, and G. Widmer. Rhythmic pattern modeling for beat and downbeat tracking in musical audio. In Proceedings of the 14th International Society for Music Information Retrieval Conference (ISMIR 2013), pages 227–232, Curitiba, Brazil, November 2013. 
[18] G. Peeters and H. Papadopoulos. Simultaneous beat and downbeat-tracking using a probabilistic framework: Theory and large-scale evaluation. IEEE Transactions on Audio, Speech, and Language Processing, 19(6):1754–1769, 2011. 
[19] L. Rabiner. A tutorial on hidden Markov models and selected applications in speech recognition. In Proceedings of the IEEE, pages 257–286, 1989.
[20] E. D. Scheirer. Tempo and beat analysis of acoustic musical signals. The Journal of the Acoustical Society of America, 103(1):588–601, 1998. 
[21] N. Whiteley, A. Cemgil, and S. Godsill. Bayesian modelling of temporal structure in musical audio. In Proceedings of the 7th International Conference on Music Information Retrieval (ISMIR 2006), pages 29–34, Victoria, BC, Canada, October 2006. 
[22] J. R. Zapata, M. E. P. Davies, and E. G ́ omez. Multi-feature beat tracking. IEEE/ACM Transactions on Audio, Speech, and Language Processing, 22(4):816–825, April 2014. -->