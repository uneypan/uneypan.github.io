---
layout: article
title: NAO & Pepper 折腾笔记
mathjax: true
---
# nao&pepper折腾笔记


## 官方文档
http://doc.aldebaran.com/index.html  

| 工具箱 | 适用对象 |
| --- | --- |
| NAOqi 1.14 | NAO V4 & V3 |
| NAOqi 2.1  | NAO V5 & V4 |
| NAOqi 2.8  | NAO V6 |
| NAOqi 2.4 & 2.5  | Pepper |
| QiSDK  | Pepper SDK for Android |
| QiBuild  | For C++ developers| 


## 开箱

NAO的安全休息状态是跪姿，充电接口在背部，网络接口在后脑；胸部按钮长按开机，等待3-5min启动过程完成，直到机器人说“OGNAK GNOUK”；开机后点按一次胸部按钮报出IP地址，同一局域网电脑浏览器输入该IP地址登陆，账号和密码都是：nao，然后会有配置引导页面出现。

**注意：不要改动账号密码，否则需要售后**。
![](nao说明书.jpg)

## 网络配置：

1.使用有线或WiFi连接将NAO连接到计算机：
	初始设置则必须用有线连接，即用网线连接到路由或者计算机，在同一局域网下获取机器人的IP地址，然后进入网页配置。

2.使用Choregraphe访问NAO网页：
	启动Choregraphe。选择连接>连接到或单击连接按钮连接到按钮。显示连接到面板。右键单击绿色NAO，然后选择查看网页。
	
3.使用NAO的名称访问NAO网页：
	打开网页浏览器。如果您知道NAO的主机名：在地址栏中键入：[naoHostName].local。默认情况下，它是：nao.local。 如果您不知道NAO的主机名：单击Bonjour图标。双击NAO的名字。


## 开发环境安装与配置

1.安装Python2.7。按安装向导提示安装python-2.7.x.xxx.msi，将python安装在C盘根目录下，从而保证在之后的qibuild的安装过程中能够找到指定目录。同时在系统和用户的PATH环境变量中添加相应的文件路径 C:\Python27 和 C:\Python27\Scripts ，如果还安装了其他版本的Python，确保2.7版本的顺序在其他版本之前。测试Python正常的方法是打开cmd，输入 `python --version` 显示版本号是2.7.x。    
 
2.安装choregraphe-suite。下载 https://www.aldebaran.com/en/support/nao-6/downloads-softwares/former-versions 版本选择2.8.6，按安装向导提示安装。Choreography文档：http://doc.aldebaran.com/2-5/software/choregraphe/ ,第一次运行需要输入注册序列号，40位长，如654e-4564-153c-6518-2f44-7562-206e-4c60-5f47-5f45。   

3.安装PyCharm-community: https://www.jetbrains.com/pycharm/download/。  

4.安装NAOqi的Python库。解压 pynaoqi-python2.7-2.8.x.x-win64-vs2015-xxxxx.zip。简便起见，将文件夹重新命名为 pynaoqi，然后拷贝到C:/Python27/Lib/site-packages/路径下;    
添加用户环境变量。变量名：PYTHONPATH, 变量值：C:\Python27\Lib\site-packages\pynaoqi\lib;测试库正常安装的方法是cmd里面输入`python -c "import naoqi"`，正常情况下没有报错。   

5.安装Visual Studio 2010。  

6.安装Cmake3.12。向导安装过程中选择将Cmake路径添加到Path中。  

7.安装qibuild。打开cmd窗口，使用pip命令安装qibuild。输入如下命令：  `pip install qibuild` qibuild安装完成后，配置qibuild：`qibuild  config  --wizard`  在弹出菜单中选择安装的C++产生器版本。在后面的配置中，按提示信息分别选择IDE。配置完成后会生成 ~/.config/qi/qibuild.xml 配置文件，其中~为当前的worktree设置，如 C:/Users/Administrator。 

8.安装C++SDK。解压缩naoqi-sdk-2.1.4.13-win32-vs2010.zip。创建空目录，如在D盘创建 D:/sdk。打开cmd窗口，进入新建目录，执行如下命令： `qibuild  init` , 将解压缩内容放置所在目录拷贝到新建目录下。如解压缩目录为naoqi-sdk，将naoqi-sdk内容拷贝到 D:/sdk下。  

## 远程登录NAO
NAO不能外接键盘，也不能外接显示器，但是NAO确实是一台计算能力和操作系统功能足够强的计算机。NAO提供了基于SSH（安全外壳协议）的远程登录(Telnet)和文件传输(Ftp)服务。通过远程登录和文件传输程序，可以象使用一台装有Linux操作系统的计算机那样使用NAO。  
PuTTY是一套免费的SSH/Telnet程序，可以连接提供SSH/Telnet服务的主机。  
WinSCP是一个Windows环境下使用的SSH的开源图形化SFTP客户端，同时支持SCP协议，主要功能是在本地与远程计算机间安全地复制文件，并且可以直接编辑文件

**1.利用WinSCP下载上传文件**    
启动WinSCP，在登录窗口中选择SCP协议，输入NAO的IP地址，端口号选择22，用户名和密码都填nao，点击“登录”按钮，进入WinSCP主界面。登录后NAO的初始目录为“/var/persistent/home/nao”，与“/home/nao”目录的内容相同，用户nao对该目录具有读写权限。在WinSCP中通过在本地文件和NAO机器人文件间简单的“拖拽”操作就可以实现文件的上传和下载。选中目录中的文件，点击右键，在弹出菜单中，可以进行删除、重命名、更改文件属性等操作。

**2.利用Putty远程登录NAO** 
启动Putty，在配置窗口中输入机器人IP地址，端口输入22（SCP协议端口），点击“打开”按钮，在弹出的窗口中输入用户名nao和密码nao，进入NAO的远程控制状态。


监控NAOqi：NAOqi是机器人运行的主要软件。NAOqi不运行，机器人的任何行为都无法完成。在机器人启动过程中，NAOqi会自动启动，脚本/etc/init.d/naoqi管理NAOqi的启动过程。
控制NAOqi的命令包括如下4条：`nao  start`、`nao stop`、`nao restart`和`nao status`。

**3.Python库安装**  
pip是Python中常用的包管理工具，在NAO机器人预装的Python2.7安装包中，pip不是默认安装的。  
(1)下载pip安装文件。下载地址:https://pypi.python.org/packages/source/p/pip/pip-10.0.1.tar.gz  

(2)将下载到本地的pip-10.0.1.tar.gz利用WinSCP上传到NAO的/home/nao目录下。  

(3)登录Putty，使用su命令将用户由nao切换到root，密码为root。root用户对“/usr/lib/ python2.7/site-packages”目录拥有写权限。  

(4)在NAO上运行如下命令：
```
tar -xzvf  pip-10.0.1.tar.gz
cd pip-10.0.1
python setup.py install
```
tar是linux的解压缩命令，运行后将pip安装文件解压缩到/home/nao/pip-10.0.1目录下，文件中包括pip的安装文件setup.py。最后利用python运行setup.py完成安装。安装完成后，可以在NAO的Python库目录“/usr/lib/python2.7/site-packages”下看到新安装的pip相关目录。
pip安装完成后，可以利用如下命令完成其他python包的安装：
```
pip  install  包名
```
4.在虚拟机中编译程序  
由于NAO的存储空间有限，NAO机器人系统中并不包含gcc一类编译程序。NAO在虚拟机系统中提供了编译环境，对于需要经过编译后才能进行安装的程序，可以先在虚拟机中编译，再安装到实际系统中。


## NAO编程基础


NAOqi操作系统

NAO机器人使用NAOqi系统，它是一个专为满足 Aldebaran 机器人需求而开发的嵌入式基于Gentoo的 GNU/Linux 发行版。NAOqi为操作NAO机器人提供了一组应用程序接口(API)。使用Python或C++可以调用API。 

NAOqi提供了操作机器人的核心API，提供了数以千计的在控制运动、语音、视频等方面的函数，可以满足机器人在资源、事件、并行、同步等方面的一般需要。

NAOqi是跨平台的，支持在Linux、Windows和MacOS平台上使用Python、C++、JAVA等语言编程。
尽管NAOqi的动作、音频、视频等不同模块差异很大，NAOqi使用结构一致的数据模型来表示信息，为调用不同模块设计了相同的编程模式，各个模块与ALMemory信息共享使用相同的结构，这些都降低了NAO程序设计的复杂性。

**大部分情况下，熟悉C++的开发者应该用C++写模块，用Python写行为控制**。

1.NAOqi进程  
在NAO上执行NAOqi是通过一个**代理程序Broker**完成的。启动机器人时，代理程序会自动加载/etc/naoqi/ autoload.ini文件， autoload.ini文件中指定了需要加载NAOqi的哪些库，这些库文件位于/usr/lib/naoqi目录下。

![](NAOqi进程.jpg)

一个库包含一个或者多个模块，加载的模块形成了一种树型结构。  

每个模块类都定义了多种方法。例如，NAO的运动功能都放在ALMotion模块中，让机器人完成移动、转头、张手等动作分别要调用ALMotion模块的moveto()、setAngles()、openHand()等方法。

使用NAOqi模块时，不需要像普通Python程序那样用import语句导入所用模块。模块通过Broker通告它所提供的方法。通过Broker，任何模块都可以找到所有己经通告的模块及方法。  
 
Broker主要有两个作用：  
(1)直接服务，查找模块和方法；  
(2)网络访问，从Broker进程外部调用模块方法。Broker既是一个可执行程序，也是一个服务器，可以对指定的IP和端口监听远程命令。远程计算机、其它机器人、机器人自身的程序(Broker的外部进程)都可以使用IP地址和端口，调用模块及方法。也就是说，**通过IP和端口调用NAOqi模块的程序，既可以在机器人上运行，也可以在远程计算机上直接运行**。  
Broker是透明的。大部分情况下，编程时可以不考虑Broker，调用本地模块的代码与调用远程模块的代码是一样的。  

## 使用模块

NAOqi模块可以在同一进程(NAOqi进程)内部调用，也可以通过网络调用，为使编程模式统一，**NAO使用代理(Proxy)方式调用NAOqi模块**。  为一个模块创建Proxy对象后，Proxy对象就代表了这个模块。


```python
# 让NAO说话(say.py)
from  naoqi  import  ALProxy
tts = ALProxy("ALTextToSpeech", "192.168.1.170", 9559)        
# 将"192.168.1.170"替换成自己机器人IP
tts.say("Hello, world!")
```
说明：  
ALProxy类用于创建模块的代理对象。  
ALTextToSpeech是NAOqi中将文本转换为语音的模块。ALTextToSpeech模块的代理对象tts创建后，tts就代表了ALTextToSpeech模块，可以直接使用其所有方法。  
say方法，实现“说话”。程序中并没有指出ALTextToSpeech模块所在位置，对模块及say方法的查找由Broker完成。    
程序运行前，需要确保计算机与机器人之间网络是连通的(可以使用ping命令进行测试)。  

say.py可以在计算机上运行(在PyCharm中直接运行)，也可以将其上传到机器人的/home/nao目录下，在机器人上运行。  程序运行时，可以听到NAO发出的语音。  

ALProxy类创建代理对象有两种方法：  
(1)class  ALProxy(name,ip,port)  
- name：调用的模块名。
- ip:机器人使用的IP地址。  
- port：端口，默认为9559。  

(2)class  ALProxy(name)
- name：调用的模块名。

方法2中只使用模块的名称。在这种情况下，正在运行的代码和要连接的模块必须位于同一代理(Broker)中。**在使用Choregraphe编程时，由于Choregraphe会将所写程序上传到机器人并做相应设置，使其能直接调用NAOqi模块，所以创建模块代理时使用的是方法2**。

## 阻塞和非阻塞调用
NAOqi的方法从调用时间上看大体上可以分为两类，如读传感器状态getData()方法(属于ALMemory模块)可以很快完成；让机器人移动到某个位置的move()方法(属于ALMotion模块)要执行很长时间。  
在move()方法调用过程中，机器人可能还需要做其它的事，如用“眼睛”看，或者“说话”，因此，在调用move()方法时，还需要同时调用其它方法。  

NAOqi提供了两种调用方式：    
(1)阻塞调用：阻塞调用是指调用结果返回之前，调用者会进入阻塞状态等待。只有在得到结果之后才会返回。也就是说，在顺序结构的程序中，只有在前一个调用结束后才能执行下一条语句。    
所有的阻塞调用都可以引发异常，应该使用try-catch结构捕获异常。调用可以有返回值。  
**对于NAOqi任何阻塞调用方法，如果使用Proxy的post对象进行调用，将在并行线程中创建任务，这样可以同时执行后面的语句**。  
(2)非阻塞调用：指在不能立刻得到结果之前，该方法不会阻塞当前线程，而会立刻返回。这样就可以继续执行下面的语句。  

```python
# 让NAO说话与行走
from  naoqi  import  ALProxy
import  time
#将"192.168.1.170"替换成自己机器人IP 
motion = ALProxy("ALMotion", "192.168.1.170", 9559)      
tts  = ALProxy("ALTextToSpeech", "192.168.1.170", 9559)
motion.setStiffnesses("Body", 1.0)
time.sleep(1.0)
motion.moveInit()
#使用Proxy的post对象进行调用
motion.post.moveTo(0.5, 0, 0) 
tts.say("I'm  walking")
```
上述代码首先创建了ALMotion运动模块和ALTextToSpeech语音模块的代理对象，例中调用了如下几种方法：  
- setStiffnesses()：设置关节的刚度（电机的转矩限制），刚度为0时关节做不了任何运动，非阻塞调用。  
- moveInit()：运动进程的初始化，检查机器人的当前状态，并选择一个正确的姿势，阻塞调用。  
- moveTo()：移动到指定坐标位置，阻塞调用。    
- setStiffnesses()方法为非阻塞调用方法，由于关节的刚度设置需要一定的时间（几毫秒），如果不经过time.sleep(1.0)延迟，调用moveInit()时，各关节的刚度不一定都已经设置为1.0，moveInit方法将不会正确执行。  
- moveInit()为阻塞调用方法，完成机器人行走前的准备动作。例如，如果机器人初始状态为休息状态，调用moveInit方法后，机器人进入准备行走状态(站立、屈膝)。moveInit方法的最长执行时间会超过1秒，方法执行完成后，才会执行moveTo()方法。   
- moveTo()为阻塞调用方法，使用post对象调用moveTo()方法，将创建新的并行线程，在新线程中调用moveTo()方法，原线程继续调用后面的say方法。
程序执行结果：机器人一边向前走，一边说I'm  walking，在0.5米处停止。   

## 内存管理

NAOqi各个模块间是通过内存交换数据的。管理内存的是ALMemory模块。

内存中的数据大体上可以分为两类：  

(1)NAO的状态数据，包括执行器和传感器的数据。
NAO安装了几十个传感器，**NAOqi周期性地调用各种传感器驱动程序接口，将传感器值写到内存中**。其他模块可以读内存中当前状态下传感器值。
例如，在调用ALMotion模块的moveTo()方法时，NAO在向前走的过程中，会不断读取位置传感器、惯性传感器、压力传感器的取值，根据当前状态和行走距离调整步态，最后在向前走0.5米后停止。

```python
# 例：读取NAO行走过程中的加速度计取值
from  naoqi  import  ALProxy
import  time
motion = ALProxy("ALMotion", "192.168.1.170", 9559) 
memory=ALProxy("ALMemory","192.168.1.170",9559)
motion.setStiffnesses("Body", 1.0)
time.sleep(1.0)
motion.moveInit()
motion.post.moveTo(0.5, 0, 0)
for  i  in  range(0,40):
    print  memory.getData(
      "Device/SubDeviceList/InertialSensor/AccelerometerX/Sensor/Value")
    time.sleep(0.2)
```
为了在运动过程中读取加速度计传感器的取值，使用非阻塞调用方式调用moveTo()方法。ALMemory模块的getData方法从内存中读取执行器/传感器的取值。  
**ALMemory以无序映射（unordered_map）的方式存储数据，每个数据是一个键/值对**。  
NAOqi为每个执行器/传感器定义了一个名称，getData方法以此名称为键，在内存中读取相应值。  
Device/SubDeviceList/InertialSensor/AccelerometerX/Sensor/Value表示X轴加速度传感器。 
完整的键表见 [NAO V6 Actuator & Sensor list](http://doc.aldebaran.com/2-8/family/nao_technical/lola/actuator_sensor_names.html#actuator-sensor-list-naov6) 和 [Pepper  Actuator & Sensor list](http://doc.aldebaran.com/2-8/family/pepper_technical/pepper_dcm/actuator_sensor_names.html#actuator-sensor-list-pepper)。


(2)订阅的事件/微型事件数据。像人脸识别等模块，由于处理过程运算量非常大，**NAOqi只在订阅这些功能时才向内存中写数据**，而不会象传感器那样周期性地写数据。订阅的模块运行完成后将以事件的方式通知NAOqi。

订阅数据也通过键从内存中读取，各订阅模块产生的数据通常使用列表表示，列表元素是其他类型或列表。

## 自定义模块
NAOqi模块都是库中的类。从autoload.ini加载库时，这些模块类将自动实例化。
除了使用NAOqi模块外，也可以自定义模块。自定义模块类的基础类是ALModule。从ALModule派生的类，可以“绑定”方法。模块名称和方法将被通告给代理，这样其他模块就可以使用自定义模块了。

**1.模块分类**  
**自定义模块可以是远程模块也可以是本地模块**。如果是远程的，在机器人外部运行。远程模块可以从外部调试，但在速度和内存使用方面效率较低。如果是本地的，在机器人本地运行。本地模块比远程模块效率高。

每个模块都可以包含多种方法，某些方法可以限定为从模块外部调用。不管模块是远程的还是本地的，调用这些限定方法的方式是一样的。  

**本地模块是在同一进程中启动的两个(或更多)模块，本地模块由NAOqi作为代理(Broker)，统一管理，可以共享变量，可以直接调用彼此的方法**。在做一些闭环控制时，必须使用本地模块。  

远程模块使用网络进行通信。远程模块需要代理与其他模块通信。Broker负责所有网络通信。  

**2.远程模块的连接**  
**远程模块可以通过使用代理方法，将其代理连接到其他模块的代理上，实现与其他模块通信**。连接方式如图所示，可以是：Broker到Broker；Proxy到Broker

![](远程模块的连接.jpg)

**3.ALModule方法**
ALModule模块类是自定义模块类的基类，负责为其子类通告方法。
 
**4.自定义Python模块**  
**(1)创建自定义模块**  
主要分以下两步：      
①以ALModule为基类创建自定义类，定义方法和事件处理方法，绑定事件。

绑定事件使用ALMemory模块的subscribeToEvent(name, callbackModule,callbackMethod)方法，
- name 为事件名称
- callbackModule 为调用事件的模块(实例)名称
- callbackMethod 为事件处理方法

②创建Broker代理，创建自定义模块类实例，进入循环状态，等待事件发生或其他模块调用。 

<!-- 表格标题

方法名 | 说明 
--- | --- 
getMethodList() | 获取模块方法列表。  
getBrokerName() | 获取模块父Broker名称 
isRunning(id) | post方式调用的方法是否还在运行，返回值为布尔量，id为使用post调用方法的返回id   
wait(id,timeout) | 对post方式调用的方法进行等待，直到指定时间结束。等待时间结束返回True,否则为False。timeout为等待时间(ms)，整型，为0表示一直等待。 
stop(id) | 停止post方式调用的方法的运行。 
exit() | 对父Broker解除模块注册，调用后模块无效。
getMethodHelp(methodName) | 返回方法描述(列表)。methodName为方法名称。
getUsage(methodName) | 返回模块方法使用情况。  -->

```python
# myModule.py
import  sys
import  time
from  naoqi  import  ALProxy
from  naoqi  import  ALBroker
from  naoqi  import  ALModule
HumanGreeter = None
memory = None
class  HumanGreeterModule(ALModule):
  
    def  __init__(self, name):
        ALModule.__init__(self, name)
        self.tts = ALProxy("ALTextToSpeech")
        global  memory
        memory = ALProxy("ALMemory")
        memory.subscribeToEvent("FrontTactilTouched", "HumanGreeter", "onFrontTactilTouched")

    def  onFrontTactilTouched(self, *_args):
        memory.unsubscribeToEvent("FrontTactilTouched", "HumanGreeter")
        self.tts.say("Hello, you touch me")
        memory.subscribeToEvent("FrontTactilTouched", "HumanGreeter",  
                  "onFrontTactilTouched")

    def  myMethod(self):
          self.tts.say("hello")

def  main(IP):
    myBroker = ALBroker("myBroker", "0.0.0.0", 0,IP , 9559) 
    global  HumanGreeter
    HumanGreeter = HumanGreeterModule("HumanGreeter")
    try:
        while  True:
            time.sleep(1)
    except  KeyboardInterrupt:
        print  "Interrupted by user, shutting down"
        myBroker.shutdown()
        sys.exit(0)

if  __name__ == "__main__":
	main("127.0.0.1") 

```
上述程序中，HumanGreeterModule类的模块实例为HumanGreeter，FrontTactilTouched为触摸头部前传感器时产生的事件，subscribeToEvent方法将该事件与onFrontTactilTouched方法绑定在一起，将其作为事件处理方法。    

创建myBroker时使用本机地址(0.0.0.0)，本地随机可用端口(0)，父Broker为naoqi，地址为机器人IP，端口号为9559。main方法中的参数127.0.0.1为程序在机器人上运行时的地址，如果在计算机上运行，地址应改为机器人IP。    

程序运行后，触摸头部前传感器，NAO将说"Hello, you touch me"。

**(2)调用自定义模块** 
自定义模块运行后，其他模块可以对其方法进行调用。  
在Choregraphe中调用HumanGreeter模块实例方法

``` python
# Choregraphe 里面新建Python指令盒
class  MyClass(GeneratedClass):
    def  __init__(self):
        GeneratedClass.__init__(self)
        self.mymodule=ALProxy("HumanGreeter")
    def  onLoad(self):
        pass
    def  onUnload(self):
        pass
    def  onInput_onStart(self):
        self.mymodule.myMethod()
        pass
    def  onInput_onStop(self):
        self.onUnload()
        self.onStopped()

```
**(3)自启动自定义模块**  
①使用WinScp在/home/nao目录下创建mymodule子目录，将myModule.py上传到该子目录下。  
②修改/home/nao/naoqi/preferences/autoload.ini。
``` ini
# autoload.ini
#
# Use this file to list the cross-compiled modules that you wish to load.
# You must specify the full path to the module, python module or program.
[user]
#the/full/path/to/your/liblibraryname.so  # load liblibraryname.so
[python]
#the/full/path/to/your/python_module.py   # load python_module.py
/home/nao/mymodule/myModule.py
[program]
#the/full/path/to/your/program            # load program
```
③重新启动机器人，将自动运行 `mymodule.py`。


## C++ SDK - Linux 安装指南

支持的操作系统: Linux	Ubuntu 16.04 Xenial Xerus - 64bits 

Windows 系统的局限性:
- C++ 交叉编译工具（用于编译机器人上运行的代码）在Windows系统上不支持。
- Windows 架构：编译时需要针对32位编译，虽然这两种架构（32位和64位）都支持运行您喜欢的IDE，但必须专门针对32位进行编译。生成的代码也将在32位或64位环境中运行。

1.支持的编译器和 IDE 
- GCC  4.8.2 或更高版本.

安装 GCC (提前于其他工具)，使用:
```bash
$ sudo apt-get install build-essential
```

- 你喜欢的 IDE 或编辑器。
  提示：不知道选择什么?
  - 推荐最新版本的 QtCreator 。
  - 也可以用 Eclipse，如果你已经习惯了 。

2.安装 qiBuild。 

为什么安装 qiBuild? 

我们需要（在你的计算机上）编译代码，在另一个操作系统（在机器人上）下运行，所以需要交叉编译。qiBuild 基于 CMake，是用于生成跨平台工程的工具。CMake 为任何操作系统生成 makefile 和 Workspaces，使项目能够在Windows、Mac、Linux和 NAOqi 操作系统上简单编译。

3.准备工作
- CMake
  使用系统支持的版本号，保证为 2.8.12 或更高
  Tips: 检查 CMake 版本:
  ```bash
  $ cmake --version
  ```
  如果不存在，使用以下命令安装:
  ```bash
  $ sudo apt-get install cmake
  ```
- Python 2.7
  Tip: 检查 Python 版本:
  ```bash
  $ python  --version
  ```

4.安装
- 使用 pip 安装 qibuild.
  建议是使用参数 --user 安装，保持系统环境的清洁。
  ```bash
  $ pip install qibuild --user
  ```
  将 \$HOME/.local/bin 添加到 $PATH，一种做法是在 ~/.bashrc 文件的末尾添加一行：
  ```text
  PATH=$PATH:$HOME/.local/bin
  ```

5.配置
运行:
```bash
$ qibuild config --wizard
```
按照提示配置:
- CMake 路径 (如果没有检测到)
- CMake 生成器，
```
Found CMake: /usr/bin/cmake
:: Please choose a generator
   1 Unix Makefiles 	(default)
   2 Ninja
   3 Watcom WMake
   4 CodeBlocks - Ninja
   5 CodeBlocks - Unix Makefiles
   6 CodeLite - Ninja
   7 CodeLite - Unix Makefiles
   8 Eclipse CDT4 - Ninja
   9 Eclipse CDT4 - Unix Makefiles
   10 KDevelop3
   11 KDevelop3 - Unix Makefiles
   12 Kate - Ninja
   13 Kate - Unix Makefiles
   14 Sublime Text 2 - Ninja
   15 Sublime Text 2 - Unix Makefiles
> 1
```
- IDE
```
:: Please choose an IDE
   1 None 	(default)
   2 QtCreator
   3 Eclipse CDT
> 2
:: Found QtCreator: /usr/bin/qtcreator
:: Do you want to use qtcreator from /usr/bin/qtcreator?
Answer 'no' if you installed qtcreator from Nokia's installer (Y/n)
> y
```
- 重复配置会出现
```
:: Found a worktree in /home/nao/Downloads/mywtree
:: Do you want to configure settings for this worktree? (y/N)
> y
:: Found the following build configs:  sys-linux_x86_64
:: Use one of these build configs by default (Y/n)
> y
:: Choose a build config to use by default
   1 sys-linux_x86_64 	(default)
> 1
:: Do you want to use a unique build dir? (mandatory when using Eclipse) (y/N)
> n
```
建议在 Linux 上使用 ‘Unix Makefiles’，在 Windows 上使用 ‘Visual Studio’。
注意，你可以在任何时候再次运行上述配置引导。
结果：生成了一个配置文件 ~/.config/qi/qibuild.xml. 它被所有我们创建的工作树 (worktree) 共享。

6.工作树初始化
- 创建一个空的文件夹作为工作树。暂时称之为: /path/to/myWorktree
- 在这个空文件夹内打开终端，输入以下命令初始化工作树：
  ```bash
  $ qibuild init
  ```

7.最后检查
创建一个样本来检查一切是否正常。 如果你想：
- 快速检查, 试试: C++ SDK - Hello world (auto-generated).
- 理解所有实现的所有细节，试试: C++ SDK - Hello world (step by step) http://doc.aldebaran.com/2-8/dev/cpp/helloworld_detailed.html.

8.安装和配置 NAOqi SDK

- 获取 C++ SDK

  下载 C++ SDK 压缩包: `naoqi-sdk-x.x.x-[OS].tar.gz` （Windows 是 `naoqi-sdk-x.x.x-win32-[visualStudioversion].zip`）

  可以在开发者中心下载到最新的发行版，注意选择的版本需要对应操作系统。

- 在你的机器上解压缩.
  假设解压得到的文件夹命名为: naoqi-sdk.
	
- 创建你的工具链

  输入以下命令利用 C++ SDK 创建工具链（toolchain）：
  ```bash
  $ qitoolchain create linux /home/nao/Downloads/naoqi-sdk/toolchain.xml
  ```
  其中 linux" 是你想给这个工具链取的名（你可以有多个工具链）, /home/nao/Downloads/naoqi-sdk/toolchain.xml 是 C++ SDK 内 toolchain.xml 的路径。

- 进入你的工作树:
  ```bash
  $ cd /path/to/myWorktree
  ```
- 创建配置
  输入以下命令来创建一个与这个工具链对应的编译配置，并将此配置作为当前工作树的默认配置:
  ```bash
  $ qibuild add-config sys-linux_x86_64 -t linux --default
  ```

## C++ SDK - Hello world (auto-generated)

1.在你的工作树里输入：
```bash
$ qisrc create myFirstExample
```
结果：创建了一个”myFirstExample“的目录，里面生成了一个”hello world“。

2.进入这个目录
```bash
$cd myfirstexample
```
输入
```bash
$qibuild configure
```
结果: 创建了一个 "build-\<myconfig\>" 目录, 其中 \<myconfig\> 送对应了当前的配置。例如 build-sys-linux-x86_64。

3.编译 ”hello world“ 程序，使用：
```bash
$ qibuild make
```

4.运行 ”hello world“ 程序，例如,进入 build-\<myconfig\>/sdk/bin 目录
```bash
cd build-<myconfig>/sdk/bin
```
然后输入：
```bash
./my_first_example
```

## C++ SDK - Hello world (step by step)

http://doc.aldebaran.com/2-8/dev/cpp/helloworld_detailed.html

## 使用qibuild编译远程C++模块

C++和Python调用NAOqi的编程方法是相同的，使用模块代理方式调用模块方法。
qibuild是一种使工程编译变得更加简单的工具，管理工程的各种依赖，支持交叉编译。
NAO开发文档中推荐使用qibuild进行程序编译。
1.sayhelloworld示例
使用say方法发声(NAOqi c++ SDK示例，文件名sayhelloworld.cpp)
```cpp
// sayhelloworld.cpp
#include <iostream>
#include <alerror/alerror.h>
#include <alproxies/altexttospeechproxy.h>
int main(int argc, char* argv[])
{   
 if(argc != 2)  {
    std::cerr << "Wrong number of arguments!" << std::endl;
    std::cerr << "Usage: say NAO_IP" << std::endl;
    exit(2);
  }
  const std::string phraseToSay = "Hello world";
  try  {
     AL::ALTextToSpeechProxy  tts(argv[1], 9559);
    tts.say(phraseToSay);
  }  catch (const AL::ALError& e)  {
    std::cerr << "Caught exception: " << e.what() << std::endl;
    exit(1);
  }
  exit(0);
}
```
2.CMake、qibuild
按附录A所示步骤安装好C++开发环境，需要的软件包括Visual studio 2010、CMake、qibuild、NAOqi C++ SDK。各部分作用及关系如下：

(1)C++程序是由数量众多的.h和.cpp文件通过编译器生成的，大量的源码文件需要使用项目工程来构建和管理。Visual studio 2010或其更高版本是windows下的开发工具，使用项目文件，完成复杂项目的管理与编译。


(2)CMake是一个跨平台的软件，能够输出各种各样的makefile或者project文件。需要要编写CMakeLists.txt文件，它是CMake所依据的规则。

(3)qibuild工具提供了一组命令，调用CMake的API，可以顺利配置编译出我们想要的IDE工程。

在SDK的sayhelloworld示例中，CMakeLists.txt内容为：
```cmake
CMakeLists.txt(sayhelloworld)
cmake_minimum_required(VERSION 2.6.4 FATAL_ERROR)
project(sayhelloworld)
find_package(qibuild)
qi_create_bin(sayhelloworld  sayhelloworld.cpp)
qi_use_lib(sayhelloworld  ALCOMMON ALPROXIES)
```
在CMakeLists.txt中，指定了项目名称、二进制文件对应的主程序、使用的类库等内容。


3.编译
以下编译过程均为cmd窗口中命令行操作方式。
(1)第一次运行qibuild要进行初始化设置，为CMake指定生成项目类型(makefile/project)及IDE类型。参见附录A。
`qibuild  config  --wizard`
(2)创建一个工作路径，即 worktree。
新建一个空目录，如`d:/sdk`。在cmd窗口中进入该目录(在资源管理器中打开目录，按shift键同时右击，选择“在此处打开命令窗口”)。执行命令：
`qibuild  init`
命令执行后，sdk目录被设为工作路径，目录下新生成一个.qi目录。
在sdk目录下新建naoqi-sdk目录，将naoqi-sdk-2.1.4.13-win32-vs2010.zip解压缩内容拷贝到该目录下，包括bin、etc、include目录等。

(3)编译。执行如下命令：

```bash
cd  d:/sdk/naoqi-sdk/doc/dev/cpp/examples
qitoolchain  create  mytoolchain  d:/sdk/naoqi-sdk/toolchain.xml
qibuild  add-config  mytoolchain  -t  mytoolchain  --default
cd core/sayhelloworld
qibuild  configure
```
此时，生成vs2010项目文件，build-mytoolchain目录下文件如图所示。
执行`qibuild  make`命令生成可执行程序；
或者使用Visual Studio打开解决方案文件sayhelloworld.sln，在解决方案资源管理器中将sayhelloworld设置为启动项目，最后生成sayhelloworld可执行程序。生成的sayhelloworld.exe位于build-mytoolchain/sdk/bin目录下。

(4)运行。在命令窗口中进入build-mytoolchain/sdk/bin目录，执行：
```bash
sayhelloworld  192.168.1.170
```
机器人将输出语音hello world。其中192.168.1.170为机器人的IP地址，作为main函数的参数直接用于生成ALTextToSpeech模块代理的IP参数。

使用Visual Studio高版本时，如果编译时出错，可以在项目属性窗口设置中将 常规 - 平台工具集 设置为Visual Studio 2010，如图所示。Visual Studio 2010平台工具集选项如果不存在，可以同时安装VS2010和高版本VS。

## 扩展NAO API

**(1)使用qibuild创建模块框架**。
进入要在其中创建项目的worktree，输入：
`qisrc   create   mymodule`
在worktree/mymodule中创建一个新项目。生成的内容：
- CMakeLists.txt：CMake读取该文件以生成makefile或Visual Studio解决方案。
- main.cpp：只是一个标准的“Hello World”。
- qibuild.cmake：CMakeLists.txt包含此文件才能找到qiBuild CMake框架。
- qibuild.manifest： qiBuild使用文件信息构建项目。

**(2)在CMakeLists.txt中声明依赖关系**。
由qisrc生成的CMakeLists.txt内容如下：
```cmake
CMakeLists.txt
cmake_minimum_required(VERSION 2.8)
project(mymodule)
include("qibuild.cmake")
qi_create_bin(mymodule  "main.cpp")
```
与naoqi通信，需要在代码中使用代理。代理在ALCommon库中。需要在CMakeLists中导入ALCommon。

在CMakeLists.txt中添加：
```cmake
qi_use_lib(<your_project_name> <library_you_want_use_1> <library_you_want_use_2>)
```
CMakeLists.txt
```cmake 
cmake_minimum_required(VERSION 2.8)
project(mymodule)
include("qibuild.cmake")
qi_create_bin(mymodule  "main.cpp")
qi_use_lib(myproject ALCOMMON ALPROXIES)
```
由qisrc生成的生成的main.cpp文件是标准的main.cpp，只在标准输出上打印一个“Hello，world”。

扩展main.cpp使之能与NAOqi进行通信，并从NAOqi的模块调用bind函数。
使用--pip和--pport选项实现命令行参数输入；
为使用的模块创建代理，包含<alcommon / alproxy.h>。
```cpp
// main.cpp
#include <iostream>
#include <stdlib.h>
#include <alcommon/alproxy.h>
#include <alcommon/albroker.h>
int main(int argc, char* argv[])
{
 ...
}
```
**(3)创建远程模块**。  
远程模块是通过网络连接到NAOqi的程序。以ALModule为父类创建新类mymodule.cpp。
```cpp
// mymodule.h
#ifndef MY_MODULE_H
# define MY_MODULE_H
# include <iostream>
# include <alcommon/almodule.h>
namespace  AL{
  class ALBroker;
}
class MyModule : public AL::ALModule{
public:
  MyModule(boost::shared_ptr<AL::ALBroker> broker, const std::string &name);
  virtual ~MyModule();
  virtual void init();
  void printHello();
  void printWord(const std::string &word);
  bool returnTrue();
};
#endif // MY_MODULE_H
```
```cpp
// mymodule.cpp
#include "mymodule.h"
#include <iostream>
#include <alcommon/albroker.h>
MyModule::MyModule(boost::shared_ptr<AL::ALBroker> broker,  const std::string& name)
  : AL::ALModule(broker, name)
{
……
}
```
新模块建立后，需要更新CMakelists.txt，将新建的模块添加。
```cmake
set(<variable_name>  <source_file>)
CMakelists.txt
cmake_minimum_required(VERSION 2.8)
project(mymodule)
find_package(qibuild)
set(_srcs    mymodule.cpp   mymodule.h   main.cpp)
qi_create_bin(mybroker   ${_srcs})
qi_use_lib(mybroker ALCOMMON ALPROXIES)
```
在main.cpp中，创建一个broker，将新broker添加到NAOqi的broker中。然后，可以创建自定义模块并将其与刚刚创建的新broker链接。


## 音频处理

ALAudioDevice管理音频的输入和输出，提供了一组操作音频输入设备(麦克风)和输出设备(扬声器)的API。其他的音频模块(ALAudioPlayer除外)做输入和输出时都使用ALAudioDevice提供的API。    
ALAudioDevice基于标准的Linux ALSA(Linux声音库)库，通过声音驱动程序，与麦克风和扬声器通信。


![](ALAudioDevice与其他模块关系.jpg)
## 输出
**1.数据格式 ** 
ALAudioDevice可以通过以下帧速率之一将数据发送到扬声器：  
两声道，交错数据(声道1数据，声道2数据，声道1数据，…)，16000Hz（设置亚洲语言时默认）；  
两声道，交错数据，22050Hz（默认设置非亚洲语言时）；  
两声道，交错数据，44100Hz；  
两声道，交错数据，48000Hz。  

**2.输出方法**  
(1)sendLocalBufferToOutput(nbOfFrames, buffer)  
本地模块 (运行在机器人上的模块)将存放在数据缓冲区声音数据发送到扬声器。 数据格式为16位两声道交错数据，缓冲区长度不能超过16384字节。  
其中，nbOfFrames 为缓冲区中包括的两声道声音数据帧数。在两声道交错数据格式中，1帧长度为两声道的数据长度，4字节；buffer 为发送缓冲区内存地址。  
发送成功方法返回值为真，否则返回值为假。  
(2)sendRemoteBufferToOutput(nbOfFrames, buffer)  
远程模块 (运行在机器人之外的模块)将存放在数据缓冲区声音数据发送到扬声器。  
任何NAOqi模块都可以在需要时调用适当的方法向NAO的扬声器发送数据。如果数据格式正确，不必做其他配置就可以调用。  

**3.输出相关方法**   
(1)setParameter(parameter,value)设置输出采样率  
其中，parameter只能设置为"outputSampleRate"，value为设置的输出采样率，可以设置为16000, 22050, 44100 或 48000。  
(2)setOutputVolume(volume)设置系统的输出音量  
其中，volume取值为[0,100]。  
系统音量可以由getOutputVolume()方法获取。  

**4.实例**
```python
# 播放WAV文件 (文件内容发送给扬声器)
import  math
import  os.path
class  MyClass(GeneratedClass):
    def  __init__(self):
        GeneratedClass.__init__(self)
        self.aad=ALProxy("ALAudioDevice")
    def  onLoad(self):
        pass
    def  onUnload(self):
        pass
    def  onInput_onStop(self):
        self.onUnload() 
        self.onStopped()
	def  onInput_onStart(self):
		self.aad.setParameter("outputSampleRate",16000)
		songPath = "/home/nao/hongdou.wav"   #文件为两声道，采样率16000
		size=os.path.getsize(songPath)
		nframe=(size-44)/4                #帧数,WAV头部为44字节，每帧4字节（两声道，每声道16位）
		with  open(songPath,"rb")  as  file:
			file.seek(0)
			data=file.read()
		data=data[44:]                                 #头部以后为数据部分
		self.aad.sendLocalBufferToOutput(int(nframe), id(data))                      #使用id函数取数据在内存地址
		pass

```

```python
# 生成正弦波并播放
import  numpy as np
class  MyClass(GeneratedClass):
    def  __init__(self):
        GeneratedClass.__init__(self)
        self.aad=ALProxy("ALAudioDevice")
    def  onLoad(self):
        pass
    def  onUnload(self):
        pass
    def  onInput_onStop(self):
        self.onUnload() 
        self.onStopped()
	def  onInput_onStart(self):
        sineTableSize = 1024
        outputBufferSize = 16384
        sine=np.arange(sineTableSize,dtype=np.int16)
        left_phase = 0
        right_phase = 0
        nbOfOutputChannels=2
        sampleRate = 16000
        freq =1000
        duration =5.0     
        fOutputBuffer=np.arange(nbOfOutputChannels*outputBufferSize,
              dtype=np.int16)
		for  i  in  range(0,sineTableSize):
          sine[i]=int(32767 * math.sin((float(i)/float(sineTableSize))*math.pi*2.0))
        ratio= 1.0 /( 1.0 / float(freq) * float(sampleRate) / float(sineTableSize))
        inc= int(math.ceil(ratio))
        nbOfBuffersToSend =int(math.ceil(duration*sampleRate/outputBufferSize))
		for  d  in  range(0,nbOfBuffersToSend):
            i=0
            while i<nbOfOutputChannels*outputBufferSize:
                fOutputBuffer[i] =sine[left_phase]
                fOutputBuffer[i+1] =sine[right_phase]
                left_phase =left_phase+ inc
                if  left_phase >= sineTableSize:
                    left_phase =left_phase- sineTableSize
                right_phase = right_phase+inc
                if  right_phase >= sineTableSize :
                    right_phase =right_phase- sineTableSize
                i=i+nbOfOutputChannels
            buffer=fOutputBuffer.tostring()
            self.aad.sendLocalBufferToOutput(outputBufferSize,id(buffer))
        pass

```


## 输入
**1.订阅ALAudioDevice模块**  
由于音频数据量非常大，其它的NAOqi模块只有在订阅ALAudioDevice模块时才能够读取麦克风输入的数据，而不会象取传感器值那样随时可以从内存中读到。
请求音频数据的模块首先使用subcribe方法订阅ALAudioDevice模块，并指定其所需数据的格式(通道数，采样率等)。ALAudioDevice模块按照采样参数采样麦克风输入数据，将数据存储在缓冲区中，缓冲区满后通知请求模块读取数据。  
订阅相关方法为：  
①subscribe(module)：订阅ALAudioDevice模块，module为模块名。  
订阅ALAudioDevice模块后，请求模块的功能中需要包含一个自动定期调用的方法，处理来自麦克风的原始数据。该方法参数包括通道数、采样率、缓冲区、时间，格式如下：   
process(nbOfChannels,nbrOfSamplesByChannel,timeStamp,buffer)  
②unsubscribe(module)：解除订阅ALAudioDevice模块，module为模块名，方法执行后，将停止请求模块处理数据方法的周期调用。  
③setClientPreferences(name,sampleRate, channels,deinterleaved)：设置参数。  
ALAudioDevice提供下述几种方式的音频数据：  
- 四声道，交错数据，48000Hz，170ms缓冲区(默认设置)；  
- 四声道，非交错数据，48000Hz，170ms缓冲;  
- 单声道(前右,前左,后左,后右)，16000Hz，170ms缓冲。  

在订阅数据之前，需要先使用setClientPreferences方法设置数据格式，参数含义为：  
- name为订阅模块名称，必须与subcribe(name)中的名称一致；  
- sampleRate为采样率，可以取48000或16000；  
- channels取值为0：四通道，1：前右，2：前左，3：后左，4：后右；  
- deinterleaved只在多通道时有效，取1时数据为非交错格式。  

**2.自定义模块处理音频数据**  
订阅ALAudioDevice模块后，缓冲区满将作为事件与请求模块方法相关联。与前面的处理方式类似，需要将数据处理与指定方法绑定。  



## 音频处理自定义模块

```python
#coding=utf-8
from  naoqi  import  *
import  time
import  numpy  as  np
import  matplotlib.pyplot  as  plt
class  SoundProcessingModule(ALModule):
    def __init__( self, strName):
        ALModule.__init__( self, strName );
        self.BIND_PYTHON( strName, "processRemote")
        self.ALAudioDevice = ALProxy("ALAudioDevice", IP, 9559)
        self.isProcessingDone = False
        self.nbOfFramesToProcess =1
        self.framesCount=0
    def  startProcessing(self):
        self.ALAudioDevice.setClientPreferences(self.getName(), 16000, 3, 0)
        self.ALAudioDevice.subscribe(self.getName())
        while self.isProcessingDone == False:
            time.sleep(1)
        self.ALAudioDevice.unsubscribe(self.getName())
    def  processRemote(self, nbOfChannels, nbOfSamplesByChannel, timeStamp,
                     inputBuffer):
        self.framesCount = self.framesCount + 1;
        if (self.framesCount <= self.nbOfFramesToProcess) :
            self.dataTreat(inputBuffer)
        else :
            self.isProcessingDone=True
	def  dataTreat(self,data):
        inputBufferSize=int(len(data)/2)
        soundData=np.frombuffer(data,dtype=np.int16)
        normalizationData=np.arange(inputBufferSize,dtype=float)
        normalizationData=soundData/32768.0
        rms=np.sqrt(np.sum(np.power(normalizationData,2)/len(normalizationData)))
        print  rms
        yf=abs(np.fft.fft(normalizationData
        yf1=yf[0:int(inputBufferSize/2)]              
        xf = np.linspace(0,8000, int(inputBufferSize/2)) 
        plt.plot(xf,yf1,'r')
        plt.title('FFT',fontsize=7,color='#7A378B') 
        plt.show()

if  __name__ == '__main__':
    IP= "192.168.1.170"                        #将"192.168.1.170"替换成自己机器人IP
    pythonBroker = ALBroker("pythonBroker","0.0.0.0",0, IP,9559)
    MySoundProcessingModule = SoundProcessingModule("MySoundProcessingModule")
    MySoundProcessingModule.startProcessing()
    pythonBroker.shutdown()


```
程序说明：  
1.模块执行时通过Broker通告模块及其方法，启动方法startProcessing()完成由self.nbOfFramesToProcess设定数量的帧处理。本例为绘制声音频率曲线，使用了matplotlib库，绘图代码与声音处理无关。由于绘图过程慢，处理帧设置为1。  
2.使用ALModule基类的BIND_PYTHON方法将模块处理方法指定为processRemote方法。  
3.启动订阅后，每采集一帧数据，调用processRemote方法进行数据处理，处理数据放置在inputBuffer中。当前设置inputBuffer长度是2700字节。  
4.声音信号的能量定义为信号的平方和，均方根rms反映了信号能量的大小。  
5.FFT变换后结果为复数形式，对应傅立叶级数公式中的两个系数，对其求绝对值结果是实部与虚部的平方根。  
6.声音处理过程中的参数：
setClientPreferences方法设置的采样方式为单通道、采样率为16000；  
最高频率为16000/2=8000Hz(采样定理)；
一帧实际数据为2700字节，为1365次采样，一帧对应1365/16000=8.53ms数据；  
一帧信号基频为16000/1365=11.72Hz，也就是一次谐波的频率为11.72Hz，二次谐波频率为23.44Hz；  
FFT变换结果为对称的，即1365个采样数据，经过变换后得到1365个FFT复数数据，实际只有682个有效数据；每个数据之间的频率差仍为11.72Hz。  

3.测试  
同时运行播放正弦波声音和音频处理自定义模块，播放正弦波声音，并通过麦克风输入读取，结果如图所示。  
playSine(frequence,gain,pan,duration)
播放正弦波声音。其中frequence为频率(Hz)；gain为音量大小，取值范围为[0,100]；pan为左右声道占比，取值为{-1,0,1}，取0表示左右声道同时发声；duration为持继时间(s)。
```python 播放正弦波声音
class  MyClass(GeneratedClass):
    def  __init__(self):
        GeneratedClass.__init__(self)
        self.aad=ALProxy("ALAudioDevice")
    def  onLoad(self):
        pass
    def  onUnload(self):
        pass
    def  onInput_onStart(self):
        self.aad.playSine(4000,100,1,10)
        pass
    def  onInput_onStop(self):
        self.onUnload()
        self.onStopped()
```
## ALAudioDevice方法

作用  
ALAudioDevice为其他 NAOqi 模块提供访问 NAO 音频输入（麦克风）和输出（扬声器）的权限。

每个旨在处理来自 NAO 麦克风的信号或需要向 NAO 扬声器发送信号的 NAOqi 模块都应使用ALAudioDevice提供的 API 。  

工作原理
ALAudioDevice基于标准的 Linux ALSA（高级 Linux 声音库）库与 NAO 的声音驱动程序进行通信，随后与麦克风和扬声器进行通信。

任何 NAOqi 模块都可以通过在需要时调用两个适当的 ALAudioDevice 方法之一来向 NAO 的扬声器发送数据。如果数据格式正确，则无需配置任何其他内容即可进行此类调用。

要处理来自麦克风的数据，程序是不同的。实际上，愿意处理此类数据的 NAOqi 模块将首先“订阅”ALAudioDevice并指定其所需数据的格式（通道数、采样率等）。格式正确的数据将自动定期发送到请求模块，方法是使用其中一种方法作为回调。



方法名|说明
---  | ---
enableEnergyComputation()|允许每个声道计算能量。缺省不计算。
disableEnergyComputation()|不计算每个声道的能量。
getFrontMicEnergy()|在允许计算每个声道能量时，返回声道在170ms时间内的平均能量。能量为信号的平方和。
getRearMicEnergy()|同上
getLeftMicEnergy()|同上
getRightMicEnergy()|同上
getParameter(parameter)|获取参数值，parameter取值为"outputSampleRate"。
getOutputVolume()|获取系统音量，[0,100]。
muteAudioOut(mute)|mute为True，静音，为False，解除静音
setFileAsInput(fileName)|通知ALAudioDevice模块，声音输入为从文件读取内容。文件必须为48000Hz、16位、4声道交错数据的WAV文件，fileName为文件的绝对路径名。
startMicrophonesRecording(fileName)|录音。fileName为文件的绝对路径名，扩展名为wav时，数据存储为16位、48000Hz, 4声道wav文件，扩展名为ogg时，数据存储为16位、16000Hz,1声道ogg文件。
stopMicrophonesRecording()|停止录音。与startMicrophonesRecording()对应。




要开始编写可处理 NAO 麦克风数据的 NAOqi 模块，请参考以下示例，该示例可作为简述：

alsoundprocessing.h

```cpp
// alsoundprocessing.h

/**
* @author Gwennael Gate
* Copyright (c) Aldebaran Robotics 2010, 2011
*/


#ifndef SOUNDPROCESSING_H
#define SOUNDPROCESSING_H
#include <string>
#include <rttools/rttime.h>

#ifdef SOUNDPROCESSING_IS_REMOTE
#include <qi/application.hpp>
#include <boost/function.hpp>
#include <boost/bind.hpp>
#endif

#include <boost/shared_ptr.hpp>
#include <alvalue/alvalue.h>
#include <alproxies/almemoryproxy.h>
#include <alaudio/alsoundextractor.h>

using namespace AL;

class ALSoundProcessing : public ALSoundExtractor
{

public:

  ALSoundProcessing(boost::shared_ptr<ALBroker> pBroker, std::string pName);
  virtual ~ALSoundProcessing();

  //method inherited from almodule that will be called after constructor
  void init();

public:
  void process(const int & nbOfChannels,
               const int & nbrOfSamplesByChannel,
               const AL_SOUND_FORMAT * buffer,
               const ALValue & timeStamp);

private:
  ALMemoryProxy fProxyToALMemory;
  std::vector<std::string> fALMemoryKeys;
};
#endif


```

alsoundprocessing.cpp


```cpp 
// alsoundprocessing.cpp
#include "alsoundprocessing.h"
#include <alcommon/alproxy.h>
#include <iostream>

ALSoundProcessing::ALSoundProcessing(boost::shared_ptr<ALBroker> pBroker,
                                     std::string pName)
  : ALSoundExtractor(pBroker, pName)
{
  setModuleDescription("This module processes the data collected by the "
                       "microphones and output in the ALMemory the RMS power "
                       "of each of the four channels.");

}

void ALSoundProcessing::init()
{
  fALMemoryKeys.push_back("ALSoundProcessing/leftMicEnergy");
  fALMemoryKeys.push_back("ALSoundProcessing/rightMicEnergy");
  fALMemoryKeys.push_back("ALSoundProcessing/frontMicEnergy");
  fALMemoryKeys.push_back("ALSoundProcessing/rearMicEnergy");

  fProxyToALMemory.insertData(fALMemoryKeys[0],0.0f);
  fProxyToALMemory.insertData(fALMemoryKeys[1],0.0f);
  fProxyToALMemory.insertData(fALMemoryKeys[2],0.0f);
  fProxyToALMemory.insertData(fALMemoryKeys[3],0.0f);


  // Do not call the function setClientPreferences in your constructor!
  // setClientPreferences : can be called after its construction!
  audioDevice->callVoid("setClientPreferences",
                        getName(),                //Name of this module
                        48000,                    //48000 Hz requested
                        (int)ALLCHANNELS,         //4 Channels requested
                        1                         //Deinterleaving requested
                        );
#ifdef SOUNDPROCESSING_IS_REMOTE
  qi::Application::atStop(boost::bind(&ALSoundProcessing::stopDetection, this));
#endif
  startDetection();
}

ALSoundProcessing::~ALSoundProcessing()
{
  stopDetection();
}


/// Description: The name of this method should not be modified as this
/// method is automatically called by the AudioDevice Module.
void ALSoundProcessing::process(const int & nbOfChannels,
                                const int & nbOfSamplesByChannel,
                                const AL_SOUND_FORMAT * buffer,
                                const ALValue & timeStamp)
{
  /// Computation of the RMS power of the signal delivered by
  /// each microphone on a 170ms buffer
  /// init RMS power to 0
  std::vector<float> fMicsEnergy;
  for(int i=0 ; i<nbOfChannels ; i++)
  {
    fMicsEnergy.push_back(0.0f);
  }

  /// Calculation of the RMS power
  for(int channelInd = 0 ; channelInd < nbOfChannels ; channelInd++)
  {
    for(int i = 0 ; i < nbOfSamplesByChannel ; i++)
    {
      fMicsEnergy[channelInd] += (float)buffer[nbOfSamplesByChannel*channelInd + i]
                                 *(float)buffer[nbOfSamplesByChannel*channelInd + i];
    }
    fMicsEnergy[channelInd] /= (float)nbOfSamplesByChannel;
    fMicsEnergy[channelInd] = sqrtf(fMicsEnergy[channelInd]);
  }

  /// Puts the result in ALMemory
  /// (for example to be easily retrieved by another module)
  for(int i=0 ; i<nbOfChannels ; i++)
  {
    fProxyToALMemory.insertData(fALMemoryKeys[i],fMicsEnergy[i]);
  }

  /// Displays the results on the Naoqi console
  std::cout << "ALSoundProcessing Energy Left  = " <<  fMicsEnergy[0] << std::endl;
  std::cout << "ALSoundProcessing Energy Right  = " << fMicsEnergy[1] << std::endl;
  std::cout << "ALSoundProcessing Energy Front  = " << fMicsEnergy[2] << std::endl;
  std::cout << "ALSoundProcessing Energy Rear  = " <<  fMicsEnergy[3] << std::endl;
}
```


可以在音频示例目录中找到这些文件对应的工程（可以轻松编译）。

http://doc.aldebaran.com/2-1/dev/cpp/tutos/create_module.html

## 编译 C++ 模块（Module）遇到的问题 

1.准备工作

操作系统是 Ubuntu 16.04 Xenial Xerus - 64bits 。 
环境配置完全参考了 http://doc.aldebaran.com/2-8/dev/cpp/install_guide.html
C++ SDK 和 Python SDK 都配置完全且可以使用。

NAO 机器人已经正常开机，处于自主生活模式，和计算机处于同一局域网下。

2.源代码，来自提供的资料光盘：设备随装资料-23.3/3-NAO机器人/3-技术资料/示例代码/C++/soundprocessing.zip 。

3.远程模块编译过程：
参考了
在 soundprocessing 目录中执行配置 `qibuild configure` ：
```bash
nao@nao-desktop:~/Downloads/mywtree/soundprocessing$ qibuild configure
```
输出结果
```bash
Current build worktree: /home/nao/Downloads/mywtree
Using toolchain: linux
* (1/1) Configuring soundprocessing
-- The C compiler identification is GNU 5.4.0
-- The CXX compiler identification is GNU 5.4.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Using qibuild 3.18
-- Binary: soundprocessing
-- Configuring done
-- Generating done
CMake Warning:
  Manually-specified variables were not used by the project:

    WITH_QIBUILD


-- Build files have been written to: /home/nao/Downloads/mywtree/soundprocessing/build-sys-linux_x86_64

```
然后执行 `qibuild make` 编译
```bash
nao@nao-desktop:~/Downloads/mywtree/soundprocessing$ qibuild make
```
输出结果
```bash
...(报很多warnings，限于篇幅就不贴了)

[100%] Linking CXX executable sdk/bin/soundprocessing
[100%] Built target soundprocessing
nao@nao-desktop:~/Downloads/mywtree/soundprocessing$ 
```
然后在 /soundprocessing/tests/ 目录下，执行提供的测试脚本 MySoundProcessingModuleTest.py，注意这里的IP地址已经改成NAO的IP：
```python
# -*- coding: utf-8 -*-
# MySoundProcessingModuleTest.py
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

结果似乎提示找不到自定义的 ALSoundProcessing
```bash

nao@nao-desktop:~/Downloads/mywtree/soundprocessing/tests$ python ./MySoundProcessingModuleTest.py 
[W] 1684757167.823390 25761 qi.path.sdklayout: No Application was created, trying to deduce paths
[I] 1684757167.825496 25761 qimessaging.session: Session listener created on tcp://0.0.0.0:0
[I] 1684757167.825802 25761 qimessaging.transportserver: TransportServer will listen on: tcp://127.0.0.1:32769
[I] 1684757167.825814 25761 qimessaging.transportserver: TransportServer will listen on: tcp://192.168.3.20:32769
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
4. 继续尝试编译本地模块
由于 2-5、2-8 版本的在线手册都没有提到本地模块的编译方法，这里参考了 2-1 版本手册的内容：http://doc.aldebaran.com/2-1/dev/cpp/tutos/create_module.html

将 CMakeList.txt 中第7行
```cmake
option(SOUNDPROCESSING_IS_REMOTE
  "module is compiled as a remote module (ON or OFF)"
  ON)
```
中的 `ON` 改为 `OFF`，然后再次执行

```bash
nao@nao-desktop:~/Downloads/mywtree/soundprocessing$ qibuild configure
``` 
输出
```bash
Current build worktree: /home/nao/Downloads/mywtree
Using toolchain: linux
* (1/1) Configuring soundprocessing
-- Using qibuild 3.18
-- Library: soundprocessing
-- Configuring done
-- Generating done
CMake Warning:
  Manually-specified variables were not used by the project:

    CMAKE_TOOLCHAIN_FILE
    WITH_QIBUILD


-- Build files have been written to: /home/nao/Downloads/mywtree/soundprocessing/build-sys-linux_x86_64
```
然后还是make

```bash
nao@nao-desktop:~/Downloads/mywtree/soundprocessing$ qibuild make 
```
输出
```bash

... (很多warnings)

[ 75%] Linking CXX shared library sdk/lib/naoqi/libsoundprocessing.so
[100%] Built target soundprocessing
nao@nao-desktop:~/Downloads/mywtree/soundprocessing$ qibuild make 
```

然后用ssh拷贝/soundprocessing目录到NAO机器人中，存放目录为 /home/nao/test_cpp_202305/soundprocessing

然后修改 /home/nao/naoqi/preferences/autoload.ini ，添加
```ini
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
/data/home/nao/test_cpp_202305/soundprocessing/build-sys-linux_x86_64/sdk/lib/naoqi/libsoundprocessing.so

[python]
#the/full/path/to/your/python_module.py   # load python_module.py

[program]
#the/full/path/to/your/program            # load program

```
然后ssh执行
```bash
nao restart
```
等待NAO重新开机完毕

然后进入 /data/home/nao/test_cpp_202305/soundprocessing/tests 目录 ，修改IP地址为127.0.0.1，然后执行 MySoundProcessingModuleTest.py
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
似乎仍然提示找不到自定义的 ALSoundProcessing