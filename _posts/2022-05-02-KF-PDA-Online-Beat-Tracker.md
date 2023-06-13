---
layout: article
title: 基于概率数据关联滤波器的在线节拍跟踪器
mathjax: true
---

<!-- # **基于概率数据关联滤波器的在线节拍跟踪器** -->

本文叙述了一种基于概率数据关联滤波器的在线节拍跟踪算法，能够将实时地将输入的音频波形数据转化为节拍序列。

## 1. 算法框架

算法的基本流程如下。首先需要对波形数据进行预处理，提取音符起始点能量包络，作为概率数据关联滤波器的输入。在使用滤波器进行节拍跟踪之前，还需要采集一小段音乐“前奏”对滤波器的状态进行初始化设定，包括全局速度估计和确定第一拍位置。初始化滤波器之后，只需将起始点能量包络输入滤波器，即可迭代得到预测拍和融合拍。

## 2. 音频预处理

### 2.1  频谱通量（Spectral Flux）

分帧：将源音频（44.1 kHz）按每帧 1024 个采样点、步长128 点分割，产生一系列采样率约为 344.5 Hz 的帧

对数能量谱：每一帧都乘以汉宁窗，然后用离散傅里叶变换计算对数幅值谱 $L_P$。假设幅值谱用 $|{X(k,n)}|$ 表示，那么对数能量谱就是

$$
L_P (k,n) = \ln{(1+1000.0 \cdot |{X(k,n)}|)};
$$

频谱通量：对对数幅值谱所有具有正向差分的频率进行求和，得到谱通量。设 $N$ 是傅里叶变换得到离散频谱中正频率点的数量，这里 $N = 513$，计算中忽略离散频谱的直流分量（FFT 第 0 点）。$I_{PF}(k,n)$ 是一个指示函数，对频率谱对数幅度中的正差分进行选择。

$$
F_{lux}(n) = \sum_{k=1}^{N-1}(L_P(k,n)-L_P(k,n-1)) \cdot  I_{PF}(k,n)
$$

低通滤波：用 14 阶 FIR 滤波器对信号进行低通滤波，截止频率为 7 Hz ，汉明窗法。

### 2.2  起始点强度信号（OSS）

理想情况下一个音符的时间域信息描述：起始点（Onset）、 上升（Attack）、过渡（Transient）和下降（Decay）。

音符起始点（Onset）是表示音符开始瞬间的标记，属于节拍的底层结构。一个节拍跟踪器要实现的目标在于，拟合检测到的起始点的同时产生一个有规律的节拍序列。这里主要利用梅尔频率倒谱系数（MFCC）的差分来检测音符起始点。MFCC 能够很好地反映音乐中的击打事件和和声变化，具体来说，第 0 阶系数能准确表示梅尔标度能量，而 1～3 阶系数能很好地捕捉到和声的能量变化。具体步骤如下

MFCC：使用 26 通道的三角梅尔滤波器组将幅值谱 $|{X(k,n)}|$ 映射到的梅尔尺度，同样取对数得到对数功率谱，然后离散余弦变换（DCT）得到 MFCC ；

对 0～4 阶 MFCC 对时间的正向差分进行求和得到起始点能量包络；FIR 滤波器进行低通滤波，然后进行半波整流，只保留能量正向跳变；

低通滤波：用 14 阶 FIR 滤波器对信号进行低通滤波，截止频率为 7 Hz ，汉明窗法。

门限过滤：使用宽度为30的滑动均值滤波器做过滤门限，得到起始点强度信号。

峰值提取：所有局部最大的峰值点即为检测到的起始点。

## 3. 初始化滤波器  

PDAF 需要设置初始节拍周期和第一拍位置，正确的初始化对于 PDAF 进入准确跟踪轨道非常重要。在对节拍进行实时跟踪前，截取一小段音乐（5～30s），利用自相关运算估计歌曲的速度，再选取检测函数中最大值对应的时间点作为第一拍。 获得的速度和节拍用于初始化 PDAF 。

### 3.1 全局速度估计

最简单的速度估计是采用自相关法找到节拍周期的最大值，但是这种方法在速度的倍半问题上很容易混淆。更好的方法是知道节奏在时域上的分布，选取其中速度较为稳定的部分作为来估计速度。这里采用了速度图（Tempogram）法。

计算速度图的方法与获取时频谱的短时傅里叶变换很类似。已知起始点检测函数，先进行分帧，窗宽 9s，步长 200 ms 。然后，对于每一帧，生成一系列周期不同的 hamming 三角函数核与之做相关运算，得到这一帧的“速度谱”，所有的帧组合成离散时域上的“速度图”。可见，上述过程也可以用短时傅里叶变换实现。


得到速度图后，首先需要按照人类的偏好曲线对速度谱进行加权，以削弱周期倍半频的影响。加权函数 $W$ 是对数时间域上的正态分布函数，其中 $\tau_0$ 是偏好节拍周期，$\sigma_\tau$ 控制加权函数的宽度。
$$
W(\tau) = \exp\{-\frac{1}{2}(\frac{ \log_2{\tau/\tau_0}}{\sigma_\tau} ) ^2  \}
$$

将每一帧速度图中最大值对应的速度连起来，得到主局部速度曲线（Predominant Local Tempo Curve，PLTC）。然后，把 PLTC 变化较快（差分大）的点打断，这样就得到速度相对平稳的几个阶段（例如下图的蓝绿两线）。最后找到持续时间最长的一个阶段，将这一阶段中 PLTC 在速度图中数值最大的点（绿点）对应的速度作为全局估计速度。


### 3.2 确定第一拍

因为速度图上包含了每一点的完整信息（时间点、周期、相位），理论上可以通过上一节选取的速度点重建出所有的节拍。但是由于速度图的分辨率很低（200 ms），此方法存在较大误差。所以简单起见，直接选择平稳阶段中起始点检测函数最大值的点作为滤波器跟踪的第一拍，这是因为能量最强的起始点有很大概率是一个节拍。

## 4. 概率数据关联滤波器

在雷达跟踪过程中，当目标处于非机动状态时，采用概率数据关联滤波器（Probabilistic Data Association Filter, PDAF）可以对目标很好的进行跟踪。节拍进行的模型与一个非机动飞行器的运动模型十分类似，都可以描述为一个线性动态模型。由卡尔曼滤波器（Kalman Filter, KF）的预测公式可以取得一个先验观测，取得先验观测的某个邻域（称为“**关联门**”）内，每个测量值正确的概率——称为“**概率数据关联**”（Probabilistic Data Association, PDA），与之相对应的滤波器称“**概率数据关联滤波器**”。 PDAF 是卡尔曼滤波器的改进版本，在对目标进行观测时使用 PDA 技术，考虑所有可能来源于目标观测值，并对应修改了 KF 更新公式。

PDAF 的典型应用是跟踪观测到的节拍，并在一定的限度内修正速度和位置误差。例如，使用速度图法估计 MIREX 2006 中 train12.wav 的速度偏低，但跟踪 5 s 后速度已被修正。最后的评估实验表明，PDAF 与卡尔曼滤波器 - 局部最大值法（KF-LM）相比，准确率更高。

### 4.1 问题描述

节拍跟踪器的动力学特性使用以下线性状态传递方程描述

$$
\begin{aligned}
&\mathbf{x}(k)=\mathbf{F}(k-1) \mathbf{x}(k-1)+\mathbf{v}(k-1),\quad k=1,2, \ldots \\ 
&\mathbf{z}(k)=\mathbf{H}(k) \mathbf{x}(k)+\mathbf{w}(k),\quad k=1,2,...
\end{aligned}
$$

其中 $\mathbf{x}(k) = \begin{bmatrix} \hat{\tau}(k) \\ \hat{\Delta}(k) \end{bmatrix}$ 是 $2$ 维状态向量，$\hat{\tau}(k)$ 是节拍时间，$\hat{\Delta}(k)$ 是节拍周期；

$\mathbf{z}(k)$ 是观测到的节拍位置；$\mathbf{F}(k) = \begin{bmatrix} 1&1\\0&1\end{bmatrix}$是传递矩阵，$\mathbf{H}(k) = \begin{bmatrix} 1&0\end{bmatrix}$ 是观测矩阵；

过程噪声 $\mathbf{w}(k) \sim \mathbf{N}(0,\mathbf{Q}(k))$ ，测量噪声 $\mathbf{y}(k) \sim \mathbf{N}(0,\mathbf{R}(k))$  均服从正态分布。

### 4.2 概率数据关联滤波算法

**预测步骤**

PDAF 的预测步与 KF 相同，即

先验状态估计、先验估计协方差

$$
\hat{\mathbf{x}}(k\mid k-1) = \mathbf{F}(k-1)\hat{\mathbf{x}}(k-1\mid k-1)\\
\\
\mathbf{P}(k\mid k-1) = \mathbf{F}(k-1)\mathbf{P}(k-1\mid k-1)\mathbf{F}(k-1)'+\mathbf{Q}(k-1)
$$

先验观测、预测残差协方差

$$
\begin{aligned}
&\hat{\mathbf{z}}(k\mid k-1) = \mathbf{H}(k)\hat{\mathbf{x}}(k-1\mid k-1)\\
&\mathbf{S}(k)= \mathbf{H}(k)\mathbf{P}(k\mid k-1)\mathbf{H}(k)'+\mathbf{R}(k)
\end{aligned}
$$

**观测验证**

使用椭圆关联门对观测值进行验证
$$
\mathcal{V}(k,\gamma) = \{ \mathbf{z}:[\mathbf{z}-\hat{\mathbf{z}}(k\mid k-1)]'\mathbf{S}(k)^{-1}[\mathbf{z}-\hat{\mathbf{z}}(k \mid k-1)] \le \gamma\}
$$

化简得到: 
$$\quad \hat{\mathbf{z}}(k\mid k-1)-\gamma(p_{11}+\sigma_v^2) \le \mathbf{z} \le \hat{\mathbf{z}}(k\mid k-1)+\gamma(p_{11}+\sigma_{\mathbf{w}}^2)
$$

其中 $p_{11}$ 是节拍位置 $\tau(k)$的方差， $\sigma_{\mathbf{w}}^2$ 是观测噪声 $\mathbf{w}(k)$ 的方差。$\gamma = 9$ 时关联门内包含正确观测的概率 $P_{G}$ 是 $99.7$%， $\gamma = 4$ 的概率是 $95.4$%.

对于一维观测值（节拍位置），关联门的体积是

$$
V(k) = 2\gamma^{\frac{1}{2}}|\mathbf{S}(k)|^{\frac{1}{2}}
$$

落入关联门的观测值称“**有效观测**”，表示为

$$
\mathbf{z}(k) = \{ z_i(k) \}^{m(k)}_{i=1}
$$

其中 $m(k)$ 是有效观测的数量。

下面的示意图展示了由先验观测所得到的椭圆关联门，且关联门内存在 3 个有效观测的情况。

**数据关联**

如果有效观测的数量 $m(k)>1$ ，则需要进行数据关联。已知 $m(k)$ 个观测拍的位置和起始点强度，根据全概率公式，后验状态估计可以写成
$$
\begin{aligned}
\hat{\mathbf{x}}(k\mid k) =& E[\mathbf{x}(k)\mid \mathbf{I}_{\mathbf{z}}(k),\mathbf{Z}^k] \\ 
=& \sum_{i=0}^{m(k)} {E[\mathbf{x}(k)\mid \theta_i(k), \mathbf{I}_{\mathbf{z}},\mathbf{Z}^k]} 
P[\theta_i(k)\mid \mathbf{I}_{\mathbf{z}},\mathbf{Z}^k]\\
=& \sum_{i=0}^{m(k)} \hat{\mathbf{x}}_i(k\mid k )\beta_i(k)
\end{aligned}
$$

其中，$\mathbf{I}_{\mathbf{z}}(k)$ 是有效观测集 $\mathbf{Z}(k)$ 的起始点强度，$\mathbf{Z}^k = \{\mathbf{Z}(j),j=1,2,...,k\}$ 表示累积历史观测，$\theta_i(k)$ 表示事件第 $i$ 个有效观测 $z_i(k)$ 来源于目标。以观测拍的位置和强度为条件，其为正确观测的概率由残差和起始点强度两部分组成。由贝叶斯公式，最终的概率权重是两部分的乘积
$$
\begin{aligned}
\beta_i(k) &\triangleq P[\theta_i(k)\mid \mathbf{I}_{\mathbf{z}},\mathbf{Z}^k]\\
&=P[\mathbf{I}_{\mathbf{z}} \mid \theta_i(k),\mathbf{Z}^k]P[\theta_i(k)\mid \mathbf{Z}^k]\\
&=\beta_{\nu,i}(k) \cdot \beta_{I,i}(k)
\end{aligned}
$$

对于带有均匀杂波空间的 PDA 模型，由残差取得的第 $i$ 个有效观测 $z_i(k)$ 是正确观测的概率是
$$
\begin{aligned}
&\beta_{\nu,i}(k)\triangleq P[\theta_i(k)\mid \mathbf{Z}^k]=
\begin{cases}
\frac{\mathcal{L}_{i}(k)}{1-P_{D} P_{G}+\sum_{j=1}^{m(k)} \mathcal{L}_{j}(k)}, & i=1, \ldots, m(k) \\ 
\\
\frac{1-P_{D} P_{G}}{1-P_{D} P_{G}+\sum_{j=1}^{m(k)} \mathcal{L}_{j}(k)}, & i=0
\end{cases}\\
\\
其中, \quad &\mathcal{L}_{i}(k) \triangleq \frac{\mathcal{N}\left[z_{i}(k) ; \hat{z}(k \mid k-1), S(k)\right] P_{D}V(k)}{m(k)}
\end{aligned}
$$

其中， $i=0$ 表示所有有效观测是错误的；$P_{D}$ 是正确节拍被探测到的概率；$P_{G}$ 是正确观测落入关联门的概率，由 $\gamma$ 确定； $\mathcal{L}_{i}(k) $ 是观测值 $z_i(k)$ 来源于目标的似然函数，$V(k)$ 是之前提到的关联门的体积。

实践发现，由于残差协方差 $S(k)$ 往往很小，由残差取得的概率权重 $\beta_{\nu,i},i=1,...,m(k)$ 也很小，使得更新后的协方差非常容易发散。

简便起见，实践中对于 PDA 的假设作出一些简化。首先认为正确节拍被必然探测到，即 $P_D = 0$。然后假设没有错误观测，只使用观测的似然函数作为概率权重，并且由关联门的定义可得
$$
\begin{aligned}
&\beta_{\nu,i}(k)=
\begin{cases}
P_G\frac{\mathcal{L}_{i}(k)}{\sum_{j=1}^{m(k)} \mathcal{L}_{j}(k)}, & i=1, \ldots, m(k) \\ 
\\
1-P_{G}, & i=0
\end{cases}\\
\\
其中, \quad &\mathcal{L}_{i}(k) \triangleq \mathcal{N}\left[z_{i}(k) ; \hat{z}(k \mid k-1), S(k)\right]
\end{aligned}
$$
由于观测拍的正确概率关于强度分布的不易取得，但是通过统计可以发现节拍的起始点通常比非节拍的起始点有更高的强度分布。简单按照能量强度线性地分配概率权重。于是，由起始点强度取得的第 $i$ 个有效观测的概率可以写成
$$
\beta_{I,i} (k)\triangleq P[\mathbf{I}_{\mathbf{z}} \mid \theta_i(k),\mathbf{Z}^k]= \frac{O(z_i(k))}{\sum_{i=1}^{m(k)}{O(z_i(k))}}, 	\quad i=1, \ldots, m(k)
$$
在概率数据关联的公式中，最终的权重是两部分概率的相乘，这导致我们无法调节残差与强度对于跟踪的重要性的贡献比例。于是，这里引入一个加权系数 $\alpha$ 对两项的比例进行加和，实践发现强度对于跟踪的影响较大。
$$
\beta_i(k) = \alpha\beta_{\nu,i}(k) + (1-\alpha)\beta_{I,i}(k)
$$

**更新步骤**

先计算卡尔曼增益
$$
\mathbf{W}(k)= \mathbf{P}(k|k-1)\mathbf{H}(k)'\mathbf{S}(k)^{-1}
$$

接下来分两种情况：

**a)**  如果有效观测的数量 $m(k)=1$，则更新步与一般卡尔曼滤波相同，即

残差、后验状态估计及对应协方差
$$
\begin{aligned}
&\nu(k) = {z}(k)-\hat{{z}}({k\mid k-1})\\
&\hat{\mathbf{x}}(k\mid k)=\hat{\mathbf{x}}(k\mid k-1) + \mathbf{W}(k)\nu(k)\\
&\mathbf{P}(k\mid k) =\left[\mathbf{I}-\mathbf{W}(k)\mathbf{H}(k)\right]\mathbf{P}(k\mid k-1)\\
\end{aligned}
$$

**b)**  如果有效观测的数量 $m(k)\ge 1$，则需对更新公式进行修改，先计算每一个观测的残差，然后使用概率数据关联得到的概率权重进行加权，得到

加权残差
$$
\begin{aligned}
&{\nu}_{i}(k) \triangleq {z}_{i}(k)-\hat{{z}}({k\mid k-1})\\
&\nu(k)=\sum_{i=1}^{m(k)} \beta_{i}(k) \nu_{i}(k)
\end{aligned}
$$

修改后的后验状态估计及对应协方差为
$$
\begin{aligned}
&\hat{\mathbf{x}}(k\mid k)=\hat{\mathbf{x}}(k\mid k-1) + \mathbf{W}(k)\nu(k)\\
&\mathbf{P}(k \mid k)=\beta_{0}(k) \mathbf{P}(k \mid k-1)+\left[1-\beta_{0}(k)\right] \mathbf{P}^{c}(k \mid k)+\widetilde{\mathbf{P}}(k)\\
其中, \quad &\mathbf{P}^{c}(k \mid k)=\mathbf{P}(k \mid k-1)-\mathbf{W}(k) \mathbf{S}(k) \mathbf{W}(k)^{\prime}\\
&\widetilde{\mathbf{P}}(k) \triangleq \mathbf{W}(k)\left[\sum_{i=1}^{m(k)} \beta_{i}(k) \nu_{i}(k) \nu_{i}(k)^{\prime}-\nu(k) \nu(k)^{\prime}\right] \mathbf{W}(k)^{\prime}
\end{aligned}
$$


### 4.3 PDAF 流程图



## 5. 实验与讨论

采用 MIREX 2006 数据集，包含 20 个 30 秒的片段，每个片段由 40 名受试者进行标记节拍，但是歌曲的速度没有告知受试者，所以有的人速度较快，有的人速度慢。音乐片段的前 10 秒用于初始化滤波器，后 15 秒用于评估节拍精度。

对于每一个片段，选取标注速度和待测节拍相差不超过 20% 的标注节拍序列计算精度，这里采用 P-Score 指标来进行定量评估
$$
P=\frac{1}{N_{\max }} \sum_{n=-\infty}^{\infty} \sum_{m=-W}^{W} \delta_{d}(n) \delta_{g}(n-m)
$$
其中 $\delta_d$ 和 $\delta_g$ 是待测脉冲序列和标注脉冲序列，$2W$是 20% 的平均节拍间隔， $N_{\max} = \max{(N_d,N_g)}$ 是待测节拍个数与真值个数的较大者。

PDAF 评估的结果 P-Score = 0.71017，好于相同条件下 KF-LM（卡尔曼滤波器 - 局部最大规则，P-Score = 0.63308）附录表 1 展示了每首歌具体的评估结果。

通过观察和分析，发现目前算法存在的问题全局速度估计不完全准确，导致滤波器没有被正确初始化。为了模拟滤波器在初始化较为“正确”的情况下的表现，实验使用了附录 2 中人工标注的速度和节拍数据。可以发现，在滤波器参数不变的情况下，“正确的”初始化使得 P-Score 从 0.71017 上升至 0.7753。三种算法的对比见下表。

| 精度 \ 方法 | KF-LM   | PDAF    | HUMAN  |
| ----------- | ------- | ------- | ------ |
| P-Score     | 0.63308 | 0.71017 | 0.7753 |

可见，想要进一步提高精度，首要的策略是改进全局速度估计和节拍感知的算法，这里可以采用非实时的算法，仍有较大改进的空间。另外，PDAF 并不适用于机动目标的跟踪，即当节奏快速变化时，PDAF 无法快速反应。对于长时间跟踪和高噪声的环境，PDAF 有一定几率丢失跟踪轨迹。解决的方法 类似于航迹跟踪问题中的机动检测，我们需要监测节奏的变化，及时重新初始化滤波器，以提高实时应用场合下的容错率。

另外关于此算法的计算量，笔者使用一台 CPU 为 Intel i7-8550U 的 PC 在 Matlab 平台完成了算法验证， 计算长度为 3 min 的音频（数据已经读入内存）平均花费时长是 0.336 s，可见此算法对于资源的使用很少。 

## 主要参考文献

[1]Shiu Y, Cho N, Chang P-C, et al. Robust on-line beat tracking with kalman filtering and probabilistic data association (KF-PDA)[J]. IEEE Transactions on Consumer Electronics, 2008, 54(3): 1369-1377.

[2]Bello J P, Daudet L, Abdallah S, et al. A tutorial on onset detection in music signals[J]. IEEE Transactions on Speech and Audio Processing, 2005, 13(5): 1035-1047.

[3]Bar-Shalom Y, Tse E. Tracking in a cluttered environment with probabilistic data association[J]. Automatica, 1975, 11(5): 451-460.

[4]杨万海. 多传感器数据融合及其应用[M]. 西安: 西安电子科技大学出版社, 2004.

[5]李伟. 音频音乐与计算机的交融——音频音乐技术[M]. 上海: 复旦大学出版社, 2019.

[6]Ellis D P W. Beat Tracking by Dynamic Programming[J]. Journal of New Music Research, 2007, 36(1): 51-60.

[7]Bar-Shalom Y, Daum F, Huang J. The probabilistic data association filter[J]. IEEE Control Systems Magazine, 2009, 29(6): 82-100.

[8]Grosche P, Muller M. Extracting Predominant Local Pulse Information From Music Recordings[J]. IEEE Transactions on Audio, Speech, and Language Processing, 2011, 19(6): 1688-1701.

## 附录 1

**KF-LM、PDAF 和 使用了人类标注数据的 PDAF 在 MIREX 2006 数据集上的评估结果（原始）**

```matlab
KF-LM
train1.wav   p-score 0.97781  tempo 129.9805  matches 35
train2.wav   p-score 0.35     tempo 155.64    matches 10
train3.wav   p-score 1        tempo 101.3756  matches 1
train4.wav   p-score 0.93125  tempo 127.5898  matches 10
train5.wav   p-score 0.83529  tempo 205.8528  matches 5
train6.wav   p-score 0.92683  tempo 163.5198  matches 2
train7.wav   p-score 0.27116  tempo 108.2599  matches 5
train8.wav   p-score 0.94387  tempo 147.7161  matches 26
train9.wav   p-score 0        tempo 218.5985  matches 0
train10.wav  p-score 0        tempo 158.2863  matches 0
train11.wav  p-score 0.92088  tempo 140.4982  matches 26
train12.wav  p-score 0.53571  tempo 112.2759  matches 1
train13.wav  p-score 0.48024  tempo 90.0176   matches 8
train14.wav  p-score 0.35073  tempo 111.1515  matches 29
train15.wav  p-score 0.89886  tempo 185.8991  matches 9
train16.wav  p-score 0.93333  tempo 180.316   matches 2
train17.wav  p-score 0        tempo 140.4428  matches 0
train18.wav  p-score 0.65297  tempo 120.3681  matches 4
train19.wav  p-score 0.86766  tempo 186.4698  matches 34
train20.wav  p-score 0.78491  tempo 220.4387  matches 38
P-Score： 0.63308


PDAF
train1.wav   p-score 0.98055  tempo 129.3466  matches 35
train2.wav   p-score 0.72619  tempo 166.0619  matches 10
train3.wav   p-score 1  		  tempo 102.1052  matches 1
train4.wav   p-score 0.93437  tempo 126.3503  matches 10
train5.wav   p-score 0.91765  tempo 205.4672  matches 5
train6.wav   p-score 0.95122  tempo 163.9734  matches 2
train7.wav   p-score 0.33466  tempo 110.6195  matches 5
train8.wav   p-score 0.95634  tempo 148.1603  matches 26
train9.wav   p-score 0  		  tempo 190.4287  matches 0
train10.wav  p-score 0  		  tempo 153.9373  matches 0
train11.wav  p-score 0.94396  tempo 140.3007  matches 26
train12.wav  p-score 0.57143  tempo 113.3152  matches 1
train13.wav  p-score 0.95109  tempo 90.3837   matches 8
train14.wav  p-score 0.7367   tempo 128.5755  matches 29
train15.wav  p-score 0.92774  tempo 185.3825  matches 9
train16.wav  p-score 0.97778  tempo 179.3021  matches 2
train17.wav  p-score 0  		  tempo 141.0535  matches 0
train18.wav  p-score 0.62046  tempo 120.9815  matches 4
train19.wav  p-score 0.86767  tempo 185.5078  matches 34
train20.wav  p-score 0.8056   tempo 220.0697  matches 38
P-Score： 0.71017

HUMAN
train1.wav   p-score 0.97698  tempo 129.8549  matches 35
train2.wav   p-score 0.95     tempo 168.2446  matches 10
train3.wav   p-score 0.15486  tempo 152.8583  matches 26
train4.wav   p-score 0.93437  tempo 126.631   matches 10
train5.wav   p-score 0.83529  tempo 206.3765  matches 5
train6.wav   p-score 0.8019   tempo 82.0652   matches 25
train7.wav   p-score 0.48889  tempo 110.3096  matches 5
train8.wav   p-score 0.9553   tempo 147.9309  matches 26
train9.wav   p-score 0.94886  tempo 129.0626  matches 25
train10.wav  p-score 0.81352  tempo 120.462   matches 14
train11.wav  p-score 0.94066  tempo 140.0333  matches 26
train12.wav  p-score 0.72527  tempo 54.514    matches 26
train13.wav  p-score 0.10452  tempo 180.5065  matches 29
train14.wav  p-score 0.94717  tempo 130.1702  matches 29
train15.wav  p-score 0.93257  tempo 185.3857  matches 9
train16.wav  p-score 0.94328  tempo 89.502    matches 20
train17.wav  p-score 0.62714  tempo 90.7786   matches 9
train18.wav  p-score 0.74763  tempo 60.5016   matches 32
train19.wav  p-score 0.86893  tempo 186.3674  matches 34
train20.wav  p-score 0.80886  tempo 219.2858  matches 38
P-Score： 0.7753
```



## 附录 2 

**真实标注数据**

| data        | $\tau_0$ (s) | Tempo (BPM) |
| ----------- | ------------ | ----------- |
| train1.wav  | 11.54        | 130         |
| train2.wav  | 13.80        | 170         |
| train3.wav  | 13.03        | 153         |
| train4.wav  | 14.57        | 126         |
| train5.wav  | 14.68        | 205         |
| train6.wav  | 14.40        | 82          |
| train7.wav  | 14.28        | 113         |
| train8.wav  | 14.24        | 148         |
| train9.wav  | 13.67        | 126         |
| train10.wav | 13.73        | 122         |
| train11.wav | 14.23        | 140         |
| train12.wav | 12.28        | 55          |
| train13.wav | 14.62        | 181         |
| train14.wav | 14.80        | 130         |
| train15.wav | 14.58        | 186         |
| train16.wav | 14.93        | 91          |
| train17.wav | 14.10        | 91          |
| train18.wav | 14.53        | 61          |
| train19.wav | 13.42        | 190         |
| train20.wav | 14.80        | 220         |





