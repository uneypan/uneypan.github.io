---
layout: article
title: 四足机器人ANYmal节拍驱动舞蹈
mathjax: true
article_header:
  type: overlay
  theme: dark
  background_color: '#203028'
  background_image:
    gradient: 'linear-gradient(0deg,rgba(0, 0, 0, .5),  rgba(255, 255, 255 , .0))'
    src:  /pictures/ANYmal.png
---



> Bi T, Fankhauser P, Bellicoso D, etc. Real-Time Dance Generation to Music for a Legged Robot[C]//2018 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS). 2018: 1038-1044.


<!-- <img src=/pictures/ANYmal.png width=400> -->

[YouTube: Robot ANYmal Dancing to Live Music](https://www.youtube.com/watch?v=kHBLaw5nfzk)






## 基本结构
ANYmal用机载麦克风收听现场音乐，提取当前节拍，并用腿和底座产生不同的舞蹈动作。在线延迟补偿使得动作与感知到的音乐同步。
<div><img src=/pictures/ANYmalStructure.png width=400 align=right><div>

## 节拍提取
机载麦克风获取现场音乐之后，由madmom库提取音乐节拍 $\tilde{b}_1,\tilde{b}_2,\dots,\tilde{b}_n$，并计算每个节拍之间的间隔 $\tilde{d}_n = \tilde{b}_n-\tilde{b}_{n-1}$。然后，作者用最近的三个节拍外推未来最近时刻的节拍时刻

$$
b_{n+1} = \tilde{b}_n + \text{median}(\tilde{d}_n,\tilde{d}_{n-1},\tilde{d}_{n-2})
$$


## 舞蹈动作设计与编排

## 舞蹈动作元语言设计

设计舞蹈动作元语言以建立动作库时，需要考虑以下几点。这些动作应该通过其运动来表达节奏，并为舞蹈编舞者提供信息，以评估舞蹈动作在当前音乐中的适应性。因此，针对每个舞蹈动作 $xi$，指定了以下特征和设计参数。

1. 舞蹈动作持续时间：每个舞蹈动作元语言的持续时间连续设置为当前节拍持续时间 $d_n$ 的整数倍。

2. 轨迹振幅：通过将某些舞蹈动作的振幅（例如，跺脚高度）以节拍持续时间线性缩放，可以表达音乐的情绪。

3. 节奏范围：节奏范围 $[\text{bpm}_{min,i}, \text{bpm}_{max,i}]$ 定义了允许执行舞蹈动作 $xi$ 的节奏限制。

4. 重复次数：为每个舞蹈动作分配一个重复次数 $r_i$ 。

5. 特征信号：每个舞蹈动作定义了一个特征信号。这个信号应该分别代表音乐同步或异步的行为。例如，与节拍一起跺脚的同步性最好通过脚与地面接触的瞬间来表现出来。因此，跺脚那只脚的接触信号被选择为特征信号。对于底座在 y 方向上的侧向运动，相对基准位置和 y 方向上的速度都是适合的特征信号。

6. 过渡行为：理想情况下，过渡无缝地连接两个舞蹈动作，不会打扰舞蹈的流畅性，同时在整个过渡过程中保持稳定。这通过将具有相似起始和结束姿势的舞蹈动作分组来实现。每当从一组切换到另一组时，将从上一个舞蹈动作的结束姿势开始，并以下一个舞蹈动作的开始姿势结束的轨迹发送到机器人。这个轨迹的持续时间也与当前的节奏缩放。

7. 轨迹生成：最后，每个舞蹈动作都必须定义为一个连续的轨迹，可以传递给全身控制器。为了生成这样的轨迹，我们采用了基于关键帧的设计，并通过 Free Gait$^{1}$ 软件使用五次多项式样条拟合。起始关键帧由当前机器人状态给定。

> P. Fankhauser, D. Bellicoso, C. Gehring, and M. Hutter, “Free Gait An Architecture for the Versatile Control of Legged Robots,” IEEERAS International Conference Humanoid Robots (Humanoids), 2016.

<div><img src=/pictures/freeGaid.png width=400 align=right>

### 舞蹈编排

<div><img src=/pictures/AMYalMarkovDanceSelection.png width=400 align=right>

马尔可夫链作为选择合适舞蹈动作的模型。随机变量 $X_{n+1}$ 表示在节拍 $n+1$ 处选择的舞蹈动作。$X_{n+1}$为动作 $x_i$ 的概率仅取决于先前拾取的舞蹈运动 $X_n=x_j$ 和当前节奏（以bpm为单位）。

## 舞蹈与音乐的同步

$$
||f-g ||^2_2 (\tau) = \frac{1}{T-|\tau|}\int_{\text{max}(0,-\tau)}^{\text{min}(T,T-\tau)} (f(t)-g(t+\tau))^2 dt
$$

$$
\delta_n=\text{arg min}_{\tau}||f-g ||^2_2 (\tau)
$$


$$
t_{\text{start},n+1}= b_{n+1} - u_n
$$

$$
u_n = u_{n,\text{feedback}} + u_{n,\text{feedforward}}
$$

$$
u_{n,\text{feedback}} = K_p \delta_n + K_i \sum_{m=1}^n \delta_m d_m+ K_d \frac{\delta_n-\delta_{n-1}}{d_n}
$$

$$
l_{i,[bpm]} = \frac{1}{\text{min}(M,m)} \sum_{j=\text{max(1,m-M+1)}}^m u_{0,i,j}
$$

$$
u_{0,i,m} = u_n \quad if \quad \delta_n < \theta
$$

<div><img src=/pictures/AMYmalDelayCompensation.png width=400 align=right><div>


<div><img src=/pictures/ANYmalDelay.png width=400 align=right><div>

<div><img src=/pictures/ANYmalControl.png width=400 align=right><div>



