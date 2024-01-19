---
layout: article
title: 脑机接口研究基础
mathjax: True
key: Brain-Computer-Interface-Review

---

<!--more-->




<img src="/pictures/BCI/大脑皮层.png" alt="大脑皮层" width="600">

大脑分为四个叶：额叶、顶叶、枕叶和颞叶。中央沟将额叶与顶叶分开。它还分离中央前回（红色表示）和中央后回（蓝色表示）。颞叶和额叶被侧裂分开。枕叶位于大脑的后部。以下皮层区域对脑机接口特别重要：**运动区、体感皮层、后顶叶皮层和视觉皮层。**


## 脑机接口分类[^Brain-Computer]

[^Brain-Computer]: Graimann B., Pfurtscheller G., Allison B. Berlin, Heidelberg. Brain-Computer Interfaces: Revolutionizing Human-Computer Interaction[M]. Springer Berlin Heidelberg, 2010.

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


### 基于 SSVEP 的脑机接口[^Yang]

[^Yang]: 杨晨. 面向应用的稳态视觉诱发电位脑—机接口算法及系统研究[D].清华大学, 2018.

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
