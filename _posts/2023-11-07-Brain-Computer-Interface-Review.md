---
layout: article
title: 脑机接口研究基础
mathjax: True
article_header:
  type: overlay
  theme: dark
  background_color: '#203028'
  background_image:
    gradient: 'linear-gradient(0deg,rgba(0, 0, 0, .5),  rgba(255, 255, 255 , .0))'
    # src:  /pictures/BCI/大脑皮层.png
---

<!--more-->

大脑分为四个叶：额叶、顶叶、枕叶和颞叶。中央沟将额叶与顶叶分开。它还分离中央前回（红色表示）和中央后回（蓝色表示）。颞叶和额叶被侧裂分开。枕叶位于大脑的后部。以下皮层区域对脑机接口特别重要：**运动区、体感皮层、后顶叶皮层和视觉皮层。**

![](/pictures/BCI/大脑皮层.png)



## 脑机接口分类

理论上，脑机接口可能会使用通过多种方法记录的大脑信号。这些包括： 
- **电场或磁场的记录 (例如脑磁图 Magnetoencephalography, MEG)**
- **功能近红外光谱（functional near-infrared spectroscopy, fNIRS）**
- **正电子发射断层扫描 (positron emission tomography, PET)**
- **功能磁共振成像 (functional magnetic resonance imaging, fMRI)**

然而实际上，这些方法中的大多数目前由于其复杂的技术要求、高昂的费用、有限的实时能力和/或开发的早期阶段而不适用于临床。只**有电场记录（可能还有 fNIR）可能在不久的将来对临床应用具有重要的实用价值**。

**电场记录**测量中枢神经系统突触、神经元和轴突在接收和传递信息时膜电位的变化。神经元接收并整合突触输入，然后以动作电位的形式将结果传递到轴突。突触电位和动作电位反映了离子在神经元膜上流动的变化。这种活动产生的电场记录为
- 由头皮上的电极记录头皮电压变化：**脑电图 (electroencephalograph, EEG)** 
- 由皮层表面的电极记录皮层表面电压变化：**皮层脑电 (electrocorticography, ECoG)** 
- 由插入皮层的电极阵列记录大脑内电压变化：**局部场电位 (local field potential, LFP)** 和 **神经元动作电位（spikes）** 

![](/pictures/BCI/检测大脑电活动的三种不同方法：EEG、ECoG和皮质内记录.png)

这三种可供选择的记录方法如图所示。用这些记录方法可以检测到的任何电压变化都可能构成对脑机接口有用的大脑信号特征。皮质内电极可以检测单个神经元尖峰频率的调节；LFP、ECoG或EEG记录可以检测事件相关电位（ERPs）。


## EEG 脑机接口

脑机接口通常无法读取大脑或破译思想。**脑机接口只能检测和分类与特定任务或事件相关的正在进行的大脑信号中的特定活动模式(Brain Patterns)**。脑机接口的用户必须做**心理策略(Mental Strategies)**指定的事情才能产生这些模式。最常见的心理策略是**选择性注意(Selective Attention, 常用 SSVEP 和 P300)**和**运动想象(Motor Imagery, MI)**。选择性注意力的脑机接口方法在不同的用户和使用场景中非常可靠，并可以相当快速的通信，且用户可以在没有任何培训的情况下产生 P300 和 SSVEP。几乎所有的受试者都可以在几分钟内学会专注于目标字母或符号的简单任务。**与基于选择性注意的脑机接口相比，基于 MI 的脑机接口不依赖于外部刺激。**然而， MI 是一项必须学习的技能。基于 MI 的脑机接口通常在第一次治疗期间效果不佳。相反，与选择性注意的脑机接口不同，一些培训是必要的。

### 基于 SMR 的脑机接口

移动肢体甚至收缩一块肌肉都会改变大脑皮层的活动。事实上，已经对运动的准备或对运动的想象也改变了所谓的**感觉运动节律（Sensorimotor rhythms, SMR）**。感觉运动节律是指从**体感和运动区域**记录的大脑活动的振荡。大脑活动振荡通常根据特定的频带进行分类，这些频带以希腊字母命名（$\delta$：＜4 Hz，$\theta$：4–7 Hz，$\alpha$：8–12 Hz，$\beta$：12–30 Hz，$\gamma$：＞30 Hz）。**从感觉运动区域记录的 $\alpha$ 活动也被称为 $\mu$ 活动**。脑电信号中与特定神经活动相关的特定频带内振荡活动的减少称为**事件相关去同步(event-related desynchronization，ERD)**。相应地，在特定频带中振荡活动的增加被称为**事件相关同步(event-related synchronization，ERS)**。ERD/ERS模式可以由 MI 自发地产生，MI 是对运动的想象，而不是实际执行运动。**对 MI 最重要的频带是 EEG 信号中的 $\mu$ 和 $\beta$** 活动。侵入性脑机接口通常也使用 $\gamma$ 活动，而安装在头部外部的电极很难检测到 $\gamma$ 活动。**用户想象某侧肢体运动时，其对侧感觉运皮层的 EEG 在 $\mu$ 节律和 $\beta$ 节律能量上会出现 ERD 现象**。

从结构上看，ERD/ERS 的模式遵循人类组织结构。右手 MI 产生的活动在电极位置 C3 上最为突出。左手 MI 产生的活动在 C4 中最为突出。也就是说，手部 MI 所调用的活动位于对侧。足部 MI 调用 Cz 上的活动。在脑电图中无法区分左脚和右脚的运动，因为相应的皮层区域太近了。类似地，在 EEG 中不能区分单个手指的 ERD/ERS 模式。**为了产生可以检测的模式，所涉及的皮层区域必须足够大，以便与剩余的脑电图（背景脑电图）相比，所产生的活动足够突出**。手部区域、脚部区域和舌头区域相对较大，并且结构不同。因此，MI 脑机接口是通过想象移动左手、右手、脚和舌头来控制的。

![](/pictures/BCI/国际10-20系统.png)

Fig. 10-20国际标准导联系统：头部左侧视图和俯视图。

<img src="/pictures/BCI/国际10-10系统.png" alt="国际10-10系统" width="400">

Fig. 10-10国际标准导联系统: Nasion 是额骨和鼻骨在鼻梁处的交叉点。Inion 是头骨后部颈部上方的一个小凸起。

字母代表的含义：F: 额叶(Frontal lobe)，Fp:前额叶（Frontal poles），T: 颞叶（Temporal lobes），O: 枕叶（Occipital lobes），P: 顶叶（Parietal lobes），C: 中心部（Central） 或感觉运动皮层（sensorimotorcortex），Z: 零点（zero）即左右脑中心

另一种类型的 MI 脑机接口依赖于更抽象、针对特定主题的运动类型。通过脑机接口的多次训练过程，人们可以学习和制定自己的 MI 策略。虽然不同受试者的表现和训练时间有所不同，但大多数受试者可以通过 1-4 小时的训练在两个选择的任务中获得良好的控制。然而，通常需要更长时间的训练才能实现足够的控制。因此，培训是许多 BCI 的重要组成部分。用户通过称为**操作性条件反射(operant conditioning)**的过程进行学习，这是心理学中的一个基本术语。在操作性条件反射中，人们学会将某种行为与反应或效果联系起来。例如，人们知道触摸热炉子很痛苦，并且再也不会这样做。在 BCI 中，想要向上移动光标的用户可能会发现，在心理上想象某个运动任务（例如握紧拳头）的效果不如思考此类运动的动觉体验有效。 BCI 学习是操作性条件反射的一个特例，原因是用户没有执行经典意义上的动作，因为他/她没有移动。尽管如此，如果想象的行为产生效果，那么条件反射仍然会发生。在 BCI 使用过程中，**操作条件反射涉及通常在计算机屏幕上显示的反馈进行训练**。正反馈表明大脑信号以所需的方式进行调节。当用户无法执行所需的任务时，会给出负面反馈或不给出反馈。 BCI 学习是一种称为**神经反馈(neurofeedback)**的反馈。反馈表明用户是否通过 BCI 很好地执行了心理任务或未能实现预期目标。用户可以利用此反馈来优化他们的心理任务并提高 BCI 性能。反馈可以是触觉的或听觉的，但最常见的是视觉的。例如，在光标移动任务中，人们了解哪种类型的想象移动最适合 BCI 控制，并可靠地向上或向下移动光标。通过进一步训练，一些受试者可以学会在两个甚至三个维度上移动光标。

### 基于 P300 信号的脑机接口

P300 响应是一种与决策认知过程相关的**事件诱发电位(event-related potential, ERP)**，是一种大脑对新奇刺激的特异性响应成分。在周期性常规刺激中穿插一个无规律的，期望的、或显著的听觉、视觉或体感（触摸）的新奇刺激，**在新奇刺激呈现后约 300ms，大脑中央区及顶区的电极可以采集到一个特异性的正成分， 这种正成分就被称为 P300 成分** 。虽然P300的潜在神经发生器存在争议，但人们认为，该信号反映了对正在进行的活动的快速神经抑制，这种抑制促进了刺激/任务信息从额叶皮层到颞顶叶皮层的传递。在基于P300的脑机接口系统中，向用户呈现一系列听觉、视觉或体感刺激，每个刺激代表特定的输出（例如，拼写特定的字母），并关注代表他/他想要的动作的刺激。由于P300脑机接口只需要用户调节注意力，而不是任何肌肉输出，因此它是一种**独立的脑机接口**。

![](/pictures/BCI/可用于脑机接口系统的大脑信号示例.png)

Fig. 可用于脑机接口系统的大脑信号示例。(a)：在中央顶叶皮层上的EEG中，在刺激开始后约300ms（即 P300 ERP），穿插在无关刺激之间的相关刺激引起正峰值。脑机接口检测 P300 并执行与参与刺激相关的命令。(b)：用户控制通过感觉运动皮层记录的脑电图中 8-12 Hz $\mu$ 节律（或 18-26 Hz $\beta$ 节律）的振幅，将光标移动到屏幕顶部的目标或底部的目标（或中间位置的其他目标）。顶部和底部目标的频谱（顶部）和样本脑电图轨迹（底部）表明，该用户的控制集中在 $\mu$ 节律频带中。用户还可以学习控制二维或三维的运动。(c)：与向右移动相比，向左移动操纵杆期间，ECoG信号中的高频 γ 振荡（>30 Hz）增加。ECoG记录的 γ 振荡可能对未来脑机接口系统中的光标控制有用。 (d)：四肢瘫痪的脑机接口用户以向左或向左移动计算机光标的首选方向减少或增加单个神经元的放电。光栅图显示了单个试验的单个神经元活动，直方图是20个试验的总和。实际光标轨迹是根据具有不同优选方向的许多神经元的发射率计算的。

**快速连续视觉呈现（rapid serial visual presentation, RSVP）**是一类基于 P300 信号的实验范式。P300 成分可以通过视觉、听觉等诱发，其中视觉诱发 P300 在脑机接口领域应用最为广泛。经典 P300 脑机接口中，目标字符排列为二维矩形阵列，并且每行和列按照一定次序依次闪烁，如所图 1-4 示。**用户需要注视其中某一目标，当该目标闪烁时，用户的脑电信号中便会诱发出 P300 成分**。通过检测 P300 成分出现时间，即可判断出用户关注的目标。

![](/pictures/BCI/典型基于%20P300%20信号的脑-机接口系统的信号特征及系统界面.png)


### 基于 SSVEP 的脑机接口

大脑视皮层对视觉刺激产生的响应被称为**视觉诱发电位(visual evoked potential, VEP)**，这些响应信号通过颅骨传导到头皮表面，并且在传导过程中相互混叠，从而形成**稳态视觉诱发电位(steady-state visual evoked potential，SSVEP)**。图形变化都可能会诱发 VEP，例如闪光刺激，图形翻转，光栅变化等。EEG 记录数据中，VEP 通常在视觉皮层响应最为强烈，因此采集 VEP 时通常需要包括枕叶区。

![](/pictures/BCI/视觉感知觉通路.PNG)

VEP 与视觉刺激的空间和时间特性密切相关。当视觉刺激出现在**中央视野区**，通常会比刺激出现在外周视野区引起幅度更加显著的 VEP，同时，VEP 也与视觉刺激出现的时刻具有较强的相关性，反映在信号特征上即为**锁时、锁相特点**。SSVEP 是一类特殊的视觉诱发电位。它**由具有固定频率的视觉刺激所诱发**，当视觉刺激以特定频率进行周期性呈现时（如闪烁、图形翻转、图形放缩等），视觉系统会受其影响产生出具有稳定频率特征的诱发响应。**这种诱发响应通常包含与刺激频率相同的频率成份及其高次谐波成份**。

![](/pictures/BCI/典型%20SSVEP%20响应信号波形及频谱.PNG)

**锁时、锁相、延迟特性**：在一定频率的闪烁刺激下，**SSVEP 表现为具有与刺激信号相位锁定，频率对应的近似正弦信号，它具有很好的锁时锁相及相位跟随能力**。受视觉注意、瞳孔放缩等影响，SSVEP 响应包络并不能在刺激开始时瞬间达到峰值，而是逐渐升高并达到相对稳定。视觉刺激启动及终止阶段 SSVEP 响应存在一个持续约 **120ms~150ms** 的过渡过程。

**空域特征**：**SSVEP的不同频率成分来自于不同脑区**，因此传导到头皮表面时不同频率的成分也呈现出各自的空域特征。**刺激目标在视野中出现的位置也会影响到SSVEP信号的强度及空间分布**。当受试者关注于刺激目标时，如果其视觉焦点与注意焦点重合，刺激目标通常位于视网膜中央凹区域，此时诱发的SSVEP响应较强。SSVEP的注意调制也具有显著的**对侧优势**，当刺激目标出现在视野一侧时，位于视野对侧皮层的SSVEP响应幅度将远远高于与刺激目标同侧皮层的SSVEP响应幅度。

**频响特性**：在单一频率刺激下，受试者的脑电信号中对应频率的基频、二次谐波及高次谐波均有显著增强。SSVEP响应的这种频率特征是由视觉通路的生理特性所引起，在不同受试者的数据中表现出较好的一致性。**随着谐波次数的增高，SSVEP在谐波频点处的能量总体上呈现逐渐降低的趋势**。高频成分（>30Hz）会显著衰减，因此从窄带信噪比角度观察，SSVEP高次谐波成分依然具有很高的窄带信噪比，故可以为处理识别过程提供更多的有用信息。在低频刺激下（8Hz~15.8Hz），大多数受试者SSVEP响应**随频率升高而降低**。

![](/pictures/BCI/SSVEP%20响应基频及谐波成分示意图.png)

视觉系统可以对较大频率范围的刺激信号产生响应，从而诱发 SSVEP 成分。目前有研究表明，能够诱发 SSVEP 的刺激频率上限可以达到 80Hz，而根据刺激频率范围，可以按照 SSVEP 响应的强弱将刺激频率划分为三个主要频段：**低频刺激段 (<15Hz)，中频刺激段 (15~30Hz)，高频刺激段(>30Hz)**，如图2-4所示。其中，**低频刺激诱发的 SSVEP 响应最强，中频范围及高频范围都有局部峰值出现**。需要特别指出的是，不同受试者对刺激频率范围的响应差异较大，因此上图只代表总体趋势。

![](/pictures/BCI/不同频率范围刺激下%20SSVEP%20响应曲线示意图.png)


## 常用算法

<!-- ### 典型相关分析(Canonical Correlation Analysis, CCA)

CCA 主要用于在两组多元数据中寻找其内在隐含关系，被广泛应用与 SSVEP 检测。假设多导 EEG 脑电信号 $$\mathbf{X}\in\mathbb{R}^{L_1\times N}$$，SSVEP 模板信号为 $$\mathbf{Y}\in\mathbb{R}^{L_2\times N}$$，其中 $N$ 为样本个数，而 $L_1, L_2$ 分别为 $\mathbf{X}$ 和 $\mathbf{Y}$ 的特征维度。模板信号定义为刺激频率的正弦和余弦信号，及其高次谐波信号组成的矩阵，即

$$
\mathbf{Y}=\frac{1}{\sqrt{N}}\begin{bmatrix}\sin\left(\omega \cdot 0\right)&\cdots&\sin\left(\omega(N-1)\right)\\\cos\left(\omega \cdot 0\right)&\cdots&\cos\left(\omega(N-1)\right)\\\sin\left(2\omega \cdot 0\right)&\cdots&\sin\left(2\omega(N-1)\right)\\\cos\left(2\omega \cdot 0\right)&\cdots&\cos\left(2\omega(N-1)\right)\\\vdots&\vdots&\vdots\\\sin\left(\frac{L_2}{2}\omega \cdot 0\right)&\cdots&\sin\left(\frac{L_2}{2}\omega(N-1)\right)\\\cos\left(\frac{L_2}{2}\omega \cdot 0\right)&\cdots&\cos\left(\frac{L_2}{2}\omega(N-1)\right)\end{bmatrix}
$$

其中 $\omega$ 为刺激频率 $f$ 和采样频率 $f_s$ 下的刺激角频率，等于 $$ 2\pi f / f_s$$；$\frac{L_2}{2}$ 为刺激频率的最高谐波次数或模板倍频数。

**以 SVD 方法为例，CCA 算法的流程为**

1. 计算 $\mathbf{X}$ 的方差 $$\mathbf{S}_{XX}$$，$\mathbf{Y}$ 的方差 $$\mathbf{S}_{YY}$$，以及 $$\mathbf{X}$$ 和 $\mathbf{Y}$ 的协方差 $$\mathbf{S}_{XY}$$
2. 计算矩阵 $$ \mathbf{M} = \mathbf{S}_{XX}^{-1/2}\mathbf{S}_{XY}\mathbf{S}_{YY}^{-1/2}$$
3. 对矩阵 $$\mathbf{M}$$ 进行 SVD 分解，最大的奇异值 $\rho$ 和对应的奇异向量为 $\mathbf{u}$ 和 $\mathbf{v}$，即 $$\displaystyle \rho = \max_{\mathbf{u},\mathbf{v}}\mathbf{u}^\top\mathbf{M}\mathbf{v}$$
4. 计算 $$\mathbf{X}$$ 和 $$\mathbf{Y}$$ 的线性系数向量 $$\mathbf{a} = \mathbf{S}_{XX}^{-1/2}\mathbf{u}$$ 和 $$\mathbf{b} = \mathbf{S}_{YY}^{-1/2}\mathbf{v}$$ -->
 
<!-- 可见算法流程并不复杂，但是要理解这个算法需要了解一些背景知识。

**CCA 算法推导**

在统计学中，**皮尔逊积矩相关系数**（英语：Pearson product-moment correlation coefficient，缩写：PPMCC 或 PCCs，有时简称相关系数）**用于度量两组数据的变量之间的线性相关的程度**。假设有两组一维的数据集 $\mathbf{x}$ 和 $\mathbf{y}$，则 PCCs 定义为

$$
\rho(\mathbf{x},\mathbf{y}) = \frac{\text{cov}(\mathbf{x},\mathbf{y})}{\sqrt{D(\mathbf{x})}\sqrt{D(\mathbf{y})}}
$$

但是，**PCCs不能直接用于高维数据**，高维数据难以计算矩阵相关性的原因有以下几点：
- **维度灾难**：随着维度的增加，数据的样本量通常远小于变量数，样本点在高维空间中变得稀疏。假设有两个变量之间存在一些相关性，但是由于维度灾难，很可能只有少数几个样本点实际上能够展现这种关系。这会导致计算协方差时的样本点不足，产生不准确的估计。而且，高维空间中的任意两个向量之间的夹角较大，相关性的度量也变得不敏感，导致矩阵相关性的估计不稳定，容易受到异常值的影响，而且在低样本量时存在过高偏倚；
- **计算量**：高维数据的矩阵相关性的计算量很大，需要对两个矩阵进行交叉乘积，然后求解特征值和特征向量，这在维数很高时会非常耗时，目前稀疏矩阵的计算仍然是难题。

**CCA 的主要思想是将高维的两组数据分别降维到1维，然后用相关系数分析相关性**。但是有一个问题是，降维的标准是如何选择的呢？回想下主成分分析 PCA，降维的原则是投影方差最大；再回想下线性判别分析 LDA，降维的原则是同类的投影方差小，异类间的投影方差大。**CCA 选择的投影标准是降维后两组数据的 PCCs 最大**。

假设多导 EEG 脑电信号 $$\mathbf{X}\in\mathbb{R}^{L_1\times N}$$，SSVEP 模板信号为 $$\mathbf{Y}\in\mathbb{R}^{L_2\times N}$$，其中 $N$ 为样本个数，而 $L_1, L_2$ 分别为 $\mathbf{X}$ 和 $\mathbf{Y}$ 的特征维度。使用线性系数向量 $\mathbf{a}$ 和 $\mathbf{b}$ 将 $\mathbf{X}$ 和 $\mathbf{Y}$ 投影为一维向量

$$
\mathbf{x} = \mathbf{a}^\top \mathbf{X} \\ 
\mathbf{y} = \mathbf{b}^\top \mathbf{Y}
$$

CCA 的优化目标是最大化相关系数 $$\rho(\mathbf{x}, \mathbf{y})$$ 得到对应的投影向量 $\mathbf{a}$ 和 $\mathbf{b}$

$$
\arg\max_{\mathbf{a},\mathbf{b}}\rho(\mathbf{x}, \mathbf{y})
$$


在投影前，我们一般会把原始数据进行标准化，对于每个特征，计算其平均值和标准差。然后，对每个特征的每个数据点减去平均值后除以标准差，得到 $\mathbf{X}$ 和 $\mathbf{Y}$ 为每个特征的均值为 0 而方差为 1 的数据，即 $E(\mathbf{X}) = \mathbf{0}$， $D(\mathbf{X}) = \mathbf{I}$。

记 $$\mathbf{S}_{XX} = \text{cov}(\mathbf{X},\mathbf{X})$$，$$\mathbf{S}_{YY} = \text{cov}(\mathbf{Y},\mathbf{Y})$$， $$\mathbf{S}_{XY} = \text{cov}(\mathbf{X},\mathbf{Y})$$ 有：

$$
\mathbf{S}_{XX} = \text{cov}(\mathbf{X},\mathbf{X}) = E(\mathbf{X}\mathbf{X}^\top) - E(\mathbf{X})E(\mathbf{X})^\top = E(\mathbf{X}\mathbf{X}^\top)，\\
\mathbf{S}_{YY} = E(\mathbf{Y}\mathbf{Y}^\top), \mathbf{S}_{XY} = E(\mathbf{X}\mathbf{Y}^\top)
$$

于是，可以得到 $\mathbf{x},\mathbf{y}$ 的方差和协方差为

$$
D(\mathbf{x}) = D(\mathbf{a}^\top \mathbf{X}) = \mathbf{a}^\top E(\mathbf{X} \mathbf{X}^\top) \mathbf{a}  = \mathbf{a}^\top \mathbf{S}_{XX} \mathbf{a} \\
D(\mathbf{y}) = D(\mathbf{b}^\top \mathbf{Y}) = \mathbf{b}^\top E(\mathbf{Y} \mathbf{Y}^\top) \mathbf{b}  = \mathbf{b}^\top \mathbf{S}_{YY} \mathbf{b}
$$

$$ 
\begin{aligned}
\text{cov} (\mathbf{x}, \mathbf{y}) &= \text{cov}(\mathbf{a}^\top \mathbf{X}, \mathbf{b}^\top \mathbf{Y})\\
&= E[<\mathbf{a}^\top \mathbf{X}, \mathbf{b}^\top \mathbf{Y}>] 
= E[ (\mathbf{a}^\top\mathbf{X}) (\mathbf{b}^\top \mathbf{Y})^\top] \\
&= \mathbf{a}^\top E(\mathbf{X} \mathbf{Y}^\top) \mathbf{b} 
= \mathbf{a}^\top \mathbf{S}_{XY} \mathbf{b}
\end{aligned}
$$

则优化目标可以转化为：

$$
\arg\max_{\mathbf{a},\mathbf{b}}\frac{\mathbf{a}^\top \mathbf{S}_{XY} \mathbf{b}}{\sqrt{\mathbf{a}^\top \mathbf{S}_{XX} \mathbf{a} \mathbf{b}^\top \mathbf{S}_{YY} \mathbf{b}}}
$$

可以采用和SVM类似的优化方法，固定分母，优化分子，具体的转化为：

$$
\arg\max_{\mathbf{a},\mathbf{b}} \mathbf{a}^\top \mathbf{S}_{XY} \mathbf{b} \\
\text{s.t.}\quad \mathbf{a}^\top \mathbf{S}_{XX} \mathbf{a} = 1, \mathbf{b}^\top \mathbf{S}_{YY} \mathbf{b} = 1
$$

也就是说，算法的目标最终转化为一个凸优化过程，只要我们求出了这个优化目标的最大值，就是我们前面提到的多维 $\mathbf{X}$ 和 $\mathbf{Y}$ 的相关性度量，而对应的 $\mathbf{a},\mathbf{b}$ 则为降维时的投影向量，或者说线性系数。


对于上面的优化目标，我们可以做一次矩阵标准化，就可以用SVD来求解了。首先，我们令 $$\mathbf{a}=\mathbf{S}_{XX}^{-1/2}\mathbf{u}, \mathbf{b}=\mathbf{S}_{YY}^{-1/2}\mathbf{v}$$，有

$$
\mathbf{a}^\top\mathbf{S}_{XX}\mathbf{a} =1 \Rightarrow \mathbf{u}^\top\mathbf{S}_{XX}^{-1/2}\mathbf{S}_{XX}\mathbf{S}_{XX}^{-1/2}\mathbf{u} = 1  \Rightarrow  \mathbf{u}^\top\mathbf{u}=1 \\ 
\mathbf{b}^\top\mathbf{S}_{YY}\mathbf{b} =1 \Rightarrow \mathbf{v}^\top\mathbf{S}_{YY}^{-1/2}\mathbf{S}_{YY}\mathbf{S}_{YY}^{-1/2}\mathbf{v} = 1  \Rightarrow  \mathbf{v}^\top\mathbf{v}=1 \\ 
\mathbf{a}^\top\mathbf{S}_{XY}\mathbf{b} = \mathbf{u}^\top\mathbf{S}_{XX}^{-1/2}\mathbf{S}_{XY}\mathbf{S}_{YY}^{-1/2}\mathbf{v}
$$


也就是说，我们的优化目标变成

$$
\arg\max_{\mathbf{u},\mathbf{v}} \mathbf{u}^\top\mathbf{S}_{XX}^{-1/2}\mathbf{S}_{XY}\mathbf{S}_{YY}^{-1/2}\mathbf{v} \\
\text{s.t.} \quad \mathbf{u}^\top\mathbf{u}=1, \mathbf{v}^\top\mathbf{v}=1
$$

如果将 $\mathbf{u},\mathbf{v}$ 看成矩阵 $$\mathbf{M} =\mathbf{S}_{XX}^{-1/2}\mathbf{S}_{XY}\mathbf{S}_{YY}^{-1/2} $$ 的某一个奇异值左右奇异向量，那么利用奇异值分解，我们可以得到 

$$
 \mathbf{M} = \mathbf{U}\mathbf{\Sigma}\mathbf{V}^\top
$$

其中 $\mathbf{U},\mathbf{V}$ 为正交矩阵，$\mathbf{\Sigma}$ 为对角矩阵，对角线上的元素为奇异值。由于 $\mathbf{U},\mathbf{V}$ 的每一列都是标准正交基，因此 $ \mathbf{u}^\top\mathbf{U} $ 和 $ \mathbf{v}^\top\mathbf{V} $ 会得到一个只有一个标量值为1，其余标量值为0的向量。此时我们有

$$
\mathbf{u}^\top\mathbf{M}\mathbf{v} = \mathbf{u}^\top\mathbf{U}\mathbf{\Sigma}\mathbf{V}^\top\mathbf{v} = \sigma_{uv}
$$

也就是说，将矩阵 $\mathbf{M}$ 做了奇异值分解后，最大的奇异值就是我们优化目标的最大值，或者说我们的 $\mathbf{X}$ 和 $\mathbf{Y}$ 之间的最大相关系数 $\rho$。利用对应的左右奇异向量 $\mathbf{u},\mathbf{v}$ 可以求出原始 $\mathbf{X},\mathbf{Y}$ 线性系数 $$\mathbf{a}=\mathbf{S}_{XX}^{-1/2}\mathbf{u}, \mathbf{b}=\mathbf{S}_{YY}^{-1/2}\mathbf{v}$$。 -->

### 共空间模式（Common Spatial Pattern, CSP）

在脑电信号处理的背景下，考虑两类脑电信号$\mathbf{X}^{(i)} \in \mathbb{R}^{C \times M_i}(i=1,2)$，其中$C$和$M_i$分别表示通道数和采样点数。为了简化问题，假设每个通道的信号均值为零。然后，可以计算两类信号的空间协方差矩阵

$$\mathbf{\hat{R}}^{(i)}=\frac{1}{M_i} \mathbf{X}^{(i)} \mathbf{X}^{(i)^\top}(i=1,2)$$

CSP的任务是找到一种线性变换 $\mathbf{w}$，使得两类之间的方差比最大化。
<!-- $$
\max _{\mathbf{w}} \frac{\mathbf{w}^\top\hat{R}^{(1)} \mathbf{w}}{\mathbf{w}^\top\hat{R}^{(2)} \mathbf{w}} \quad \text { s.t. } \quad\|\mathbf{w}\|=1
$$ -->

数学上，这可以表示为以下优化问题：

$$
\begin{aligned}
\max _{\mathbf{w}} &\quad  \mathbf{w}^\top \mathbf{\hat{R}}^{(1)} \mathbf{w}\\
\text { s.t. } &\quad \mathbf{w}^\top \mathbf{\hat{R}}^{(2)} \mathbf{w} = 1
\end{aligned}
$$


我们可以引入拉格朗日乘子，定义拉格朗日函数：

$$
L(\mathbf{w}, \lambda) = \mathbf{w}^\top \mathbf{\hat{R}}^{(1)} \mathbf{w} - \lambda(\mathbf{w}^\top \mathbf{\hat{R}}^{(2)} \mathbf{w} - 1)
$$

这里，$\lambda$ 是拉格朗日乘子。接下来，我们要最大化这个拉格朗日函数。对于最大化问题，可以通过对拉格朗日函数对 $\mathbf{w}$ 求偏导，然后令其等于零来找到最优解：

$$
\frac{\partial L}{\partial \mathbf{w}} = 2\mathbf{\hat{R}}^{(1)}\mathbf{w} - 2\lambda\mathbf{\hat{R}}^{(2)}\mathbf{w} = 0
$$

整理得到：

$$
\mathbf{\hat{R}}^{(1)}\mathbf{w} = \lambda \mathbf{\hat{R}}^{(2)} \mathbf{w}
$$

有的时候约束条件会写成 $\mathbf{w}^\top \left[\mathbf{\hat{R}}^{(1)}+\mathbf{\hat{R}}^{(2)}\right] \mathbf{w} = 1$，这两个约束条件是等价的，因为方程可以写成

$$
\mathbf{\hat{R}}^{(1)} \mathbf{w} = \lambda' \left[\mathbf{\hat{R}}^{(1)}+\mathbf{\hat{R}}^{(2)}\right] \mathbf{w}, \quad \lambda' = \frac{\lambda}{1+\lambda}
$$

定义矩阵 $\mathbf{W}$ 为包含最优解（特征向量）的列矩阵 $\mathbf{w}$，$\boldsymbol{\Lambda}$为对应的对角矩阵，包含特征值 $\lambda$。上述方程可以写为：

$$
\mathbf{\hat{R}}^{(1)} \mathbf{W} =  \mathbf{\hat{R}}^{(2)}  \mathbf{W} \boldsymbol{\Lambda}
$$

这就是广义特征值问题的标准形式。因此，原始的优化问题可以通过求解这个广义特征值问题来得到最优解 $\mathbf{W}$ 和对应的 $\boldsymbol{\Lambda}$。特征向量 $\mathbf{w}_i$ 变化下两类的方差比等于其相应特征值的分量：

$$
\lambda_i = \frac{\|\mathbf{w}_i^\top \mathbf{X}^{(1)}\|^2}{\|\mathbf{w}_i^\top \mathbf{X}^{(2)}\|^2}
$$


注意，由于不能保证 $\mathbf{\hat{R}}^{(i)}$ 非奇异，因此 $\mathbf{\hat{R}}^{(i)}$ 的逆矩阵可能不存在。因此，我们不能直接将上述方程写成 $\mathbf{\hat{R}}^{(2)-1} \mathbf{\hat{R}}^{(1)}\mathbf{W}= \mathbf{W} \boldsymbol{\Lambda}$，也就是说，不能对$\mathbf{\hat{R}}^{(2)-1} \mathbf{\hat{R}}^{(1)}$进行特征值分解。


等价地，特征向量可以通过协方差矩阵 $\mathbf{\hat{R}}^{(1)}$和 $\mathbf{\hat{R}}^{(2)}$的联合对角化获得(证明见[5])：

$$
\mathbf{W}^\top \mathbf{\hat{R}}^{(i)} \mathbf{W}=\boldsymbol{\Lambda}^{(i)}(i=1,2)
$$



<!-- 脑电图 (EEG) 信号表示大脑的电活动。它们以微伏 (μV) 为单位进行测量。有多种方法可以从这些信号中收集原始数据。从脑电图信号中提取的一些重要频率是 delta（频率范围：0-4  Hz，主要负责深度睡眠、自然治疗和免疫系统）、theta（频率范围：4-8  Hz，代表创造力、情绪状态 、放松）、α（频率范围：8-12  Hz，代表专注和放松的能力）、β（频率范围：12-40  Hz，负责有意识的专注和解决问题的能力）、gamma （频率范围：40–100  Hz，涉及感官结合、认知和学习）。上述频s率代表了脑电图信号成分的主要分类。 -->

<!-- Preprocessing
Covariance Matrix Adaptation Evolution Strategy (CMA-ES)

Feature extraction techniques

Principal Component Analysis (PCA) 
Independent Component Analysis (ICA) 
Fast fourier transform (FFT)
Short Term Fourier Transform (STFT)
Auto Regressive Model (AR)
Wavelet Transform (WT)
Wavelet Packet Transform (WPT)
Wavelet Packet Best Basis Decomposition (WPBBD)
Wavelet Packet Decomposition (WPD)
Power Spectrum Density (PSD)
Common spatial pattern (CSP)
Common Spatio-Spectral Pattern (CSSP)
Common Sparse Spectral-Spatial Pattern (CSSSP)
Sub Band Common Spatial Pattern (SBCSP)
Filter Bank Common Spatial Pattern (FBCSP)
Discriminative FBCSP (DFBCSP)
Separable Common Spatio Spectral Patterns (SCSSP)

Classification techniques

Linear Discriminate Analysis (LDA)
Regularized Fisher LDA
Support vector machine (SVM)
Transition Detection based SVM (TD-SVM)
Neural network (NN)
Multilayer Perceptron (MLP)
Radial Basis Function (RBF)
Online Meta neuron based Learning Algorithm (OMLA)
Deep learning
Deep Belief Network (DBN)
Convolutional Neural Network (CNN) -->


## Ref:

1. Graimann B., Pfurtscheller G., Allison B. Berlin, Heidelberg. Brain-Computer Interfaces: Revolutionizing Human-Computer Interaction[M]. Springer Berlin Heidelberg, 2010.

3. [论文笔记] 高维数据矩阵相关性：[https://zhuanlan.zhihu.com/p/631380041](https://zhuanlan.zhihu.com/p/631380041)

4. 典型关联分析(CCA)原理总结：[https://www.cnblogs.com/pinard/p/6288716.html](https://www.cnblogs.com/pinard/p/6288716.html)

2. 杨晨. 面向应用的稳态视觉诱发电位脑—机接口算法及系统研究[D].清华大学, 2018.

5. CSP共空间模式一些公式的证明：[https://zhuanlan.zhihu.com/p/142635961](https://zhuanlan.zhihu.com/p/142635961)