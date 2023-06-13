---
layout: article
title: 解决“无法加载文件 ***\WindowsPowerShell\profile.ps1，因为在此系统上禁止运行脚本”
mathjax: false
---

解决“无法加载文件 ***\WindowsPowerShell\profile.ps1，因为在此系统上禁止运行脚本”
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

