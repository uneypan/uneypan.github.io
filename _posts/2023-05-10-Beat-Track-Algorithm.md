---
layout: article
title: 节拍跟踪相关算法
mathjax: true
mathjax_autoNumber: true
---

![](https://drive.google.com/uc?export=view&id=1_NR85DqE2QSpe2_9sBemMXGE1dVDKWGj)

## 节拍跟踪器的动力学方程描述

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

## 卡尔曼滤波

1.预测步：先验状态估计、先验估计协方差、先验观测

$$
\hat{\mathrm{x}}(k) = \mathrm{F}{\mathrm{x}}(k-1)\\
$$

$$
\hat{\mathrm{P}}(k) = \mathrm{F}\mathrm{P}(k-1)\mathrm{F}^{\mathrm{T}}+\mathrm{Q}
$$


$$
\hat{\mathrm{y}}(k) = \mathrm{H}\hat{\mathrm{x}}(k-1)
$$

2.更新步：计算卡尔曼增益、后验状态估计及对应协方差

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

## 流式速度估计
> Percival, Graham, and George Tzanetakis. "Streamlined tempo estimation based on autocorrelation and cross-correlation with pulses." IEEE/ACM Transactions on Audio, Speech, and Language Processing 22.12 (2014): 1765-1776.

Percival 等人提出一种针对音频流的实时速度估计算法，算法利用速度谱图（Tempogram）和累加器分步估计速度，用“广义自相关”和快速傅里叶变换加快自相关运算，在计算量和精度上取得不错的平衡。

**1.起始点强度信号**

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

**2.节拍周期检测**

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
