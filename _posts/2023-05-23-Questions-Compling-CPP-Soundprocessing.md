---
layout: article
title: 编译 NAO 机器人 C++ 例程 soundprocessing 模块遇到的问题
mathjax: true
---


# 编译 NAO 机器人 C++ 例程 soundprocessing 模块遇到的问题

## 1.准备工作

为了交叉编译本地模块，使用的操作系统是 Ubuntu 16.04.7 LTS (Xenial Xerus) - 64bits 。 

测试机器不是虚拟机，没有使用代理。

NAO 机器人已经正常开机，处于自主生活模式，和计算机处于同一局域网下。

环境配置完全参考了 

http://doc.aldebaran.com/2-8/dev/cpp/install_guide.html

C++ SDK 和 Python SDK 都配置完全且可以使用，检验的方法是在本机编译了资料光盘的实例代码，/设备随装资料-23.3/3-NAO机器人/3-技术资料/示例代码/C++/sayhelloworld.zip，它调用了机器人自带的模块。

编译成功后，在终端执行
```bash
cd /build-sys-linux_x86_64/sdk/bin
./sayhelloworld 192.168.3.19
```
结果是机器人说出了 hello world。我认为C++程序编译成功了，可以远程执行，因此工具链配置也是正确的，后面的编译过程类似。


## 2.soundprocessing 模块编译过程：
源代码来自提供的资料光盘：/设备随装资料-23.3/3-NAO机器人/3-技术资料/示例代码/C++/soundprocessing.zip 。在线手册说明了，这是一个远程/本地模块。
解压压缩包之后，在 soundprocessing 目录中执行配置：
```bash
$ qibuild configure
```
提示生成编译目录 /build-sys-linux_x86_64。

然后执行 `qibuild make` 编译，输出结果
```bash
...(报很多warnings，限于篇幅就不贴了)

[100%] Linking CXX executable sdk/bin/soundprocessing
[100%] Built target soundprocessing
```
提示已经编译成功，产生一个 sdk/bin/soundprocessing 可执行文件。
## 3.尝试执行程序
进入 /build-sys-linux_x86_64/sdk/bin， 在终端运行这个可执行文件:
```bash
cd /build-sys-linux_x86_64/sdk/bin
./soundprocessing
```
报错：
```bash
[I] 1684826244.445762 4667 core.common.toolsmain: ..::: starting alsoundprocessing :::..

[I] 1684826244.445804 4667 core.common.toolsmain: Connecting to 127.0.0.1:9559...

[I] 1684826244.449202 4667 qimessaging.session: Session listener created on 
tcp://0.0.0.0:0
[I] 1684826244.449651 4667 qimessaging.transportserver: TransportServer will listen on: 
tcp://127.0.0.1:34389
[I] 1684826244.449671 4667 qimessaging.transportserver: TransportServer will listen on: 
tcp://192.168.3.20:34389
[E] 1684826244.450429 4667 core.common.toolsmain: Failed to connect to broker at 
127.0.0.1:9559

[E] 1684826244.450454 4667 core.common.toolsmain: alsoundprocessing exit


```
或者使用
```bash
./soundprocessing 192.168.3.19 或
./soundprocessing 192.168.3.19:9559
```
报错也是类似的。暂时不清楚这个错误的含义。
示例代码的文件夹还提供了一个python脚本，意思应该是测试这个声音处理模块，于是尝试执行这个脚本。
/soundprocessing/MySoundProcessingModuleTest.py
```python

# -*- coding: utf-8 -*-
"""
  UnitTests for ALSoundProcessing Module

"""

from naoqi import ALProxy

IP = "192.168.3.19" # 我的NAO机器人IP
PORT = 9559

if __name__ == "__main__":

    proxy = ALProxy("ALSoundProcessing", IP, PORT)
    proxy.subscribe("MyModule")
```
注意这里的IP地址已经改成NAO的IP，在 /soundprocessing/tests/ 目录下，执行提供的测试脚本，结果似乎提示找不到自定义的 ALSoundProcessing
```bash
$ python ./MySoundProcessingModuleTest.py 
[W] 1684757167.823390 25761 qi.path.sdklayout: No Application was created, trying to deduce paths
[I] 1684757167.825496 25761 qimessaging.session: Session listener created on 
tcp://0.0.0.0:0
[I] 1684757167.825802 25761 qimessaging.transportserver: TransportServer will listen on:
 tcp://127.0.0.1:32769
[I] 1684757167.825814 25761 qimessaging.transportserver: TransportServer will listen on:
 tcp://192.168.3.20:32769
Traceback (most recent call last):
  File "./MySoundProcessingModuleTest.py", line 14, in <module>
    proxy = ALProxy("ALSoundProcessing", IP, PORT)
  File "/home/nao/Downloads/pynaoqi/lib/python2.7/site-packages/naoqi.py", line 237, in __init__
    inaoqi.proxy.__init__(self, args[0], args[1], args[2])
  File "/home/nao/Downloads/pynaoqi/lib/python2.7/site-packages/inaoqi.py", line 464, in __init__
    this = _inaoqi.new_proxy(*args)
RuntimeError: 	ALProxy::ALProxy
	Can't find service: ALSoundProcessing

```
## 4. 继续尝试编译本地模块
由于远程执行报错找不到定义的 ALSoundProcessing 模块，因此尝试了编译本地模块的办法。
由于 2-5、2-8 版本的在线手册都没有提到本地模块的编译方法，这里参考了 2-1 版本手册的内容：http://doc.aldebaran.com/2-1/dev/cpp/tutos/create_module.html，模块的类型是远程还是本地，似乎是由 CMakeList.txt 里面的宏定义 XXXX_IS_REMOTE 决定的。

将 CMakeList.txt 中第7行
```cmake
option(SOUNDPROCESSING_IS_REMOTE
  "module is compiled as a remote module (ON or OFF)"
  ON)
```
中的 `ON` 改为 `OFF`，然后再次编译

```bash
qibuild configure
qibuild make 
```
提示生成了库文件 sdk/lib/naoqi/libsoundprocessing.so

然后用ssh拷贝整个/soundprocessing目录到NAO机器人中，存放目录为 /home/nao/test_cpp_202305

然后修改 /home/nao/naoqi/preferences/autoload.ini ，添加 libsoundprocessing.so 的完整路径
```bash
/home/nao/test_cpp_202305/soundprocessing/build-sys-linux_x86_64/sdk/lib/naoqi/libsoundprocessing.so
```
修改后的autoload.ini 为
```ini
# autoload.ini
#
# Use this file to list the cross-compiled modules that you wish to load.
# You must specify the full path to the module, python module or program.

[user]
#the/full/path/to/your/liblibraryname.so  # load liblibraryname.so
/home/nao/test_cpp_202305/soundprocessing/build-sys-linux_x86_64/
sdk/lib/naoqi/libsoundprocessing.so

[python]
#the/full/path/to/your/python_module.py   # load python_module.py

[program]
#the/full/path/to/your/program            # load program

```
然后ssh在NAO机器人终端执行
```bash
nao restart
```
让 auto.ini 生效，等待NAO重新开机完毕。

## 5.尝试启动本地模块
然后在NAO机器人的ssh终端进入 /test_cpp_202305/soundprocessing/tests 目录 ，修改MySoundProcessingModuleTest.py 中 IP 地址为127.0.0.1，然后执行 
输出结果
```bash
nao [0] /data/home/nao/test_cpp_202305/soundprocessing/tests $ python MySoundProcessingModuleTest.py 
Traceback (most recent call last):
  File "MySoundProcessingModuleTest.py", line 14, in <module>
    proxy = ALProxy("ALSoundProcessing", IP, PORT)
  File "/opt/aldebaran/lib/python2.7/site-packages/naoqi.py", line 237, in __init__
    inaoqi.proxy.__init__(self, args[0], args[1], args[2])
  File "/opt/aldebaran/lib/python2.7/site-packages/inaoqi.py", line 464, in __init__
    this = _inaoqi.new_proxy(*args)
RuntimeError: 	ALProxy::ALProxy
	Can't find service: ALSoundProcessing

```
似乎仍然提示找不到自定义的 ALSoundProcessing。
```bash
sudo reboot
```
之后同样报错。
