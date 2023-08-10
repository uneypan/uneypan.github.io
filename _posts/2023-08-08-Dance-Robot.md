---
layout: article
title: 节拍驱动的舞蹈四足机器人ANYmal
mathjax: true
article_header:
  type: overlay
  theme: dark
  background_color: '#203028'
  background_image:
    gradient: 'linear-gradient(0deg,rgba(0, 0, 0, .5),  rgba(255, 255, 255 , .0))'
    src:  /pictures/ANYmal.png
---



> Bi T, Fankhauser P, Bellicoso D, etc. Real-Time Dance Generation to Music for a Legged Robot[C]. 2018 IEEE RSJ International Conference on Intelligent Robots and Systems (IROS). 2018: 1038-1044.


<!-- <img src=/pictures/ANYmal.png width=400> -->

<div>{%- include extensions/youtube.html id='kHBLaw5nfzk' -%}</div>

YouTube: [https://www.youtube.com/watch?v=kHBLaw5nfzk](https://www.youtube.com/watch?v=kHBLaw5nfzk)

<style>
    .mystyle {
        margin: 50px 0 0 0;
        width: 400px;
        display: block;
        margin: 0 auto;
    }
</style>



## 基本结构
ANYmal用机载麦克风收听现场音乐，提取当前节拍，并用腿和底座产生不同的舞蹈动作。在线延迟补偿使得动作与感知到的音乐同步。
<div class="mystyle">
    <img src="/pictures/ANYmalStructure.png">
</div>


## 节拍提取

机载麦克风获取现场音乐之后，由 [madmom](https://github.com/CPJKU/madmom) 库提取音乐节拍 


并计算每个节拍之间的间隔 

$$\tilde{d}_{n} = \tilde{b}_n-\tilde{b}_{n-1}$$


然后，作者用过去最近的3个节拍间隔外推未来最近时刻的节拍时刻

$$
b_{n+1} = \tilde{b}_n + \text{median}(\tilde{d}_n,\tilde{d}_{n-1},\tilde{d}_{n-2})
$$


## 舞蹈动作设计与编排

### 舞蹈动作元语言设计

设计舞蹈动作元语言以建立动作库时，需要考虑以下几点。这些动作应该通过其运动来表达节奏，并为舞蹈编舞者提供信息，以评估舞蹈动作在当前音乐中的适应性。因此，针对每个舞蹈动作 $x_i$，指定了以下特征和设计参数。

- 持续时间：每个舞蹈动作元语言的持续时间连续设置为当前节拍持续时间 $d_n$ 的整数倍。

- 轨迹振幅：通过将某些舞蹈动作的振幅（例如，跺脚高度）以节拍持续时间线性缩放，可以表达音乐的情绪。

<div class="mystyle">
    <img src="/pictures/freeGaid.png">
</div>

- 节奏范围：节奏范围 $$ [\text{bpm}_{min,i}, \text{bpm}_{max,i}] $$ 定义了允许执行舞蹈动作 $x_i$ 的节奏限制。

- 重复次数：为每个舞蹈动作分配一个重复次数 $r_i$ 。

- 特征信号：每个舞蹈动作定义了一个特征信号。这个信号应该分别代表音乐同步或异步的行为。例如，跺脚那只脚的接触信号、底座相对基准位置和 y 方向上的速度都是适合的特征信号。

- 过渡行为：理想情况下，过渡行为无缝地连接两个舞蹈动作，不会破坏舞蹈的流畅性，同时在整个过渡过程中保持稳定。通过将具有相似起始和结束姿势的舞蹈动作分组来实现。每当从一组切换到另一组时，将从上一个舞蹈动作的结束姿势开始，并以下一个舞蹈动作的开始姿势结束的轨迹发送到机器人。这个轨迹的持续时间也与当前的节奏缩放。


- 轨迹生成：最后，每个舞蹈动作都必须定义为一个连续的轨迹，可以传递给全身控制器。为了生成这样的轨迹，采用基于关键帧的设计，并通过 [FreeGait](https://www.researchgate.net/publication/312111333_Free_Gait_-_An_architecture_for_the_versatile_control_of_legged_robots) 使用五次多项式样条拟合。起始关键帧由当前机器人状态给定。



### 舞蹈编排




马尔可夫链作为选择合适舞蹈动作的模型。随机变量 $X_{n+1}$ 表示在节拍 $n+1$ 处选择的舞蹈动作。$X_{n+1}$为动作 $x_i$ 的概率仅取决于先前拾取的舞蹈运动 $X_n=x_j$ 和当前节奏（以bpm为单位）。

<div class="mystyle">
    <img src="/pictures/AMYalMarkovDanceSelection.png">
</div>

为每个舞蹈动作 $x_i$ 赋予一个权重
$$
w_{i,n+1} = R_i B_{i,n+1}
$$

其中，重复因子是
$$
R_i = \begin{cases}r_i \quad \text{if} \quad i=j \\ 1 \quad \text{else}\end{cases}
$$

速度范围因子是

$$
B_{i,n+1} = \begin{cases}1 \quad \text{if} \quad \text{bpm}_{min,i} \leq \text{bpm}_{n+1} \leq \text{bpm}_{max,i} \\ 0 \quad \text{else}\end{cases}
$$

那么下一个状态的概率分布为

$$
P(X_{n+1}=x_i|X_n=x_j) = \frac{w_{i,n+1}}{\sum_{k=1}^N w_{k,n+1}}
$$







## 舞蹈与音乐的同步


### 延迟估计

机器人同步系统的工作原理，主要分为三个步骤：

<div class="mystyle">
    <img src="/pictures/AMYmalDelayCompensation.png">
</div>

1. 参考信号：首先，需要定义一个与节奏同步的运动作为参考信号，通过Free Gait接口实现。利用来自即将执行的舞蹈动作的关键帧姿势（这些姿势与当前的节奏周期相匹配），模拟所需的机器人状态，持续模拟舞蹈动作的时间长度，从而得到一个与节奏同步的机器人状态参考信号。如前所述，同步度的度量基于每个舞蹈动作单独定义的特征信号。这个特征信号从模拟的机器人状态中提取出来，将作为参考信号。

2. 实际测量信号：实际要同步的信号由机器人状态估计器提供。与参考信号类似，测量的特征信号将在整个运动过程的持续时间内充当实际信号，将与参考信号进行比较。

3. 延迟估计：最后、也是最关键的一步是找到参考信号和实际信号之间的时间延迟。为了解释起见，将参考信号表示为 $f$，实际信号表示为 $g$。算法的基本思想是在时间上相对于彼此将 $f$ 和 $g$ 进行移动，对于每个时间偏移 $\tau$，通过最小二乘法（LSE）定义两个信号的相似程度。因此，LSE 被定义为时间偏移的函数

$$
||f-g ||^2_2 (\tau) = \frac{1}{T-|\tau|}\int_{\text{max}(0,-\tau)}^{\text{min}(T,T-\tau)} (f(t)-g(t+\tau))^2 dt
$$

$$
\delta_n=\text{arg min}_{\tau}|| f-g ||^2_2 (\tau)
$$





### 延迟补偿


下一个舞蹈动作的开始时间设置为

$$
t_{\text{start},n+1}= b_{n+1} - u_n                     
$$

使用组合的 PID 反馈和前馈控制器来控制延迟。在第 $n$ 拍的控制动作由下式给出

$$
u_n = u_{n,\text{feedback}} + u_{n,\text{feedforward}}
$$


a. 反馈控制器

$$
u_{n,\text{feedback}} = K_p \delta_n + K_i \sum_{m=1}^n \delta_m d_m+ K_d \frac{\delta_n-\delta_{n-1}}{d_n}
$$


b. 前馈控制器

在参考信号发生变化之后，例如由于音乐节奏的变化，纯反馈控制器通常需要一定的时间来收敛。因此，我们引入一个由查找表给出的前馈项，理想情况下它将收敛时间归零。假设不同的节奏和舞蹈动作的延迟是不同的，因为舞蹈动作的节奏和类型都会影响机器人的速度及其惯性，进而影响机器人的跟踪性能。因此，查找表的维度是 (1) 舞蹈动作和 (2) bpm，其中 bpm 被划分为用户定义的 $B$ bpm 分辨率的网格。此外，我们选择实现动态查找表，而不是静态查找表。这种设计选择允许改变机器人动力学，例如新的机器人控制器参数、电机等。

每当当前控制动作成功同步特定舞蹈动作和节拍时，即当检测到低于用户定义阈值 $\delta_n < \theta$ 的延迟时，动态查找表值 $l_{i,[bpm]} $ 就会更新。然后，查找表值由先前 $M$ 个控制动作的移动平均值给出，其中舞蹈动作 $x_i$ 的延迟低于上述阈值：

$$
l_{i,[bpm]} = \frac{1}{\text{min}(M,m)} \sum_{j=\text{max(1,m-M+1)}}^m u_{0,i,j}
$$

$$
u_{0,i,m} = u_n \quad \text{if} \quad \delta_n < \theta
$$

在进行实验时，查找表不断被填充，其中包括模拟的内部节拍和来自节拍跟踪器的节拍。图 6 显示了两种不同舞蹈动作的查找表值。显然，较高的tempo会导致较高的补偿值。这可以用较高tempo下机器人速度的增加以及全身控制器跟踪更快运动的固有困难来解释。此外，不同舞蹈动作的补偿值也不同。此外，跺脚动作的延迟是负的，这意味着在没有控制动作的情况下动作过早完成。这是由于机器人底座的高惯性使得底座运动比较轻的腿部运动更难跟踪，其次，由于跺脚运动的末端位置设置在地板下方，因此末端执行器具有很高的冲击力。


$$
u_{n,\text{feedforward}} = l_{i_n}[\text{bpm}_n]
$$

其中，$\text{bpm}_n = \frac{60s}{d_n}$

<div class="mystyle">
    <img src="/pictures/ANYmalDelay.png">
</div>




## 结果

<div width = 800px>
    <img src="/pictures/ANYmalControl.png" >
</div>

实验在四足机器人 [ANYmal](https://www.researchgate.net/publication/311758153_ANYmal_-_a_highly_mobile_and_dynamic_quadrupedal_robot) 上进行，该机器人高 0.5 m，重 30 kg。它的每条腿和点脚均具有三个扭矩可控的驱动关节，并配备了接触传感器。选择哺乳动物的关节布置，具有连续的髋部外展/内收、髋部屈曲/伸展和膝部屈曲/伸展，配备了采样率为 44100 Hz 的单声道麦克风。

在这个作品的最后，我们一共设计了7个舞蹈动作，包括底座的左右移动、左右脚两种不同的跺脚动作以及扭臀舞蹈动作。系统输出的最终轨迹作为全身控制器的参考信号，该控制器以 400 Hz 的速率与驱动器通信。


<div class="mystyle">
    <img src="/pictures/ANYmalExternalDisturbances.png">
</div>

图9说明了实验结果。机器人在 14 秒时开始固定，并在 16.5 秒时释放。释放机器人后，系统会再执行一次完整的左右运动，然后检测到干扰引起的延迟。因此，下一个运动由 PID 控制器及时向后移动，并且机器人能够在一个完整的运动内返回到其同步状态。