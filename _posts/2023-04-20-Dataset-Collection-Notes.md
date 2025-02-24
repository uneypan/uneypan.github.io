---
layout: article
title: 节拍数据集收集记录
mathjax: true
---

## 节拍数据集

1.SDPH 数据集

SDPH (Spotify Dance Pop Hits) 是一个自建数据集，选自 Spotify 的 Dance Pop Hits 歌单，主要包含英文流行舞曲113曲，年份覆盖 2010 至 2022 年。统一截取前3分钟音频，按照 songbpm 网站 (https://songbpm.com/) 给出的速度，由人工标注所有节拍时刻。由于版权问题，该数据集不会公开，仅用于研究。

2.GZTAN 数据集

GZTAN音乐数据集是一个用于音乐分类和音乐信息检索的重要数据集，被广泛应用于音乐分类、音乐信息检索、音乐推荐等领域的研究和实践中。

它包含了1000首歌曲，每首歌曲的长度为30秒，共包含10个音乐类别，每个类别包含100首歌曲。这些类别包括：布鲁斯(blues)、古典(classical)、乡村(country)、迪斯科(disco)、嘻哈(hiphop)、爵士(jazz)、金属(metal)、流行(pop)、雷鬼(reggae)和摇滚(rock)。

这些文件是在 2000-2001 年从各种来源收集的，包括个人 CD、收音机、麦克风录音，以代表各种录音条件。该数据集的音频文件格式为WAV，采样率为22050Hz，每个样本的长度为661,500个采样点。此外，该数据集还提供了每个样本的元数据，包括歌曲名称、艺术家、年份和流派等信息。

原始数据下载地址（不可用）：

http://marsyas.­info/­download/­data_sets   
http://opihi.cs.uvic.ca/sound/genres.tar.gz 

Kaggle镜像（可用）：

https://www.kaggle.com/datasets/andradaolteanu/gtzan-dataset-music-genre-classification

Marchand 和 Peeters 对 GZTAN 进行了标注，提供了节拍和节奏信息(速度，小节，节拍，tatum，swing ratio，...) ，可以在这里下载此标注集，其中的音频下载脚本已经失效：

http://anasynth.ircam.fr/home/media/GTZAN-rhythm/

使用此数据标注需要引用[1].
 
3.Ballroom 数据集

该数据集包括它不包括音频文件。音频文件（698个文件，大小为1.5 gb）
原始数据集下载：

http://mtg.upf.edu/ismir2004/contest/tempoContest/node5.html （最新于2013年12月访问，目前不可用）。

请使用

http://mtg.upf.edu/ismir2004/contest/tempoContest/data1.tar.gz

Md5sum校验: 2872a3e52070bc342a4510a95e2fa0b8

由 Gouyon 等人引入的舞厅数据集的节拍和小节标注[2]，目前托管在 GitHub 上：

https://github.com/CPJKU/BallroomAnnotations

注：在这个标注里面，不包含任何音乐内容的前奏没有标注，评估时应该跳过。

4.SMC 数据集

原始下载链接：http://smc.inescporto.pt/data/SMC_MIREX.zip

镜像主页： https://joserzapata.github.io/project/smc-beat-tracker-dataset/

镜像下载链接： https://bit.ly/33SlutJ


## 节拍跟踪系统

1.IBT 节拍跟踪系统

IBT 是由C++ 编写的音乐音频信号的实时节拍跟踪系统[8]，是免费框架MARSYAS的一部分。  

IBT 集成在 MARSYAS 框架 (http://marsyas.info/) ，这也是论文提到的官方实现。 源代码托管在 github, 需要编译才可以使用:  https://github.com/marsyas/marsyas  
由于其文档网站已经年久失修，我们使用托管在sourceforge的预编译可执行程序：  
https://sourceforge.net/projects/marsyas/files/  

采用其中一个 Windows 平台的 release 版本(2014-01-28)  ```marsyas-win32-0.5.0-alpha``` ，于是可以利用 python 的 subprocess 模块调用命令行来执行 `/bin/ibt.exe` 得到结果。 

IBT 的应用实例 —— 机器人 RoboNova: https://www.youtube.com/watch?v=DWKRZJdn1JU  
实现细节见[3],[4]。 

2.LibROSA
Librosa是一个用于音频、音乐分析、处理的python工具包，一些常见的时频处理、特征提取、绘制声音图形等功能。其内置了节拍跟踪算法基于动态规划[12]。

3.Madmom

Madmom: https://github.com/CPJKU/madmom 是一个Python音乐信号处理库，用于分析音频信号并提取有关音乐的信息。它提供了许多功能，包括节奏分析、音高估计、音频分割、节拍检测、音频特征提取等。Madmom库的应用非常广泛，包括音乐信息检索、音乐自动分类、音乐自动标注、音乐自动转录等。

Madmom默认算法使用循环神经网络（RNN）和一个由隐马尔可夫模型（HMM）近似的动态贝叶斯网络（DBN）来进行节拍跟踪。DBN是一种用于建模时间序列数据的概率图模型，而HMM是一种常用的DBN近似方法。

其 bin/ 目录还提供了一些api文档中没有的算法实现：

- CRFBeatDetector[5]  
- 动态贝叶斯网络 DBNBeatTracker[6] 注：状态空间和传递模型采用[7]
- 多模型融合 MMBeatTracker[6] 
- 时域卷积网络 TCNBeatTracker[9] 
- BeatTracker[10] 
- BeatDetector[10] 

4.pdaf
之前编写的基于卡尔曼滤波和概率数据关联滤波的的节拍跟踪方案。


## 精度评估实验


使用 Python 编写的评估脚本，可以调用各种算法的二进制脚本或python脚本得到结果。评估算法主要采用的是 madmom 库的 evaluation.beats.BeatEvaluation 类做精度计算，可以求解多个通用精度指标。实验结果主要看召回率，准确度， F1 精度和 P 指标，这几个指标的定义可以见[11]。

| Model  | Dataset | Recall | Precision | F-measure | P-score |   time   |  casual  |
|--------|---------|--------|-----------|-----------|---------|----------|----------|
| ibt     | GTZAN   | 0.760  | 0.751     | 0.740     | 0.746   | 11:22   | √        |
| librosa | GTZAN   | 0.780  | 0.726     | 0.730     | 0.694   | 10:20   | ×        |
| madmom  | GTZAN   | 0.893  | 0.875     | 0.871     | 0.860   |1:16:40 | ×        |
| pdaf    | GTZAN   | 0.532  | 0.528     | 0.516     | 0.601   | 10:42   | √        |

| Model  | Dataset | Recall | Precision | F-measure | P-score |   time   |  casual  |
|-------- |---------|--------|-----------|-----------|---------|---------|----------|
| ibt     | Ballroom| 0.754  | 0.707     | 0.713     | 0.701   | 06:20   | √        |
| librosa | Ballroom| 0.782  | 0.665     | 0.699     | 0.659   | 05:17   | ×        |
| madmom | Ballroom| 0.901  | 0.933     | 0.906     | 0.865   | 50:28   | ×        |
| pdaf    | Ballroom| 0.457  | 0.432     | 0.432     | 0.526   | 07:48   | √        |

| Model  | Dataset | Recall | Precision | F-measure | P-score |   time   |  casual  |
|-------- |---------|--------|-----------|-----------|---------|---------|----------|
| ibt     | SDPH    | 0.755  | 0.752     | 0.753     | 0.770   | 07:28   | √        |
| librosa | SDPH    | 0.716  | 0.711     | 0.712     | 0.718   | 07:02   | ×        |
| madmom | SDPH    | 0.958  | 0.961     | 0.958     | 0.956   | 58:56   | ×        |
| pdaf    | SDPH    | 0.466  | 0.473     | 0.466     | 0.580   | 06:48   | √        |


# 参考文献

[1] U. Marchand, G. Peeters. “Swing Ratio Estimation” in Proc. of the 18th Int. Conference on Digital Audio Effects (DAFx-15), December 2015.

[2] Gouyon F. A. et al. An experimental comparison of audio tempo induction algorithms. 

[3] Santiago, Catarina B., et al.  Autonomous robot dancing synchronized to musical rhythmic stimuli. 

[4] Oliveira, João Lobato, et al.  Beat tracking for interactive dancing robots. 

[5] Filip Korzeniowski, Sebastian Böck and Gerhard Widmer. Probabilistic extraction of beat positions from a beat activation function. 

[6] Sebastian Böck, Florian Krebs and Gerhard Widmer. A Multi-Model Approach to Beat Tracking Considering Heterogeneous Music Styles. 

[7] Florian Krebs, Sebastian Böck and Gerhard Widmer. An Efficient State Space Model for Joint Tempo and Meter Tracking. 

[8] Oliveira, Joao Lobato, et al.  IBT: A Real-time Tempo and Beat Tracking System.  ISMIR. 2010.

[9] Sebastian Böck, Matthew Davies and Peter Knees. Multi-Task learning of tempo and beat: learning one to improve the other. 

[10] Sebastian Böck and Markus Schedl. Enhanced Beat Tracking with Context-Aware Neural Networks 

[11] Matthew E, et al, Evaluation Methods for Musical Audio Beat Tracking Algorithms.

[12] Ellis, Daniel PW. “Beat tracking by dynamic programming.” Journal of New Music Research 36.1 (2007): 51-60.