---
layout: article
title: 安装Anaconda之后的环境变量和VScode设置
---


## 安装Anaconda之后的环境变量设置

> 2023.06 更新:根据官方提示，如果不是必须在 cmd 里面调用 conda，请在安装 anacoda 或 minicoda 之后，**直接在 Anaconda Prompt 中完成操作即可，例如 conda create，pip install 等，添加环境变量不是必要的。**

Python 报错：

```
>> conda activate base
conda : 无法将“conda”项识别为 cmdlet、函数、脚本文件或可运行程序的名称。请检查名称的拼写，如果包括路径，请确保路径正确，然后再试一次。
```

cmd 运行报错：
```
>> conda
'conda' 不是内部或外部命令，也不是可运行的程序
或批处理文件。

>> anaconda 
'anaconda' 不是内部或外部命令，也不是可运行的程序
或批处理文件。
```

解决方法：

第一步：找到Anaconda安装位置。打开Anaconda Prompt，运行命令

```
>> where conda
C:\ProgramData\Anaconda3\Library\bin\conda.bat
C:\ProgramData\Anaconda3\Scripts\conda.exe
C:\ProgramData\Anaconda3\condabin\conda.bat
```

可见Anaconda安装位置为 C:\ProgramData\Anaconda3\

第二步，添加环境变量

此电脑----> 点击右键 ---- > 选择属性 -----> 高级系统设置 ------>环境变量 ------>系统环境变量----->Path----->编辑------>添加

```
C:\ProgramData\Anaconda3\
C:\ProgramData\Anaconda3\Scripts\
C:\ProgramData\Anaconda3\Scripts\Library\bin
```

验证：

cmd 运行 conda list 可见conda可以使用了

```
>> conda list
# packages in environment at C:\ProgramData\Anaconda3:
#
# Name                    Version                   Build  Channel
_ipyw_jlab_nb_ext_conf    0.1.0                    py38_0
alabaster                 0.7.12             pyhd3eb1b0_0
anaconda                  2021.05                  py38_0
anaconda-client           1.7.2                    py38_0
anaconda-navigator        2.1.0            py38haa95532_0
...
```


## 解决VScode终端“无法加载文件 ***\WindowsPowerShell\profile.ps1，因为在此系统上禁止运行脚本”
在VScode使用anaconda时，提示

> . : 无法加载文件 C:\Users\47370\Documents\WindowsPowerShell\profile.ps1，因为在此系统上禁止运行脚本。有关详细信息，请参阅 https:/go.microsoft.com/fwlink/?LinkID=135170 中的 about_Execution_Policies。

想了解计算机上的现用执行策略，打开 PowerShell 然后输入：

```
>> get-executionpolicy
Restricted
```


更改执行策略，以管理员身份打开 PowerShell 输入：

```
>> set-executionpolicy remotesigned
```

![](https://pic4.zhimg.com/v2-e78d392728b08fa3eed95a99b68c2a43_r.jpg)

选择“是”，即可。

如果要更改回Windows 客户端计算机的默认执行策略，则设置为restricted：

```
set-executionpolicy restricted
```
