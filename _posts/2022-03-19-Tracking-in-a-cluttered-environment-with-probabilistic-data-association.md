---
layout: article
title: 翻译：Tracking in a cluttered environment with probabilistic data association
mathjax: true
chart: false
mermaid: false
---

> Bar-Shalom, Yaakov and Edison T. S. Tse. “Tracking in a cluttered environment with probabilistic data association.” Autom. 11 (1975): 451-460.


# 杂波环境下基于概率数据关联的跟踪


《Automatica》，第11卷，第451-460页。Pergamon出版社，1975年。英国印刷。

YAAKOV BAR-SHALOM 和 EDISON TSE

实现了一种能够实现跟踪一个带有不确定性的目标且适合实时应用的算法，该算法利用对目标测量值的后验概率。

## 摘要

本文提出了一种新的方法来解决测量数据来源不确定时的跟踪问题。假设在轨迹上有一个感兴趣的物体（“目标”），并且在目标回波的预测位置附近的特定时间检测到并处理了许多不希望出现的回波值。一种次优估计方法被提出，该方法考虑了所有可能来自轨迹中物体的测量值而没有不断增长的内存和计算需求。取得（在预测收益的某个邻域内，称为“关联门”）每个回波值正确的概率——称为“概率数据关联”（PDA）。不希望出现的回波值假设为是均匀且独立分布的。通过使用PDA方法和经过适当修改的跟踪滤波器（称为PDAF）进行估计。由于PDAF的计算要求仅略高于标准滤波器，因此该方法可用于实时系统。在杂波环境中跟踪目标的仿真结果表明，PDAF比目前用于此类问题的标准滤波器提供了显著更好的结果。

## 1. 引言

本文针对测量数据的来源不确定时的跟踪问题，提出了一种新的次优方法。当传感器，例如雷达，在杂波或虚警率高的环境中工作时，可能会发生这种情况。我们假设非期望的回波值在时间和空间上独立出现，并且无法从过去的数据推断这些回波值的位置、性质或数量，这样假设的原因在于对环境的知识很少。假设目标的动力学以及驱动噪声的协方差已知；这里介绍的方法不适用于可能机动的目标。本研究的动机来自一个现实问题，在这个问题中，白噪声是一个很好的输入模型，例如文献[1]，而不希望出现的回波值会严重影响估计器的性能。

在这样的环境中跟踪目标时，可能会有几个“候选回波值”。假设已知肯定不是来源于感兴趣目标的回波值（例如通过其特征判别）已经被丢弃。然而，根据可用信息、正确回波值的预测位置及其统计数据及其测量位置，接收机仍能分辨出几个可能来自轨迹中的的物体的回波值。通常，接下来的流程是使用“标准过滤器”，是根据特定规则（例如“最近邻法”）选择回波值的其中一个，然后使用它更新轨迹中对象状态的估计值。在这种情况下，即使动力学和测量方程是线性的，噪声是高斯的，线性（卡尔曼）滤波器也不是最优的。这是因为在某些非零的概率下，选择用于更新的测量值不来源于轨迹中的物体。因此，该过滤器会因为过于“乐观”而降低性能——对估计值寄予过高的置信度——结果可能是丢失轨迹。在这种情况下，另一种常用的方法是“轨迹分裂”，即当多个回波值被认为是有效的时，每个可能的路径都会形成一条新的轨迹。这种方法非常保守，由于计算成本高，在实时系统中可能不可行。

Sea[2]和Singer等人[3,4]处理了这里考虑的问题，并开发了一种改进的跟踪滤波器。该跟踪过滤器考虑了与预测值最近邻的测量值不正确的可能性。然而，这是通过使用所选回波值源于轨迹中的对象的先验概率来完成的。Jaffer和Bar Shalom[6]推导出了一个改进的过滤器，该过滤器利用了所选回波值源于轨迹中对象的后验概率。该算法结合了实时信息来修改卡尔曼滤波器，而[2-4]仅使用先验信息。上述[2-4,6]过程是带有改进的过滤器的最近邻法。

本文提出的方法是获得一个包含可能来自轨道中物体的所有测量值的估计器，而不像以往的流程每次只选择其中一个。这种估计器利用了每个有效测量值都来自轨道中的物体的后验概率——因此，它包含了所有可用的信息。这些概率是通过测量位置获得的。*附加信息（例如目标特征）也可能包含在这些概率中。在计算这些概率时，假设与正确收益相对应的新息为正态分布。在计算这些概率时，假设与正确回波相对应的新息为正态分布。虽然这只是一种近似，但它允许我们得到一个固定内存的算法。仿真结果表明，PDAF的性能明显优于标准滤波器；上述结果可以认为证明了上述近似的可行性。该算法适用于下述意义中的环境：观测到的实际回波数及其位置将影响依据协方差估计的置信度。很明显，虽然没有滤波器能够在任意密集的杂波环境中进行跟踪，但在此提出的方法的重要性在于，它扩展了以杂波密度的范围，在该范围内可以可靠地跟踪具有合理计算需求的目标。

本文第2节描述了该问题，第3节描述了概率数据关联流程。第4节介绍了称为概率数据关联滤波器（PDAF）的新滤波器。此外，还展示了PDAF的计算要求，并与轨迹分裂滤波器（TSF）的计算要求进行了比较。第5节中给出的仿真结果表明，对于禁止使用TSF的跟踪问题，PDAF的性能相比最近邻标准滤波器有显著改善。

## 2. 问题描述

轨迹中物体的动力学特性使用以下方程建模
$$
\mathbf{x}_{k+1}=\mathbf{F}_{k} \mathbf{x}_{k}+\mathbf{w}_{k}, \quad k=0,1, \ldots, \tag{2.1}
$$

其中 $\mathbf{x}_k$ 是 $n$ 维状态向量，$\mathbf{F}_k$是传递矩阵（已知），$\mathbf{w}_k$是过程噪声，假设服从均值为零的正态分布，方差为
$$
E(\mathbf{w}_k\mathbf{w}_j')=\mathbf{Q}_k \delta_{kj} \tag{2.2}
$$

初始状态同样服从均值为$\mathbf{\hat{x}_{0\mid 0}}$，方差为$\mathbf{P_{0\mid 0}}$，与$\mathbf{w}_k$相互独立。

测量系统建模如下。如果测量源于轨迹中的对象，则
$$
\mathbf{z}_k=\mathbf{H}_k \mathbf{x}_k+\mathbf{y}_k,\quad k=1,..., \tag{2.3}
$$
其中, $\mathbf{H}_k$ 是已知的 $(r \times n)$ 矩阵，$\mathbf{y}_k$ 是测量噪声，与 $\mathbf{w}_j$ 和 $\mathbf{x}_0$ 相互独立且服从均值为零的正态分布，方差为
$$
E(\mathbf{v}_k\mathbf{v}_j')=\mathbf{R}_k \delta_{kj} \tag{2.4}
$$

第4节讨论了动态和/或测量方程为非线性的情况。

假设有一种“候选测量”的有效规则可用，确保以给定的概率保留正确回波值。这将在后面进行更详细的讨论。

错误回波值的模型如下：假设已经观察到一定数量的错误回波，则假设错误回波空间分布是一致且独立的。假设错误回波的概率分布为“分散”的均匀分布，即对其只有模糊的了解。下一节和附录C将对此进行更详细的讨论。

将 $k$ 时刻有效的测量值的集合表示为
$$
Z_k = {\{ \mathbf{z}_{k,i} \}_{i=1}}^{m_k} \tag{2.5}
$$

和
$$
Z^{k} \triangleq {\left\{Z_{j}\right\}_{j=1} }^{k} \tag{2.6}
$$

因此，最小方差估计，即条件平均值
$$
\hat{\mathbf{x}}_{k \mid  k}=\int \mathbf{x}_{k}\ p\left(\mathbf{x}_{k} \mid Z^{k}\right) \mathrm{d} \mathbf{x}_{k} \tag{2.7}
$$

上述方程构成了新方法的数学基础：在最小均方差意义上，最佳估计是在观察到的随机变量的条件下来计算的，这些随机变量在某些非零概率下依赖于 $\mathbf{x}_k$。因此，必须使用所有可能来自轨迹中物体的测量值。这种对所有随机变量的条件作用，在一些非零概率的情况下，依赖于 $\mathbf{x}_k$ 是这种方法区别于以前方法的主要特征。

## 3. 概率数据关联（PDA）方法

使用上一节中介绍的符号，定义以下事件
$$
\chi_{k, i}=\left\{\mathbf{z}_{k, i} \text { 是正确的返回值 }\right\}, \quad i=1, \ldots, m_{k} \tag{3.1}
$$

和
$$
\chi_{k, 0}=\left\{\mathbf{z}_{k, i} \text { 所有已验证的返回值都不是正确的 }\right\}. \tag{3.2}
$$

因为只有一个回波值是正确的，所以上述事件是互斥和完备的。据此，（2.7）可以写成*：
$$
\begin{aligned}
\mathbf{x}_{k \mid k} & \triangleq E\left\{\mathbf{x}_{k} \mid Z^{k}\right\} \\
                      & = \sum_{i=0}^{m_{k}} E\left\{\mathbf{x}_{k} \mid \chi_{k, j}, Z^{k}\right\} P\left\{\chi_{k, i} \mid Z^{k}\right\}
\end{aligned} \tag{3.3}
$$

*观察到的回波数 $m_k$ 包含在条件中。

因此，我们需要的是找到下式的表达
$$
\beta_{k,i} \triangleq P \{\chi_{k,i} \mid Z^k  \}, \quad i = 0,\ldots,m_k, \tag{3.4}
$$

即每个回波值来源于轨迹中物体的后验概率。这就是“概率数据关联”。如引言所述，假设无法从过去的数据推断错误回波值的数量。一个现实的假设是，传感器仅在有效区域内“看”，因此没有关于当前“杂波密度”的信息。Singer等人[7]假设这种密度是可用的，并使用带有一个固定的已知参数的泊松模型来处理无关报告。虽然这种模型适用于虚警，但它是否适用于可能存在杂波的环境（例如，集中在空间的某些区域）仍有疑问。假设我们对于环境无知，即无法推断错误回波值的数量，如附录C所示，等同于错误回波数量的不正确或“分散”均匀分布。

PDA方法中做出以下假设：

(1) 以过去的数据为条件，假设一个测量值是正确的，它的概率密度为
$$
p\left(\mathbf{z}_{k, i} \mid \chi_{k, i}, Z^{k-1}\right) \triangleq f\left(\mathbf{z}_{k, i} \mid Z^{k-1}\right) \tag{3.5}
$$

假设它可以求得。下一节将详细讨论这种概率密度，其中介绍改进后的滤波器。为了获得只占用固定内存的滤波器，将该概率密度近似为正态分布。

(2) 测量值不正确的概率密度在关联门内是均匀的，关联门的体积用$V_k$表示，即
$$
p\left(\mathbf{z}_{k, i} \mid \chi_{k, j}, Z^{k-1}\right) = V_k^{-1}, j \neq i. \tag{3.6}
$$

(3) 无法从过去的数据中推断出有效的回波值数量。

(4) 在已有过往数据都条件下每次回波正确的概率是相同的，即没有使用目标特征信息；可以修改程序以使用此类信息。

附录C详细说明了使用贝叶斯规则推导（3.4）中定义的概率 $\beta_{k,i}$。第 $i$ 次回波正确的后验概率如下：
$$
\begin{aligned}
\beta_{k, i} &=\left\{f\left(\mathbf{z}_{k, i} \mid Z^{k-1}\right)\right.\\
						 &\left.\bigg/ \left[b_{k}+\sum_{i=1}^{m_{k}} f\left(\mathbf{z}_{k, i} \mid Z^{k-1}\right)\right]\right\}, \quad i=1, \ldots, m_{k}, 
\end{aligned} \tag{3.7}
$$

其中
$$
b_{k} \triangleq m_{k} V_{k}^{-1} \frac{\alpha_{1}+\alpha_{2}-\alpha_{1} \alpha_{2}}{\left(1-\alpha_{1}\right)\left(1-\alpha_{2}\right)}. \tag{3.8}
$$

$\alpha_{1}$ 是正确回波不在有效区域内的概率，$\alpha_2$是正确回波不被检测到的概率。所有回波值都不正确的后验概率是
$$
\beta_{k, 0}=b_{k} /\left[b_{k}+\sum_{i=1}^{m_{k}} f\left(z_{k, i} \mid Z^{k-1}\right)\right] \tag{3.9}
$$

等式（3.7）和（3.9）构成了概率数据关联方法。

## 4. 概率数据关联滤波器

正如[5]中指出的，以所有有效测量值为条件计算状态的精确概率密度，即一个高斯密度和，相当于轨迹分裂，因此成本太高而不可行。因此，必须进行近似计算，才能得到一个能够实时实现的算法。

为了得到这样一个滤波器，基于过去观测值的状态变量可近似为服从均值为$\hat{\mathbf{x}}_{k\mid k-1}$，方差为$\mathbf{P}_{k\mid k-1}$，即
$$
p(\mathbf{x}_k\mid Z^{k-1})= \mathcal{N}(\hat{\mathbf{x}}_{k\mid k-1},\mathbf{P}_{k\mid k-1}) \tag{4.1}
$$

为了得到一个可行解，在其他相关问题中也做出了类似的假设，例如[8]。在[3]中也使用了这一假设，尽管没有明确说明。假设(4.1)的可行性因最终算法的简易性可认为是合理的，其在实际问题中的性能如下一节阐述。

如前所述，由于计算方面的考虑，只有通过特定有效测试的测量回波才会被考虑用于更新一条特定的轨迹。紧随(4.1)，与正确回波值相对应的残差或新息[9]，这里用$\mathbf{z}_{k\mid k-1}$表示，为
$$
\mathbf{v}_{k, j} \triangleq \mathbf{z}_{k, j}-\hat{\mathbf{z}}_{k \mid k-1} \tag{4.2}
$$

其中 $\hat{z}_{k \mid k-1}$ 是在近似(4.1)基础上观测值的条件均值，同样假设其服从均值为零，方差为$S_{k}$ 的正态分布
$$
\mathbf{S}_{k}=\mathbf{H}_{k} \mathbf{P}_{k \mid k-1} \mathbf{H}_{k}{ }^{\prime}+\mathbf{R}_{k} . \tag{4.3}
$$
有鉴于此，我们可以按如下方式编写有效测试：在k时刻接受满足以下条件的测量值
$$
\rho \triangleq \rho_k(\mathbf{v}_{k,j}) \le \gamma, \tag{4.4}
$$
其中$\gamma$是一个阈值，
$$
\rho_k(\mathbf{v}) \triangleq \boldsymbol{\nu}' \mathbf{S}_k^{-1} \boldsymbol{\nu} \tag{4.5}
$$
$\boldsymbol{\nu}_{k,i}$表示与测量值$\mathbf{z}_{k,i}$相对应的残差。式(4.5)定义的变量$\rho_k(\mathbf{v})$从现在起应称为距离或模的平方。

测试(4.4)实际上表示在给定的概率集中度下，一个有效的测量值需要在一个多大的椭球体内（一个置信椭球体）[10]。也就是说，常数$\gamma$是这样获得的：对于正确的回波值，其距离$\rho$服从$r$自由度$\chi^2$分布。对于一个给定的拒绝正确回波的概率值$\alpha_1$，可以通过查询$\chi^2$表来获得。

根据式(4.1)，过往测量值的近似充分统计表示为
$$
Y_{k\mid k-1} = \{\hat{\mathbf{x}}_{k\mid k-1},\mathbf{P}_{k\mid k-1} \} \tag{4.6}
$$

由(4.1)-(4.5)，给定一个测量值来源于轨迹中的物体，已经有效且以$Z^{k-1}$为条件，其概率密度(3.5)是一个截断的正态分布，即
$$
f\left(\mathbf{z}_{k, i} \mid Y_{k \mid k-1}\right)=\left(1-\alpha_{1}\right)^{-1} \mathcal{N}\left(H_{k} \hat{\mathbf{x}}_{k \mid k-1}, S_{k}\right) \tag{4.7}
$$

仅分布于关联门内，而在关联门外概率为零。

根据假设(4.1)和上一节描述的PDA方法，附录A推导了概率数据关联滤波器(PDAF)。状态变量的近似条件平均值如下所示：
$$
E\left\{\mathbf{x}_{k} \mid Z_{k}, Y_{k \mid k-1}\right\} \triangleq \hat{\mathbf{x}}_{k \mid k}=\hat{\mathbf{x}}_{k \mid k-1}+\mathbf{W}_{k} \boldsymbol{\nu}_{k} \tag{4.8}
$$

其中
$$
\boldsymbol{\nu}_{k} \triangleq \sum_{i=1}^{m_{k}} \beta_{k, i} \boldsymbol{\nu}_{k, i} \tag{4.9}
$$
是所有有效测量值的加权残差。 估计值 $\hat{\mathbf{x}}_{k \mid k}$是通过系数 $\beta_{k, i}$的观测值的非线性函数。系数 $\beta_{k, i}$ 是 $\mathbf{z}_{k, i}$来源于轨迹中的物体的后验概率， $\mathbf{W}_{k}$ 一个加权矩阵
$$
\mathbf{W}_{k}=\mathbf{P}_{k \mid k-1} \mathbf{H}_{k}{ }^{\prime} \mathbf{S}_{k}{ }^{-1}. \tag{4.10}
$$
与估计值(4.9) 相关的协方差为
$$
\mathbf{P}_{k \mid k}=\mathbf{P}_{k \mid k}^{0}+\mathbf{P}_{k},\tag{4.11}
$$
其中 $\mathbf{P}_{k \mid k}{ }^{0}$ 表示如果只有一个回波值时更新的方差。而
$$
\mathbf{P}_{k} \triangleq \mathbf{W}_{k}\left[\sum_{i=1}^{m_{k}} \beta_{k, i} \boldsymbol{\nu}_{k, i} \boldsymbol{\nu}_{k, i}{ }^{\prime}-\boldsymbol{\nu}_{k} \boldsymbol{\nu}_{k}{ }^{\prime}\right] \mathbf{W}_{k}{ }^{\prime} \tag{4.12}
$$
是一个半正定矩阵，当增大附录A中所述的更新$\mathbf{P}_{k\mid k}$的协方差，不正确测量值的影响增大。从(4.12)中可以看出，估计的置信度是实际有效的回波数及位置的函数。这是新滤波器的一个重要功能。

该算法的另一个重要特征是，当窗口中只有一个回波值时，其计算量与标准滤波器相同，并且仅当需要处理多个回波时，其计算要求才会增加。表1显示了新滤波器的计算量与标准滤波器*相比，计算机指令计数关于有效区域中回波值个数的函数近似增加。



图1显示了PDAF对比轨迹分裂滤波器(TSF)，相比标准滤波器每个采样周期预期的计算量。可以看出，当每个窗口内预期错误回波值的个数 $\bar{r}$ 大于1时，TSF的计算需求随着时间的增长呈指数增长；附录B讨论了相关方程式。虽然这些数字是一个上限，因为一些轨道可能会被丢弃，但计算机可能仍然会迅速饱和。正如将在下一节中说明的那样，在TSF不可行且PDAF的性能优于标准滤波器的情况下，PDAF可以在一定的 $\bar{r}$ 范围内成功使用。

PDAF相对于标准滤波器的每个采样时间的预期计算要求， vs 轨道分割滤波器（TSF）相对于标准滤波器的每个采样时间的预期计算要求


在动力学模型和/或测量为非线性的问题中，使用第3节假设的PDA方法可与方程(4.8)-(4.12)所述的滤波器一起使用，该滤波器基于关于最新估计的线性化系统。线性化引入的额外误差可以以类似于扩展卡尔曼滤波器的方式处理，例如通过增加过程噪声协方差[11]。下一节将给出使用这种“扩展的”PDAF解决实际问题的仿真结果。

## 5. 仿真结果

使用该算法进行仿真的问题是利用两个纯角度传感器对七维状态的目标进行跟踪。我们选择一个相对复杂且非线性的例子来评估PDAF的性能。由于这样的仿真更真实，因此基于它得出的结论比从简单例子中得出的结论更具有实际意义。通过使用扩展卡尔曼滤波器作为标准滤波器，并对最新估计值进行线性化，解决了非线性带来的额外困难。这个滤波器使用的是“最近邻法”，把最近邻回波值当成是正确的回波值，这在大多数应用中都是如此。PDAF由方程(4.8)-(4.12)组成，其中增益和协方差通过对最新估计值进行线性化，并结合第3节所述的PDA方法进行评估。

在惯性坐标系下，目标的动力学方程
$$
\mathbf{\dot{x}}(t)=\mathbf{f}[\mathbf{x}(t)]+\mathbf{w}(t), \tag{5.1}
$$
其中$\mathbf{w}$是过程噪声
$$
\mathbf{x}=[x, y, z, \dot{x}, \dot{y}, \dot{z}, \alpha]^{\prime}, \tag{5.2}
$$

$$
f(\mathbf{x})=\left[\begin{array}{c}
\dot{x} \\
\dot{y} \\
\dot{z} \\
\alpha \frac{\dot{x}+\Omega y}{v_{R}}-\mu \frac{x}{\left(x^{2}+y^{2}+z^{2}\right)^{\frac{3}{2}}} \\
\alpha \frac{\dot{y}-\Omega x}{v_{R}}-\mu \frac{y}{\left(x^{2}+y^{2}+z^{2}\right)^{\frac{3}{2}}} \\
\alpha \frac{z}{v_{R}}-\mu \frac{z}{\left(x^{2}+y^{2}+z^{2}\right)^{\frac{3}{2}}} \\
\frac{\alpha^{2}}{c}
\end{array}\right]
\tag{5.3}
$$

$x,y,z$ 是笛卡尔位置坐标，$\alpha$是单位质量轴向推力和阻力之间的净差
$$
\alpha = (T-D)/m \tag{5.4}
$$
其满足以下微分方程
$$
\dot{\alpha}=\alpha^2/c, \tag{5.5}
$$
其中 $c=4\ \mathrm{km/sec}$ ， $\alpha(0)=20\ \mathrm{m/sec^2}$ 。$\Omega$ 表示地球自转速率，
$$
{v_R}^2 =(\dot{x}+\Omega y)^2+(\dot{y}-\Omega x)^2+(\dot{z})^2 \tag{5.6}
$$
  $\mu$ 是重力常数。目标轨迹在无噪声的情况下生成；但是为了解释系统的非线性，假设滤波器的过程噪声如(5.1)所示，其“调谐”协方差等于更新状态协方差的分数（$10^{-2}$）。

每个传感器测量视线与目标的方位角和仰角，方差为$10^{-8}\ \mathrm{rad^2}$. 这些传感器位于不同的地点，保证目标轨迹的完全可观测性。在“干净”的环境中，扩展卡尔曼滤波器在大约10次观测后达到稳定状态，由此产生的99%有效区域（“标准窗口”）被用作定义杂波密度的基础。在仿真中，使用附录D中所述的随机数生成器生成了不正确的回波。该研究的参数为 $\bar{r}$，即标准窗口中预期的非期望回波数。

如前所述，PDAF不使用任何环境参数，因此，即使 $\bar{r}$是时变的，PDAF也可以在任何环境中使用，无需修改。

仿真的目的是研究PDAF与标准滤波器的跟踪能力。在这一阶段，没有新滤波器丢失轨迹的概率理论模型可用。这是因为协方差是可实时计算的，其取决于有效测量的数量。因此，仿真实验探究了PDAF的跟踪能力。

对于两个传感器，每个传感器每次运行的长度各为100个样本。图2显示了不同杂波密度下NNSF与PDAF的丢失轨迹的百分比。这些数字来自50次蒙特卡罗试验；如果至少在最近20次采样中，正确的测量值不在至少一个传感器的有效区域内，则认为轨迹丢失。这是对丢轨的合理定义，因为在这种情况下，最终误差非常大。可以看出，即使在 $\bar{r}=2$ 时，PDAF也能够可靠地跟踪，而在 $\bar{r}=0.75$ 时，NNSF已经有大约4%的丢轨概率。*（请注意，由于PDAF的窗口根据式(4.12)增加，因此PDAF窗口中的杂波回波的预期数量大于 $\bar{r}$ . ）还请注意，对于 $\bar{r}>1$，轨迹分割滤波器（TSF）的计算需求随时间呈指数增长（见图1）。因此，在NNSF丢失轨迹的可能性已经很高且TSF不可行的地方，PDAF似乎是有用的。

图3和图4展示了在两种 $\bar{r}$ 取值下，PDAF相比于标准滤波器50次运行的位置和速度估计的平均误差，对于 $\bar{r}=0.75$，PDAF的性能因与干净环境中相同而不能体现。可以看出，当$\bar{r}=1.5$时，性能只有轻微的减弱，甚至于$\bar{r}=2.25$，误差仍然不是很大。另外一方面，图2显示，NNSF在 $\bar{r}=2.25$ 时有大约40%的概率丢轨，相较之下PDAF的表现则相当不错。

因此，如该仿真所示，PDAF可以将跟踪能力扩展到高杂波密度区域，在该区域内，NNSF由于其较高的丢轨概率而变得不可靠。

## 6. 结论

在杂波的环境中运行的滤波器，如果利用好所有通过某个有效测试的测量值，其性能就会得到显著改善。利用这些测量值的方法是计算每个测量值是来自轨道中的对象的后验概率（“概率数据关联”），然后将它们合并到一个经过适当修改的滤波器PDAF中。为了得到一种计算量适合于实时应用的算法，我们做了一系列假设。这里构建的PDAF只适用于非机动目标。其性能通过一个仿真实验来阐述，实验将PDAF和目前正在使用的标准滤波器在这种类型的问题上进行对比。

致谢——作者希望感谢R.E.Larson博士在这项工作中给予的鼓励，M.Athans教授和A.G.Jaffer博士对这一主题进行的激励性讨论，以及N.Rosenfield女士对编程的大力支持。特别感谢编辑和审稿人的建设性批评。

## 参考文献

[1] R. K. MBHRA: A comparison of several non-linear filters for reentry vehicle tracking. IEEE Trans. Aut. Control AC-16, 307-319 (1971).

[2] R. G. SEA: An efficient suboptimal decision procedure for associating sensor data with stored tracks in realtime surveillance systems. Proc. of the 1971 IEEE Conference on Decision and Control, pp. 33-37, Miami Beach, Fla., December (1971).

[3] R. A. SiNGeR and J. J. S~xN: An optimal tracking filter for processing sensor data of imprecisely determined origin in surveillance systems. Proc. of the 1971 IEEE Conference on Decision and Control, pp. 171175, Miami Beach, Fla., December (1971).

[4] R. A. SrNoEa and R. SeA: New results in optimizing surveillance system tracking and data correlation performance in dense multitarget environments. IEEE Trans. Aut. Control AC-lg, 571-582 (1973).

[5] Y. B~-SHXLOM and A. JAVreR: Adaptive nonlinear filtering for tracking with measurements of uncertain origin. Proc. of the 1972 1FEE Conference on Decision and Control, pp. 243-247, New Orleans, La., December (1972).

[6] A. G. JArr~R and Y. BAR-SHALOM:On optimal tracking in multiple-target environments. Proceeding.~ of the Third Symposium on Nonlinear Estimation Theory and its Applications, pp. 112-117, San Diego, California, September (1972).

[7] R. SnqGER, R. SEA and K. HOUSEWmGnT:Derivation and evaluation of improved tracking filters for use in dense multitarget environments. 1973 Syrup. on Inf. Theory. [81G. A. ACKERSONand K. S. Fu: On state estimation in switching environments. IEEE Trans. Aut. Control AC-15, 10-17 (1970).

[9]T. KAmATt-I: An innovations approach to least-squares estimation. Part I: Linear filtering in additive white noise. IEEE Trans. Aut. Control AC-13, 646--654 (1968).

[10] T. S. FI~RGUSON:Mathematical Statistics. Academic Press, New York (1967).

[11] A. JAZWlNSKI: Stochastic Processes and Filtering Theory. Academic Press, New York (1970).

[12] M. DEGRooT: Optimal Statistical Decisions. McGrawHill (1970).

[13] Y. BAR-SHALOMand E. TsE: Tracking in a cluttered environment with probabilistic data association. Proc. of the Fourth Syrup. on Nonlinear Estimation, Theory and its Applications, pp. 13-22, San Diego, California, September (1973).

[14]A. PAI'OULIS: Probability, Random Variables, and Stochastic Processes. McGraw-Hill, New York (1965).



## 附录A  推导PDA滤波器

注意到 (3.1) 和 (3.2) 定义的$\chi_{k,i},i=0,...,m_k$，是互斥且完备的，我们有
$$
p(\mathbf{x}_{k}\mid Z_k,Y_{k\mid k-1})=\sum^{m_k}_{i=1}p(\mathbf{x}_k\mid \chi_{k,i},Z_k,Y_{k\mid k-1})\beta_{k,i}, \tag{A.1}
$$
其中
$$
\beta_{k,i} \triangleq P\{ \chi_{k,i}\mid Z_k,Y_{k\mid k-1} \} \tag{A.2}
$$
从 $\chi_{k,i}$ 的定义和 (4.1) 可以看出，对于 $i=1,...,m_k$
$$
\begin{aligned}
p(\mathbf{x}_k\mid \chi_{k,i},Z_k,Y_{k\mid k-1})&= p(\mathbf{x}_k\mid \chi_{k,i},\mathbf{z}_{k,i},Y_{k\mid k-1})\\
&= \mathcal{N}(\hat{\mathbf{x}}_{k\mid k,i},\mathbf{P}_{k\mid k,i}), 
\end{aligned}
\tag{A.3}
$$
其中
$$
\begin{aligned}
\mathbf{x}_{k \mid k, i} =\mathbf{x}_{k \mid k-1}+\mathbf{W}_{k, i} \boldsymbol{\nu}_{k, i}, 
\end{aligned} \tag{A.4}
$$

$$
\mathbf{W}_{k, i} =\mathbf{P}_{k \mid k-1} \mathbf{H}_{k}{ }^{\prime}\left[\mathbf{H}_{k} \mathbf{P}_{k \mid k-1} \mathbf{H}_{k}{ }^{\prime}+\mathbf{R}_{k}\right]^{-1} \triangleq \mathbf{W}_{k}, \tag{A.5}
$$

$$
\mathbf{P}_{k \mid k, i} =\left(I-\mathbf{W}_{k} \mathbf{H}_{k}\right) \mathbf{P}_{k \mid k-1} \triangleq \mathbf{P}_{k \mid k}{ }^{*}. \tag{A.6}
$$

注意到权重矩阵 $\mathbf{W}_{k,i}$ 和 $i$ 无关。然而，式 (A.6) 中的 $\mathbf{P}_{k\mid k,i}=\mathbf{P}_{k \mid k}{ }^{*}$ 是以 $\chi_{k,i}$ 为条件，状态估计 (A.4) 的协方差，虽然它和 $i$ 的取值无关，后面会看到它不是最终估计的协方差。

如果事件 $\chi_{k,0}$ 发生，即没有一个有效的回波值是正确的，则 (A.3) 和 (A.6) 保持不变，且
$$
\mathbf{W}_{k,0}=0, \tag{A.5'}
$$

$$
\mathbf{P}_{k \mid k, 0}=\mathbf{P}_{k \mid k-1}, \tag{A.6'}
$$

因此，我们现在有了 (A.1) 中进入求和的密度。

由式 (A.1) 可以便利地求出状态的条件均值，写成
$$
\begin{aligned}
E\left\{\mathbf{x}_{k} \mid Z_{k}, Y_{k \mid k-1}\right\} &\triangleq \hat{\mathbf{x}}_{k \mid k} \\
&= \sum_{i=0}^{m_{k}} \hat{\mathbf{x}}_{k \mid k, i} \beta_{k, i}\\
&=\hat{\mathbf{x}}_{k \mid k-1}+\mathbf{W}_{k}\boldsymbol{\nu}_{k},  
\end{aligned}
\tag{A.7}
$$
其中
$$
{\boldsymbol{\nu}}_{k} \triangleq \sum_{i=1}^{m_{k}} \beta_{k, i}{\boldsymbol{\nu}}_{k, i} \tag{A.8}
$$
注意 $\hat{\mathbf{x}}_{k \mid k}$ 是通过系数 $\beta_{k, i}$ 的观测值的非线性函数。

与上述估计相关的方差如下
$$
\mathbf{P}_{k \mid k}=\int\left[\mathbf{x}_{k}-\hat{\mathbf{x}}_{k \mid k}\right]\left[\mathbf{x}_{k}-\hat{\mathbf{x}}_{k \mid k}\right]^{\prime} p\left(\mathbf{x}_{k} \mid Z_{k}, Y_{k \mid k-1}\right) \mathrm{d} \mathbf{x}_{k}  \tag{A.9}
$$
利用全概率公式和标记 (A.2)，上式可以写成
$$
\begin{aligned}
\mathbf{P}_{k \mid k} &=\sum_{i=0}^{m_{k}} \beta_{k, i} \\
& \int\left[\mathbf{x}_{k}-\hat{\mathbf{x}}_{k \mid k}\right]\left[\mathbf{x}_{k}-\hat{\mathbf{x}}_{k \mid k}\right]^{\prime} p\left(\mathbf{x}_{k} \mid \chi_{k, i}, Z_{k}, Y_{k \mid k-1}\right) \mathrm{d} \mathbf{x}_{k}
\end{aligned}  \tag{A.10}
$$
经过一些代数之后，我们得到
$$
\begin{aligned}
P_{k \mid k} &=\mathbf{P}_{k \mid k}^{0}+\sum_{i=0}^{m_{k}} \beta_{k, i} \hat{\mathbf{x}}_{k \mid k, i} {\hat{\mathbf{x}}_{k \mid k, i}}^\prime-\hat{\mathbf{x}}_{k \mid k} {\hat{\mathbf{x}}_{k \mid k}}^{\prime} \\
&=\mathbf{P}_{k \mid k}^{0}+\mathbf{W}_{k}\left[\sum_{i=1}^{m_{k}} \beta_{k, i}{\boldsymbol{\nu}}_{k, i}{\boldsymbol{\nu}}_{k, i}^{\prime}-\boldsymbol{\nu}_{k}{\boldsymbol{\nu}}_{k}^{\prime}\right] {\mathbf{W}_{k}}^{\prime} \\
& \triangleq \mathbf{P}_{k \mid k}^{0}+\mathbf{P}_{k} .
\end{aligned}  \tag{A.11}
$$
从 (A.14) 中可以很容易看出该滤波器的自适应性：估计值的置信度是实际有效回报数的函数。矩阵 $\mathbf{P}_{k}$ 通过增加估计值的协方差来显示并非源自轨迹中对象的测量值的影响。这是因为 $\mathbf{P}_{k}$ 是半正定的，如[13]所示。

请注意，协方差的计算需要实时数据。这是非线性滤波器的一般特征。

## 附录B  轨迹分裂滤波器（TSF）的计算需求

假设在时间 $k$ 的第 $i$ 个窗口中检测到的错误回波数是一个随机变量 $r_i(k)$ .  从时间 $k=0$ 开始，如果每次检测到一个以上的回波时都分割轨迹，则在特定时间 $k$ 设置的窗口数如下所示。

在 $k=0$ 时，有一个窗口，窗口中检测到了
$$
1(正确)+R(0)(错误)
$$
个回波。 $k=1$ 时，对于每一个这种回波，对应建立一个新的窗口。在一个这种窗口内，有 $1+r_0(1)$ 个回波，包括正确的回波值，而在另一个窗口中有 $i=1,...,R(0)$ 个回波。下标零表示正确回波所在的窗口。假设窗口不重叠，回波的总数现在是 $1+R(0)$，其中
$$
R(1)=\sum^{R(0)}_{i=0}r_i(1). \tag{B.1}
$$
同样的， $k=2$ 时，有 $1+R(2)$ 个回波，其中
$$
R(2)=\sum^{R(1)}_{i=0}r_i(2). \tag{B.2}
$$
一般来说，$k$ 时刻的回波数是 $1+R(k)$ ，其中
$$
R(k)=\sum_{i=0}^{R(k-1)}r_i(k). \tag{B.3}
$$
假设门限中错误回波的预期数量为
$$
Er_i(k)=\bar{r}(k) \tag{B.4}
$$
对于 $k \neq m $ 且 $ \forall i,j  ，$ $r_i(k)$ 与 $r_{j}(m)$ 相互独立。那么，此时的预期轨迹数是一个和
$$
\bar{N}(k)=1+E R(k)  \tag{B.5}
$$

$$
\begin{aligned}
\\
E R(k)=& \bar{r}(k)[1+E R(k-1)] \\
=& \bar{r}(k)\{1+\bar{r}(k-1)[1+E R(k-2)]\} \\
=& \sum_{j=1}^{k} \prod_{l=j}^{k} \vec{r}(l) .
\end{aligned} \tag{B.6}
$$
令 $\alpha$ 为每个轨迹的计算量。简便起见，假设$\bar{r}(k)=\bar{r}$，那么第 $k$ 时刻预期计算需求为
$$
\begin{aligned}
\bar{C}(k) &=\alpha \bar{N}(k)=\alpha\left[1+\sum_{j=1}^{k} \bar{r}^{j}\right] \\
&=\left\{\begin{array}{l}
\alpha\left[1+\frac{\bar{r}-\bar{r}^{k+1}}{1-\bar{r}}\right], \quad \bar{r} \neq 1 \\
\alpha(1+k), \quad \bar{r}=1 .
\end{array}\right.
\end{aligned} \tag{B.7}
$$
如果 $\bar{r}(k)$ 是时变的，那么闭式表达式 (B.7) 会变化，(B.6) 需要直接估计。图4显示了各种 $\bar{r}$ 值的预期计算需求随时间的增加而增加。可以看到，当 $\bar{r}<1$ （即每个窗口不正确回波的期望数量小于单位数量），$\bar{C}$ 趋近于一个常数，即
$$
\lim _{k \rightarrow \infty} \bar{C}(k)=\alpha \frac{1}{1-\bar{r}}. \tag{B.8}
$$

## 附录C  PDA方法的推导

利用贝叶斯法则，式 (3.4) 中的概率可以写成 $[P1]$
$$
\begin{aligned}
P\{\chi_{k,i}\mid Z^k,m_k \}&= P\{\chi_{k,i}\mid Z^k,Z^{k-1},m_k \} \\
&=c_k^{-1}p(Z_k\mid \chi_{k,i},Z^{k-1},m_k) \\
&\quad \quad  \times P\{m_k\mid \chi_{k,i},Z^{k-1}\}P\{\chi_{k,i}\mid Z^{k-1}\}
\end{aligned}
\tag{C.1}
$$
其中，关于在 $k$ 时刻等于 $m_k$ 时观察到的总回波数的条件已经明确写出，且
$$
c_k = \sum^{m_k}_{i=0}p(Z_k\mid \chi_{k,i},Z^{k-1},m_k)P\{m_k\mid \chi_{k,i},Z^{k-1}\}\\ 
\times P\{\chi_{k,i}\mid Z^{k-1}\} \tag{C.2}
$$
是一个归一化常数。

注意到在 $\chi_{k,0}$ (没有回波值是正确的) 和 $Z^{k-1}$ 的条件下，观察到回波总数等于 $m_k$ 的概率是
$$
P\{m_k\mid \chi_{k,0},Z^{k-1}\}=P\{I_k=m_k\mid Z^{k-1}\}, \quad i=1,...,m_k \tag{C.3}
$$
期中 $I_k$ 是 $k$ 时刻观察到的不正确的回波数。在 $\chi_{k,i}$ ($i\neq0$) 和 $Z^{k-1}$ 的条件下，观察到回波总数等于 $m_k$ 的概率是
$$
P\{m_k\mid \chi_{k,i},Z^{k-1}\}=P\{I_k=m_k-1\mid Z^{k-1}\}, \quad i=1,...,m_k \tag{C.4}
$$
因为在这种情况下，有一个回波是正确的。

 “无法根据从过往数据中作出推理” 这个假设，可以翻译成：事件 $I_k=m_k$ 和事件 $I_k=m_k-1$ 大致是等价的，即
$$
P\{I_k=m_k\mid Z^{k-1}\}=P\{I_k=m_k-1\mid Z^{k-1}\}= {\prod}_0. \tag{C.5}
$$
注意到 ${\prod}_0$ 的值是无关紧要的，因为它在 (C.1) 中被消去了。这种分布常用于描述模糊的先验知识，这是不恰当的，因为其积分是无限的；然而由于上述消除，最后的结果是正确的。对于不正确的先验分布的详细讨论见[12].

对于 $i=0$，即所有有效的回波都是不正确的，从第2节讨论的视角来看，其联合概率是
$$
p(Z_k\mid \chi_{k,0},Z^{k-1},m_k)= \prod^{m_k}_{i=1}p(z_{k,i}\mid \chi_{k,0},Z^{k-1},m_k)=V_k^{-m_k} \tag{C.6}
$$
由于 $V_k$ 表示关联门的体积，则
$$
p(z_{k,i}\mid \chi_{k,j},Z^{k-1},m_k)=V_k^{-1},j\neq i \tag{C.7}
$$
这样因为我们假设不正确的测量值是均匀分布的。

基于过往数据，$\chi_{k,0}$ 的概率是
$$
P\{\chi_{k,0}\mid Z^{k-1},m_k\} = \alpha_1+(1-\alpha_1)\alpha_2, \tag{C.8}
$$
其中 $\alpha_1$ 是正确回波不落在关联门内的概率， $\alpha_2$ 是正确回波不被探测到的概率。

对于 $i=1,..,m_k$ 我们可以将概率写在 (C.1) 的右手边
$$
\begin{aligned}
p&\{Z_k\mid x_{k,i},Z^{k-1},m_ks\}\\
&=p(z_{k,i}\mid \chi_k,i,Z^{k-1}) \prod_{j=1,i\neq j}^{m_k}p(z_{k,j}\mid \chi_{k,i},Z^{k-1},m_k)\\
&=[V_k]^{-(m_k-1)} p(z_{k,j}\mid \chi_{k,i},Z^{k-1},m_k).
\end{aligned} \tag{C.9}
$$
基于过往数据，$z_{k,i}$ 是正确回波的概率密度假设已知，用下式表示
$$
p(z_{k,i}\mid \chi_{k,i},Z^{k-1}) \triangleq f(z_{k,i}\mid Z^{k-1}) \tag{C.10}
$$
基于过往数据， 假设 $\chi_{k,i}$ 的概率对于所有的 $i$ 相同，除非可以使用目标的特征信息。如果没有这种信息可用，则
$$
\begin{aligned}
P\{\chi_{k,i}\mid &Z^{k-1}\}\\
&=\frac{1-P\{\chi_{k,0}\mid Z^{k-1}\}}{m_k}\\
&=\frac{(1-\alpha_1)(1-\alpha_2)}{m_k} ,\quad i=1,...,m_k. 
\end{aligned} \tag{C.11}
$$
将 (C.6)，(C.9) 和 (C.11) 插入 (C.1) , 构成 PDA 方法 (3.7) 和 (3.9).

## 附录D  不正确回波的仿真实验

出于本次模拟的目的，产生如下所示不正确的回波。每一步中，为每一个传感器生成关联门（99%的置信度）。稳定状态下的标准滤波器生成的关联门的区域（窗口）用 ${A_v}^0$ 表示。我们研究的参数
$$
\bar{r} = {A_v}^0 \bar{\rho}, \tag{D.1}
$$
是标准窗中不正确回波数量的期望，$\bar{\rho}$ 是他们的期望概率密度。然后，$A_v$ 是有效区域的实际面积，并且如 (4.12) 所示，PDAF是时变的，
$$
n=[10A_v \bar{\rho}+1]^*\tag{D.2}
$$
\* $[x]$是小于等于 $x$ 的最大整数。

测量值在以正确测量值的预测为中心的正方形内均匀产生，正方形的面积是
$$
A=n/\bar{\rho} \tag{D.3}
$$

图5描述了上述过程，其中正确测量值使用 “ * ” 表示，错误测量值使用 “ + ” 表示。在这种情况下，因为 $A \gg A_v$ ，有效的不正确回波值的是均值为 $A_v \rho$ 的随机数。这个产生错误回波的过程可认为较准确地模拟了现实中杂波或虚警率较高的情况。在每一次运行中，参数 $\bar{r}$ 保持不变。 
