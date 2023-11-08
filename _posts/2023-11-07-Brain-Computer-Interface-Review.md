## 脑-机接口分类

- 皮层脑电(electrocorticography, ECoG)
- 局部场电位(local field potential, LFP)
- 功能近红外光谱（functional near-infrared spectroscopy, fNIRS）
- 功能磁共振成像(functional magnetic resonance imaging, fMRI)
- 脑电图(electroencephalograph, EEG)


## EEG 脑-机接口

基于感觉运动节律的脑-机接口

当事件发生时，脑电信号中与特定神经活动相关的部分频带能量发生衰减的现象被称为事件相关同步(event-related synchronization，ERS)/去同步(event-related desynchronization，ERD)。用户想象某侧肢体运动时，其对侧感觉运皮层的脑电在 mu 节律（频带在 8-13 Hz ）和 beta 节律（频带在 18-26 Hz ）能量上会出现 ERD 现象。

基于 P300 信号的脑-机接口

P300 成分是一种大脑对新奇刺激的特异性响应成分。**在新奇刺激呈现后约 300ms，大脑中央区及顶区的电极可以采集到一个特异性的正成分， 这种正成分就被称为 P300 成分** 。快速连续视觉呈现（rapid serial visual presentation, RSVP）是一类基于 P300 信号的实验范式。

基于稳态视觉诱发电位的脑-机接口

大脑视皮层对视觉刺激产生的响应被称为视觉诱发电位(visual evoked potential, VEP)，这些响应信号通过颅骨传导到头皮表面，并且在传导过程中相互混叠，从而形成稳态视觉诱发电位(steady-state visual evoked potential，SSVEP)。图形变化都可能会诱发 VEP，例如闪光刺激，图形翻转，光栅变化等。EEG 记录数据中，视觉诱发电位通常在视觉皮层响应最为强烈，因此采集视觉诱发电位信号时通常需要包括枕叶区。

![](/pictures/BCI/视觉感知觉通路.PNG)

VEP 与视觉刺激的空间和时间特性密切相关。当视觉刺激出现在中央视野区，通常会比刺激出现在外周视野区引起幅度更加显著的视觉诱发电位，同时，视觉诱发电位也与视觉刺激出现的时刻具有较强的相关性，反映在信号特征上即为锁时、锁相特点。 

稳态视觉诱发电位(SSVEP)是一类特殊的视觉诱发电位。它由具有固定频率的视觉刺激所诱发，当视觉刺激以特定频率进行周期性呈现时（如闪烁、图形翻转、图形放缩等），视觉系统会受其影响产生出具有稳定频率特征的诱发响应。这种诱发响应通常包含与刺激频率相同的频率成份及其高次谐波成份。
![](/pictures/BCI/典型%20SSVEP%20响应信号波形及频谱.PNG)