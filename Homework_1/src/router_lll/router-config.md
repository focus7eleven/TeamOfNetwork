## 路由器配置
### 摘要
本论文主要涉及路由器的配置相关内容，主要包括路由器基础、路由器配置、cisco ios使用方法、路由器常用配置和路由器配置实验五个方面。  
路由器基础中主要从路由器的功能和cisco路由器系统组成两方面做了介绍，描述了其包括的内容以及主要关注点。  
路由器配置主要从配置途径、ios的启动与系统配置对话和路由器状态以及配置模式三方面进行了解析，给出了详细的配置步骤。  
cisco ios使用方法模块，首先介绍了使用帮助的用法，然后介绍了命令行的注释和默认配置，如何显示路由器状态和查看相邻的网络设备，重点介绍了ios及配置文件的备份，同时还介绍了配置控制、改变工作模式命令、口令管理以及路由器测试命令相关内容。  
在路由器常用配置模块，主要介绍了ip协议的配置、ip路由配置、路由协议配置、广域网协议配置和NAT配置的相关原理，并对相关配置命令格式进行了解析。  
在路由器配置实验中，通过具体的实验拓扑图为指导，描述了各种协议的具体配置过程。  
本文从各个方面对路由器的配置进行了描述和解析，对于理解路由器的配置各方面的原理以及具体如何配置有很好的参考价值。  

### 路由器基础
#### 路由器功能
路由器是在网络层实现互联的设备。路由器实现网络层上数据包的存储转发，它具有路径选择功能，可依据网络当前的拓扑结构，选择“最佳”路径，把接收的数据包转发出去，从而实现网络负载平衡，减少网络拥塞路由器工作在网络层，用于连接不同的局域网和广域网，故称为“LAN网间互联设备”。一个路由器可以连接两个局域网、一个局域网和一个广域网，或两个广域网。  

路由器的具体功能如下：
- 路由功能（寻径功能）——寻找并记录到达目的网段的最佳路径，体现在路由器上则包括路由表的建立、维护和查找；
- 交换功能——路由器的交换功能与以太网交换机执行的交换功能不同，路由器的交换功能是指在网络之间转发分组数据的过程，涉及到从接收接口收到数据帧，解封装，对数据包做相应处理，根据目的网络查找路由表，决定转发接口，做新的数据链路层封装等过程。
- 隔离广播、指定访问规则——路由器阻止广播的通过，并且可以设置访问控制列表(ACL)对流量进行控制。
- 异种网络互连——支持不同的数据链路层协议，可以连接异种网络。
- 子网间的速率匹配——路由器有多个接口，不同接口具有不同的速率，路由器需要利用缓存及流控协议进行速率适配。  

路由器的主要任务是把通信引导到目的地网络，然后到达特定的节点站地址。后一个功能是通过网络地址分解完成的。例如，把网络地址部分的分配指定成网络、子网和区域的一组节点，其余的用来指明子网中的特别站。分层寻址允许路由器对有很多个节站的网络存储寻址信息。在广域网范围内的路由器按其转发报文的性能可以分为两种类型，即中间节点路由器和边界路由器。尽管在不断改进的各种路由协议中，对这两类路由器所使用的名称可能有很大的差别，但所发挥的作用却是一样的。中间节点路由器在网络中传输时，提供报文的存储和转发。同时根据当前的路由表所保持的路由信息情况，选择最好的路径传送报文。由多个互连的LAN组成的公司或企业网络一侧和外界广域网相连接的路由器，就是这个企业网络的连界路由器。它从外部广域网收集向本企业网络寻址的信息，转发到企业网络中有关的网络段；另一方面集中企业网络中各个LAN段向外部广域网发送的报文，对相关的报文确定最好的传输路径。  
#### cisco路由系统的组成
<div align="center">
<img src="https://github.com/focus7eleven/TeamOfNetwork/blob/master/Homework_1/src/router_lll/cisco-route-system.png?raw=true" style="max-width:700px;"/>
<p align="center">图1.1 cisco路由系统组成</p>
</div>

##### cpu  
与计算机一样，路由器也包含了一个中央处理器（CPU）。不同系列和型号的路由器，其中的CPU也不尽相同。Cisco路由器一般采用Motorola 68030和Orion/R4600两种处理器。  
路由器的CPU负责路由器的配置管理和数据包的转发工作，如维护路由器所需的各种表格以及路由运算等。路由器对数据包的处理速度很大程度上取决于CPU的类型和性能。   
##### 存储器  
ROM:存储开机诊断程序，用于引导操作系统，类似于计算机的BIOS  
RAM:路由器的主存储器，存放Running-config，路由器，ARP表，类似于计算机的内存。  
FLASH:路由器的快闪存储器，用于存放路由器的IOS，类似于计算机硬盘。  
NVRAM:非易失存储器，用于放置启动配置文件Startup-Config文件  

##### 接口  
<div align="center">
<img src="https://github.com/focus7eleven/TeamOfNetwork/blob/master/Homework_1/src/router_lll/router-interface.png?raw=true" style="max-width:700px;"/>
<p align="center">图1.1 路由器接口</p>
</div>

所有路由器都有接口（Interface），每个接口都有自己的名字和编号。一个接口的全名称由它的类型标志与数字编号构成，编号自0开始。  
对于接口固定的路由器（如Cisco 2500系列）或采用模块化接口的路由器（如Cisco 4700系列），在接口的全名称中，只采用一个数字，并根据它们在路由器的物理顺序进行编号，例如Ethernet0表示第1个以太网接口，Serial1表示第2个串口。  
对于支持“在线插拔和删除”或具有动态更改物理接口配置的路由器，其接口全名称中至少包含两个数字，中间用斜杠“/”分割。其中，第1个数字代表插槽编号，第2个数字代表接口卡内的端口编号。如Cisco 3600路由器中，serial3/0代表位于3号插槽上的第1个串口。  
对于支持“万用接口处理器（VIP）”的路由器，其接口编号形式为“插槽/端口适配器/端口号”，如Cisco 7500系列路由器中，Ethernet4/0/1是指4号插槽上第1个端口适配器的第2个以太网接口。  
+ 控制台端口
 几乎所有路由器都在路由器背后安装了一个控制台端口。控制台端口提供了一个EIA/TIA—232(以前叫作RS—232)异步串行接口、使我们能与路由器通信。至于同控制台端口建立哪种形式的物理连接，则取决于路由器的型号。有些路由器采用一个DB25母连接(DB25F)，有些则用RJ45连接器。通常，较小的路由器采用RJ45控制台连接器，而较大路由器采用DB25控制台连接器。
- 辅助端口  
 大多数Cisco路由器都配备了一个“辅助端口”(Auxiliary Port)。它和控制台端口类似，提供了一个EIA／TIA—232异步串行连接，使我们能与路由器通信。辅助端口通常用来连接Modem，以实现对路由器的远程管理。远程通信链路通常并不用来传输平时的路由数据包，它的主要的作用是在网络路径或回路失效后访问一个路由器。

##### IOS  
IOS为CISCO的专有操作系统，功能有连接多种网络，用于不同协议的路由和转换，实现流量控制、QoS服务质量控制、网络安全服务，网络拨号及VPN等。  
- 基本特征集：为cisco路由器使用的基本功操作系统c2600-is
- 增强体征集：集成了硬件平台特性的增强功能操作系统c2600-io3
- 加密特征集：使用了加密技术的CISCO操作系统c2600-ipvoice  

有两种类型的IOS配置：
1. 运行配置：有时也称作“活动配置”，驻留于RAM，包含了目前在路由器中“活动”的IOS配置命令。配置IOS时，就相当于更改路由器的运行配置。
2. 启动配置：启动配置驻留在NVRAM中，包含了希望在路由器启动时执行的配置命令。有时也把启动配置称作“备份配置”。这是由于修改并认可了运行配置后，通常应将运行配置复制到NVRAM里，将作出的改动“备份”下来，以便路由器下次启动时调用。启动完成后，启动配置中的命令就变成了“运行配置”。  

两者均以ASCII文本格式显示。所以，我们能够很方便地阅读与操作。一个路由器只能从这两种类型中选择一种。
### 路由器配置
#### 路由器配置途径
1. 控制台
将PC机的串口直接通过Rollover线与路由器控制台端口Console相连，在PC计算机上运行终端仿真软件，与路由器进行通信，完成路由器的配置。也可将PC与路由器辅助端口AUX直接相连，进行路由器的配置。
2. 虚拟终端(Telnet)
　　如果路由器已有一些基本配置，至少有一个端口有效(如Ethernet口)，就可通过运行Telnet程序的计算机作为路由器的虚拟终端与路由器建立通信，完成路由器的配置。
3. 网络管理工作站
　　路由器可通过运行网络管理软件的工作站配置，如Cisco的CiscoWorks、HP的OpenView等。
4. CISCO　ConfigMaker
ConfigMaker是一个由CISCO开发的免费的路由器配置工具。ConfigMaker采用图形化的方式对路由器进行配置，然后将所做的配置通过网络下载到路由器上。ConfigMaker要求路由器运行在IOS　11.2以上版本，可用Show Version命令查看路由器的版本信息。
5. TFTP(Trivial File Transfer Protocol)服务器
TFTP是一个TCP/IP简单文件传输协议，可将配置文件从路由器传送到TFTP服务器上，也可将配置文件从TFTP服务器传送到路由器上。TFTP不需要用户名和口令，使用非常简单。  

注意：路由器的第一次设置必须通过第一种方式进行；这时终端的硬件设置为波特率：9600，数据位：8，停止位：1，无校验。
#### ios的启动和系统配置对话
(1)ios的启动  
 ios的正确启动包含以下几个步骤：  
1.	自ROM执行上电、检测CPU、内存、接口电路；
2. 引导操作系统（由配置寄存器的引导域决定：FLASH或网络下载，Boot system命令确定）
3. 自ROM执行引导，将操作系统下载到主存；
4. 操作系统下载到低地址内存，下载后操作系统确定路由器的工作，硬件和软件部分，分别在屏幕上显示其结果。
5. NVRAM中存储的配置文件装载到主内存，并检测通行词，配置命令启动路由进程，提供接口地址、设置介质特性。如果NVRAM中没有有效的配置文件，则进入setup会话模式。
6. 进入系统配置会话，提示配置信息，如每个接口的配置信息  

(2)系统配置对话  
cisco路由器的启动和系统配置对话过程如下：
1. 用CISCO随机带CONSOLE线，一端连在CISCO路由器的CONSOLE口,一端连在计算机的COM口。
2.  打开电脑，启动超级终端.为您的连接取个名字,比如CISCO_SETUP,下一步选定连接时用COM1,下一步选定第秒位数9600,数据位8,奇偶校验无，停止位1,数据流控制无.最后选确定。
3.  打开路由器电源，这时超级终端将出现以下画面：

```
System Bootstrap, Version 11.1(20)AA2, EARLY DEPLOYMENT RELEASE SOFTWARE (fc1)  Copyright (c) 1999 by cisco Systems, Inc.C3600 processor with 32768 Kbytes of main memory Main memory is configured to 64 bit mode with parity disabled  program load complete, entry point: 0x80008000, size: 0x4ed478 Self decompressing the image :   [OK]   Restricted Rights Legend   Use, duplication, or disclosure by the Government is   subject to restrictions as set forth in subparagraph 　(c) of the Commercial Computer Software - Restricted 　　Rights clause at FAR sec. 52.227-19 and subparagraph 　　(c) (1) (ii) of the Rights in Technical Data and Computer 　　Software clause at DFARS sec. 252.227-7013. 　　cisco Systems, Inc. 　　170 West Tasman Drive 　　San Jose, California 95134-1706 　　Cisco Internetwork Operating System Software 　　IOS (tm) 3600 Software (C3640-I-M), Version 12.1(2)T, RELEASE SOFTWARE (fc1) 　　Copyright (c) 1986-2000 by cisco Systems, Inc. 　　Compiled Tue 16-May-00 12:26 by ccai 　　Image text-base: 0x600088F0, data-base: 0x60924000 　　cisco 3640 (R4700) processor (revision 0x00) with 24576K/8192K bytes of memory. 　　Processor board ID 25125768 　　R4700 CPU at 100Mhz, Implementation 33, Rev 1.0 　　Bridging software. 　　X.25 software, Version 3.0.0. 　　2 FastEthernet/IEEE 802.3 interface(s) 　　1 Serial network interface(s) 　　DRAM configuration is 64 bits wide with parity disabled. 　　125K bytes of non-volatile configuration memory. 　　8192K bytes of processor board System flash (Read/Write) 　　
--- System Configuration Dialog --- 　　
Would you like to enter the initial configuration dialog? [yes/no]: y 　　
<!--您是否进入初始化配置对话，选Y-->
At any point you may enter a question mark '?' for help.
Use ctrl-c to abort configuration dialog at any prompt.
Default settings are in square brackets '[]'.Basic management setup configures only enough connectivity
for management of the system, extended setup will ask you
to configure each interface on the system
Would you like to enter basic management setup? [yes/no]: n
<!--您是否进入基本配置安装，选N-->
First, would you like to see the current interface summary? [yes]: y
<!--首先，您是否看一下当前端口状态-->
Any interface listed with OK? value "NO" does not have a valid configuration
Interface IP-Address OK? Method Status Protocol
FastEthernet0/0unassigned NO unset up down
Serial0/0 unassigned NO unset down down
FastEthernet0/1unassigned NO unset up down
Configuring global parameters:
Enter host name [Router]:RouterA 　
<!--输入路由器的名字-->
The enable secret is a password used to protect access to
privileged EXEC and configuration modes. This password, after
entered, becomes encrypted in the configuration.
Enter enable secret: aaa
<!--输入密文-->
The enable password is used when you do not specify an
enable secret password, with some older software versions, and
some boot images.
Enter enable password: bbb 　
<!--输入密码(不能和密文相同)-->
The virtual terminal password is used to protect
access to the router over a network interface.
Enter virtual terminal password: ccc 　
<!--输入虚拟终端的密码(以备远程登录)-->
Configure SNMP Network Management? [yes]: n
<!--配置简单网管吗?选N-->
Configure IP? [yes]: y
<!--配置IP吗?选Y-->
Configure IGRP routing? [yes]: n
<!--配置IGRP路由选择协议吗?选N-->
Configure RIP routing? [no]: 　n
<!--配置RIP路由选择协议吗?选N-->
Configure bridging? [no]: n
<!--配置桥接吗?选N-->
Async lines accept incoming modems calls. If you will have
users dialing in via modems, configure these lines. 　
Configure Async lines? [yes]: n
<!--配置异步线路吗?选N-->
Configuring interface parameters:
Do you want to configure FastEthernet0/0 interface? [yes]: y
<!--您是否想配置fastethernet0/0接口?选Y-->
Use the 100 Base-TX (RJ-45) connector? [yes]: y
<!--用RJ45的连接器吗?选Y-->
Operate in full-duplex mode? [no]: y
<!--选用全双工模式?选Y-->
Configure IP on this interface? [yes]: y
<!--在这个接口上配置IP吗?选Y-->
IP address for this interface: 192.168.0.1
<!--配置该接口的IP地址(在此地址为192.168.0.1-->
Subnet mask for this interface [255.255.255.0] :
<!--配置该接口的子网掩码.(默认的是255.255.255.0,可以手工输入修改)-->
Class C network is 192.168.0.0, 24 subnet bits; mask is /24
Do you want to configure Serial0/0 interface? [yes]: y
<!--您想配置serial0/0接口吗?选Y-->
Some supported encapsulations are
ppp/hdlc/frame-relay/lapb/x25/atm-DXi/smds
Choose encapsulation type [hdlc]:
选择封装方式(默认的封装方式是HDLC,您可根据与您的路由器相连选用的封装类型来决定用什么样的封装类型
No serial cable seen. 　　
Choose mode from (dce/dte) [dte]:
(因为没有连串口线所以会让您选择设备类型)
Configure IP on this interface? [yes]: y
(在接口上配置IP)
Configure IP unnumbered on this interface? [no]: 　　
IP address for this interface: 172.16.0.5
配置该接口的IP地址(在此地址为172.16.0.5)
Subnet mask for this interface [255.255.0.0] : 255.255.255.252
配置该接口的子网掩码.(默认的是255.255.0.0,可以手工输入修改为255.255.255.252)
Class B network is 172.16.0.0, 30 subnet bits; mask is /30
(以下配置同上)
Do you want to configure FastEthernet0/1 interface? [yes]:
Use the 100 Base-TX (RJ-45) connector? [yes]:
Operate in full-duplex mode? [no]: y
Configure IP on this interface? [yes]: y
IP address for this interface: 172.16.0.9
Subnet mask for this interface [255.255.0.0] : 255.255.255.252 Class B network is 172.16.0.0, 30 subnet bits; mask is /30 　
The following configuration command script was created:
(把您的配置显示出来)
hostname aaa
enable secret 5 $ul/V$ezbZFgvzGHD.YPSieC0Ew/
enable password RouterA
line VTY 0 4
password ccc
no snmp-server
!
ip routing
no bridge 1
!
interface FastEthernet0/0
media-type 100BaseX
full-duplex
ip address 192.168.0.1 255.255.255.0
!
interface Serial0/0
encapsulation hdlc
ip address 172.16.0.5 255.255.255.252
!
interface FastEthernet0/1
media-type 100BaseX
full-duplex
ip address 172.16.0.9 255.255.255.252
dialer-list 1 protocol ip permit
dialer-list 1 protocol ipx permit
!
end
以下提示您是否保存这次设置 　　
[0] Go to the IOS command prompt without saving this config.
[1] Return back to the setup without saving this config.
[2] Save this configuration to nvram and exit.
Enter your selection [2]: 2
选择2保存设置并存入NVRAM中
Building configuration...
[OK] Use the enabled mode 'configure' command to modify this configuration.
Press RETURN to get started

```
#### 路由器状态以及配置模式
路由器的配置模式是通过控制台连接路由器进入的模式，该模式下路由器有以下几个状态。
1. 用户命令状态  
  前置符类似“Router>”，此时路由器处于用户命令状态，这时用户可以看路由器的连接状态，访问其它网络和主机，但不能看到和更改路由器的设置内容。
2. 特权命令状态  
  前置符类似“Router#”，用户命令状态下输入“enable”即可进入，此时路由器处于特权命令状态，这时不但可以执行所有的用户命令，还可以看到和更改路由器的设置内容。
3. 全局设置状态  
  前置符类似“Router(config)#”，特权命令状态下输入“configure terminal”即可进入，此时路由器处于全局设置状态，这时可以进行路由器端口以外的一些设置，如：路由协议，nat等。
4. 局部设置状态  
  从全局设置状态进入，对某个功能的详细设置，这时可以设置路由器某个局部的参数。
5. RXBOOT状态  
  前置符为“>”，在开机后60秒内按ctrl-break可进入此状态，这时路由器不能完成正常的功能，只能进行软件升级和手工引导。
6. 设置对话状态  
   这是一台新路由器开机时自动进入的状态，在特权命令状态使用SETUP命令也可进入此状态。这时可通过对话方式对路由器进行设置。  

### cisco ios使用方法
Cisco IOS（Intenetwork Operating System）是运行于Cisco路由器和交换机上的操作系统，其能提供多种网络服务，进而支持多种网络应用。IOS是被用来传送网络服务并启动网络应用的。能用来加载网络协议和功能、在设备间连接高速流量、在控制访问中添加安全性防止未授权的网络使用、为简化网络的增长和冗余备份，提供可缩放性、为连接到网络中的资源，提供网络的可靠性。  
#### 使用帮助
在电脑上装好Packet Tracer之后，即可以使用IOS命令了。在Packet Tracer中拖动路由器图标到主串口，双击路由器即可打开配置窗口。第三个Tab页就是使用IOS命令的命令行工具。在这里输入命令即可得到执行。  
在用户模式下输入？即可得到命令帮助。如下图：
<div align="center">
<img src="https://github.com/focus7eleven/TeamOfNetwork/blob/master/Homework_1/src/router_lll/user-mode.png?raw=true" style="max-width:700px;"/>
</div>

在特权模式下输入？同样可以得到帮助命令，帮助命令稍多。
<div align="center">
<img src="https://github.com/focus7eleven/TeamOfNetwork/blob/master/Homework_1/src/router_lll/special-model.png?raw=true" style="max-width:700px;"/>
</div>

关于用户模式和特权模式请见6、改变工作模式命令。
#### 命令行的注释和默认设置

1. 注释：很多命令都带有参数，用户输入命令的时候，命令行工具会执行输入命令，会检查命令的正确与否。在1、使用帮助中我们看到了使用？可以查看命令，同样可以看到输入命令的解释和参数的情况。比如使用常用的ping命令,可以看到起参数后面是一个ip地址。  
<div align="center">
<img src="https://github.com/focus7eleven/TeamOfNetwork/blob/master/Homework_1/src/router_lll/ping-more.png?raw=true" style="max-width:700px;"/>
</div>

2. 默认设置：路由器启动的时候，IOS命令行会自动输出默认设置。我们可以关闭路由器然后开启，就可以看到路由器的默认配置。
<div align="center">
<img src="https://github.com/focus7eleven/TeamOfNetwork/blob/master/Homework_1/src/router_lll/router-default-config.png?raw=true" style="max-width:700px;"/>
</div>

图中包含了路由器的型号、内存使用情况、处理器板ID、NVRAM的大小等一系列路由器的默认设置信息。

#### 实现路由器状态和查看相邻的网路设备
路由器的状态影响着网络的工作情况，所以经常需要了解路由器的状态，比如端口是开启还是关闭、设备是否连接通等问题。这些命令包括：
- show running-config显示当前运行在RAM中的路由器配置。
- show startup-config显示存储在NVRAM中的路由器配置。这是在路由器接通电源的时候使用的配置，除非进行了其他特殊的配置。
- show flash 显示闪存中的格式和内容，包括闪存中的IOS映像的文件名称。
- Show buffers 显示路由器上的缓冲区的统计信息。
- show mem显示路由器内存的统计信息，包括自由库统计信息。
- Show processec cpu显示路由器中正在运行的活动过程或程序的统计信息。
- show protocols 显示路由器中配置的所有协议的信息，以及在每个接口上配置的关于网络层地址的信息。
- Show stacks 显示关于过程对内存堆栈的使用以及中断例程的信息，还包括上次系统重新启动的原因。
- show version显示系统硬件的配置信息、软件版本、配置文件的名称和来源，以及启动映像。
- show ip route显示IOS当前选择到达目的网路的最佳路径所使用的路由表
- Show interfaces后面还跟有参数，显示某接口的具体情况   

上面我们用于查看路由器组件和过程状态的命令总称为SHOW命令。下面我们用一个简易的拓扑来检查路由器状态。  
<div align="center">
<img src="https://github.com/focus7eleven/TeamOfNetwork/blob/master/Homework_1/src/router_lll/network-topology1.png?raw=true" style="max-width:700px;"/>
</div>

使用show interface为例来查看路由器接口状态：
<div align="center">
<img src="https://github.com/focus7eleven/TeamOfNetwork/blob/master/Homework_1/src/router_lll/show-interface.png?raw=true" style="max-width:700px;"/>
</div>

从上图中可以看出，第一行显示Router0路由器的Serial0/1/0端口已经开启，第二行显示硬件版本是HD64570，第三行显示网络地址是192.168.1.1/24，第五行显示封装协议是HDLC，回环地址没有设置。  
除了关注自己的配置，路由器还需要查看相邻的网络设备的各种状态。连接端口的情况可以通过上述show interfaces命令查看。另外，需要通过使用  查看邻居路由 命令：
```
show cdp interface
show cdp neighbors [detail]
show cdp entry routerA
```
通过这三个命令可以查看邻居的路由。cdp是Cisco Discovery protocol，思科发现协议。下图是用上图的网络拓扑图中的Router0进行查看相邻的网络设备：
<div align="center">
<img src="https://github.com/focus7eleven/TeamOfNetwork/blob/master/Homework_1/src/router_lll/cdp.png?raw=true" style="max-width:700px;"/>
<p align="center">图1.1 cdp查看相邻网路设备</p>
</div>

 如图所示，在Router0的控制台上输入“show cdp neighbors”命令后，控制台输出与Router0相邻的网络设备Router1，其中的Local Intrfce表示路由器上的接口，Capability包括了路由器、交换机、主机等类型，Port ID表示连接在远程路由器上的接口。
默认cisco串行接口为DTE设备，但也可以将他们配置为DCE设备。DTE电缆的连接器为插头型，而DCE电缆的连接器为插孔型，可以通过“show controllers”命令确定路由器连接的电缆是DCE还是DTE，DCE设备需要通过“clock rate”命令来设置时钟。
```
R1# show controllers serial 0/0/0
	interface Serial0/0/0
	Hardware is PowerQUICC MPC860
	DCE V.35, no clock
R1(config)# interface serial 0/0/0
R1(config-if)# clock rate 64000

```

 ##### ios及配置文件的备份
为了方便管理，我们可以 把IOS系统及设备配置文件备份在本地计算机上，当出故障时，我们可以把IOS系统或配置文件恢复到Cisco设备上。  
路由器可以将配置信息复制到TFTP服务器上，或从TFTP服务器上复制配置信息。这使网络管理员可以将配置信息保存在服务器上，以跟踪配置、监视修改或者灾难恢复。如果配置大于32000字节，则需要将配置保存在TFTP服务器上，因为32000字节是NVRAM可以保存的最大配置文件。当用TFTP将配置文件传送到路由器时，可以将其放置在闪存、NVRAM或RAM内存中。当将配置放置在闪存中时，仍然需要将其放置在NVRAM或RAM，目的是路由器可以使用它。COPY TETP命令可以通过控制台或VTY会话来执行。  
复制配置文件到TFTP服务器或从TFTP服务器复制配置文件的命令如下：
- Copy tftp running-config直接从TFTP服务器复制配置文件到RAM，而配置路由器。
- Copy tftp startup-config用来自TFTP服务器的文件覆盖存储在NVRAM中的配置文件。
- Copy running-config tftp在TFTP服务器上对RAM中的路由器正在运行的配置进行复制。
- Copy startup-config tftp将NVRAM中的配置复制到TFP服务器上。
- Copy flash tftp把flash存储器内的ios文件备份到TFTP server上。
- Earase 命令是删除配置文件，慎用。
路由器的配置参数较多，可根据实际需要增减。同时在1使用帮助中提过？帮助命令，可以查看copy命令的帮助和解释以及参数的情况，根据实际情况，做相应的备份。

#### 配置控制
使用Cisco ios配置路由器的命令和过程是复杂的，有时候需要保存此次的配置文件以便下次使用，或者载入上次的同样配置文件继续做模拟实验，需要对配置进行控制。  
在使用有些软件时可以通过一些界面化操作来载入和导出配置文件。如packet tracer的界面化操作：  
<div align="center">
<img src="https://github.com/focus7eleven/TeamOfNetwork/blob/master/Homework_1/src/router_lll/packet-tracer.png?raw=true" style="max-width:700px;"/>
<p align="center">图1.1 packet tracer界面</p>
</div>

如果不是模拟软件，则需要使用配置导入命令来导入配置文件。在3.4ios及配置文件的备份中我们已经了解到了配置文件如何备份。接下来描述配置文件恢复和载入到当前的路由器中。
1. 确保tftp服务器处于开启状态，以及要导入的配置文件正确无误，适合路由器本身的各项参数;
2. 清除路由器原来的startup-config文件,并reload路由器;  
配置路由器Fastenternet口的临时IP地址，保证和tftp服务器正常通信;  
开启路由器并进入全局模式，将tftp服务器上的配置文件复制到路由器上;  
执行的命令copy tftp startup-config;
3. 接着输入tftp服务器的IP地址，例如：172.16.20.223;
4. 输入要导入的文件名，要和服务器上的文件名保持一致;
5. 将startup-config文件复制到running-config中，copy startup-config
running-config;
6. 在路由器上show running，查看路由信息是否与导入的配置信息保持一致;

为了更方便的进行各种操作，更好的在IOS中管理配置文件，需要了解以下命令：
- dir 这条指令用来显示文件夹下的文件列表，输入dir ? 可以查看可选参数。
- cd 改变路径，改变当前所在的路径。
- copy 这个命令用来将 IOS 或一个配置文件拷贝到某处。可以用这个命令将路由器配置文件拷贝到 TFTP服务器上，或者拷贝到路由器里的某个文件夹中作为备份。还可以用 copy 命令将新的IOS 文件从TFTP服务器拷贝到路由器里，实现路由器升级。
- delete 和 rm 两个命令都很简单 delete 用来删除文件， rm 用来删除文件夹。
- show flash 用来显示flash中的文件。show flash 命令和 dir flash 命令类似，但是前者比后者显示出的信息更丰富一些，即多出了flash内存大小和类型信息。
- erase 和 format 需要知道应该format flash中的文件系统，而erase nvram里的文件系统。其余文件则根据其类型既可以erase又可以format。erase 命令大多数时候都是用在清除路由器配置，恢复出场配置的情况。具体的命令就是 erase startup-configuration。
- more 这个命令可以显示文本/配置文件的内容。比如你想查看一个备份的配置文件，就可以使用more 命令来查看该文件的内容。
- verify 这个个命令用来核查或者计算一个文件的MD5校验和。
- mkdir 和DOS环境一样，可以在路由器中使用 mkdir 命令创建文件夹。一般用这个命令来创建备份文件夹，用来存储配置文件或者ISO文件的备份。
- fsck  FAT 文件系统检测主要是用来检测flash文件系统的完整性。如果你感觉ISO文件有损坏，可以通过这个命令对文件系统进行检查。

#### 改变工作模式命令
路由器配置工作模式分为用户(USER)模式、特权(privileged)模式、全局模式、端口配置模式和协议配置模式。
1. USER模式的特性：用户模式仅允许基本的监测命令，在这种模式下不能改变路由器的配置，router>的命令提示符表示用户正处在USER模式下。
2. privileged模式的特性：特权模式可以使用所有的配置命令，在用户模式下访问特权模式一般都需要一个密码，router#的命令提示符表示用户正处在特权模式下。
3. 全局模式：全局模式是用于用户配置协议及端口的属性，router(config)#的命令提示符表示用户正处在全局模式下。
4. 端口配置模式：端口配置模式是用于配置端口属性，router(config-if)#的命令提示符表示用户正处在端口配置模式下。
5. 协议配置模式：协议配置模式是用于配置路由协议，router(config-router)#的命令表示提示符表示用户正处在路由协议配置模式下。

模式切换如下：
- 从用户模式router>切换到特权模式router#，使用如下命令:router>enable(第一次启动路由器时不需要密码)，这时，路由器的命令提示符变为router#。
- 从特权模式router#切换到用户模式router>，使用如下命令:router#exit(第一次启动路由器时不需要密码)，这时，路由器的命令提示符变为router>。
- 从特权模式router#切换到全局模式router(config)#，使用如下命令:router#conf t，这时，路由器的命令提示符变为router(config)#。
- 从全局模式router(config)#切换到特权模式router#，使用如下命:router(config)#
exit，这时，路由器的命令提示符变为router#。
- 从全局模式router(config)#切换到端口配置模式router(config-if)#，使用如下命令:router#int f0/0(f0/0可以是其它端口，比如s0/0)，这时，路由器的命令提示符变为router(config-if)#。
- 从端口配置模式router(config-if)#切换到全局模式router(config)#，使用如下命令:router(config-if)#exit，这时，路由器的命令提示符变为router(config)#。
- 从全局模式router(config)#切换到协议配置模式router(config-router)#，使用如下命令: router(config)#router rip(rip可以是其它路由协议)，这时，路由器的命令提示符变为router(config-router)#。
- 从协议配置模式router(config-router)#切换到全局模式router(config)#，使用如下命令: router(config-router)#exit，这时，路由器的命令提示符变为router(config)#。

#### 口令管理
在CISCO路由器产品中，我们在最初进行配置的时候通常需要使用限制一般用户的访问。这对于路由器是非常重要的，在默认的情况下，我们的路由器是一个开放的系统，访问控制选项都是关闭的，任一用户都可以登陆到设备从而进行更进一步的攻击，所以需要我们的网络管理员去配置密码来限制非授权用户通过直接的连接、CONSOLE终端和从拨号MODEM线路访问设备。  
关于Cisco IOS 的登录密码以及权限分配设置：
```
enable password  pwd   初级密码，用于验证从用户模式到特权模式的验证  
enable secret pwd       MD5加密密码，同样用于从用户模式到特权模式  
enabl password level pwd 指定密码作用于哪个级别  
enable secret level xx     MD5加密密码，同样用于指定密码作用于哪个级别  
```
配置进入特权模式的密码和密匙：这两个密码是用来限制非授权用户进入特权模式。因为特权密码是未加密的，所以我们一般都推荐用户使用特权密匙，且特权密码仅在特权密匙未使用的情况下才会有效。  
```
配置：
    router(config)#enable password cisco
    命令解释：开启特权密码保护
    router(config)#enable secret cisco
    命令解释：开启特权密匙保护。
配置控制端口的用户密码
    router(config)#line console 0
    命令解释：进入控制线路配置模式。
    router(config-line)#login
    命令解释：开启登陆密码保护
    router(config-line)#password cisco
    命令解释:设置密码为cisco，这里的密码区分大小写。
配置辅助端口（AUX）的用户密码
    router(config)#line aux 0
    命令解释:进入辅助端口配置模式。
    router(config-line)#login
    命令解释：开启登陆密码保护
    router(config-line)#password cisco
    命令解释:设置密码为cisco，这里的密码区分大小写。
配置VTY(telnet)登陆访问密码
    router(config)#line vty 0 4
    命令解释:进入VTY配置模式。
    router(config-line)#login
    命令解释：开启登陆密码保护
    router(config-line)#password cisco
    命令解释:设置密码为cisco，这里的密码区分大小写。
```
通过在一个路由器上查看password命令的详细解释：
<div align="center">
<img src="https://github.com/focus7eleven/TeamOfNetwork/blob/master/Homework_1/src/router_lll/showpassword.png?raw=true" style="max-width:700px;"/>
</div>

注意这里：0 5 7 的 含义不同。0 为不加密，5 为MD5 。7 为Cisco自有加密算法（不牢靠，容易被破解）。  
service password-encryption 是使用7的加密方法加密存储在本地的所有密码。Cisco官方不推荐使用这个方法。  
下面是简单密码验证：  
console 验证配置：  
```
router(config)#line console 0
router(config-line)#password 123
router(config-line)#end
```
Vty验证配置
```
router(config)#line vty 0 4
router(config-line)#password 123
router(config-line)#end
```
待用户名和密码的验证配置
```
router(config)#username qsy secret 123
router(config)#line console 0
router(config-line)#login local
router(config-line)#end
```
当需要重新登录时，需要登录：
<div align="center">
<img src="https://github.com/focus7eleven/TeamOfNetwork/blob/master/Homework_1/src/router_lll/userLogin.png?raw=true" style="max-width:700px;"/>
</div>

可以使用show run命令查看配置的情况。  
分级的权限验证：如果用户只想让Level 7 的用户在特权模式下：clear counters 和reload
配置如下：  
```
privilege exec level 7 clear counters
privilege exec level 7 reload
enable secret level 7 xxxx
```
可以使用show run命令查看配置的情况。  

##### 路由器测试命令
Cisco IOS软件包括几个命令，它可以用于测试IP网络中的基本连接情况。Ping是一个工具，用于仅仅测试网络层的连接情况。它向目的地发送一系列的ICMP回送数据包，并跟踪目的地发送回的ICMP应答信号。我们可以在用户模式下使用ping的默认特性（5个100字节数据包，2秒钟暂停），但是如果处于特权模式，可以使用其他几个选项。这就是所谓的扩展ping。扩展ping的某些可以使用的其他选项包括：大小不同的数据包，增加暂停时间，一次发送多于5个数据包，在IP报头设置“不分段”位，甚至在其他协议中使用ping，例如IPX和Apple Talk。下图是ping目的主机的IP地址：
<div align="center">
<img src="https://github.com/focus7eleven/TeamOfNetwork/blob/master/Homework_1/src/router_lll/pingIP.png?raw=true" style="max-width:700px;"/>
</div>

5个感叹号说明路由器成功地收到了响应数据包。如果不是感叹号，而是点(句号)，则说明连接中断，这或者是因为ICMP回送要求从来就没有达到过目的地，或者是因为响应在网络上的某个地方被损坏了或者路由错误。测试网络层连接情况的另一个命令是TRACEROUTE命令。TRACEROUTE提供这样的信息，即通信量正在互连网上采取哪一条路径，这样的信息是按跳提供的，以及每个跳有多长。这里是一个TRACEROUTE输出的例子：  
<div align="center">
<img src="https://github.com/focus7eleven/TeamOfNetwork/blob/master/Homework_1/src/router_lll/traceRoute.png?raw=true" style="max-width:700px;"/>
</div>

### 路由器常用配置
#### IP协议配置
##### 配置以太网接口
在配置以太网接口时，我们需要为以太网接口配置IP地址及子网掩码来进行IP数据包的处理。默认情况下，以太网接口是管理性关闭的，所以在配置完成ip地址后，我们还需要激活接口。  
我们以一个简易的实例来说明以太网接口的IP协议配置。在这个实例中，我们需要为以太网接口配置192.168.0.1的IP地址并且激活接口。  
相关配置命令如下：
1. 在特权模式下进入全局配置模式。  
   router#configure terminal
2. 进入第一个以太网接口。  
   router(config)#interface f0/0
3. 这时，命令提示符变为router(config-if)#，在提示符后输入以下命令为接口配置私有IP地址192.168.0.1，使用默认子网掩码255.255.255.0。  
   router(config-if)#ip address 192.168.0.1 255.255.255.0
4. 在默认情况下，cisco路由器的接口是在关闭状态下的，我们需要键入“no shutdown”命令来激活接口。  
   router(config-if)#no shutdown

##### 配置串行端口
我们可以通过虚拟终端来配置一个串行接口，配置串行接口需要以下步骤：
 1. 在全局模式下键入命令“interface serial 0”进入到串行接口配置模式下。
 2. 每一个连接的串行接口都必须有一个IP地址和子网掩码来转发IP数据包，我们可以在接口配置模式下键入“ip address <IP address> <netmask>”的命令来配置串行接口的IP地址。
 3. 如果串行接口连接的是一个DCE设备，我们还需要为串行接口配置一个时钟频率，如果是DTE设备则不需要。默认情况下，cisco路由器是一个DTE设备，但是我们可以通过使用命令来将其配置成DCE设备。我们可以在串行接口配置模式下键入“clock rate”的命令来配置时钟频率，可利用的时钟频率有“1200、2400、9600、19200, 38400、56000、64000、72000、125000、148000、500000、800000、1000000、1300000、2000000或者4000000。
 4. 在默认情况下，cisco路由器的接口是在关闭状态下的，我们需要键入“no shutdown”命令来激活接口，如果因为管理的要求，需要关闭一个接口，可以在相应的接口模式下键入“shutdown”就可以管理性关闭这个接口了。  

#### IP路由配置
通过配置静态路由，用户可以人为地指定对某一网络访问时所要经过的路径,在网络结构比较简单，且一般到达某一网络所经过的路径唯一的情况下采用静态路由。  
相关配置命令如下：
```  
建立静态路由：ip route prefix mask {address | interface} [distance] [tag tag] [permanent]   
Prefix :所要到达的目的网络   
mask :子网掩码   
address :下一个跳的IP地址，即相邻路由器的端口地址。   
interface :本地网络接口   
distance :管理距离（可选）
tag tag :tag值（可选）
permanent :指定此路由即使该端口关掉也不被移掉。
```
以下在Router1上设置了访问192.1.0.64/26这个网下一跳地址为192.200.10.6，即当有目的地址属于192.1.0.64/26的网络范围的数据报，应将其路由到地址为192.200.10.6的相邻路由器。在Router3上设置了访问192.1.0.128/26及192.200.10.4/30这二个网下一跳地址为192.1.0.65。由于在Router1上端口Serial 0地址为192.200.10.5，192.200.10.4/30这个网属于直连的网，已经存在访问192.200.10.4/30的路径，所以不需要在Router1上添加静态路由。  
```
Router1:
ip route 192.1.0.64 255.255.255.192 192.200.10.6
Router3:
ip route 192.1.0.128 255.255.255.192 192.1.0.65
ip route 192.200.10.4 255.255.255.252 192.1.0.65
```
同时由于路由器Router3除了与路由器Router2相连外，不再与其他路由器相连，所以也可以为它赋予一条默认路由以代替以上的二条静态路由，  
ip route 0.0.0.0 0.0.0.0 192.1.0.65   
即只要没有在路由表里找到去特定目的地址的路径,则数据均被路由到地址为192.1.0.65的相邻路由器。
#### 路由协议配置
路由选择用于划分拥塞的网络，增加网络上允许的节点数目、过滤器数目和管理通信量。路由选择协议用于保证网络上的路由可以使用。度用于确定从起源路由器到目的网络之间的距离或代价。度是一个路由变量值，由路由选择协议计算。路由选择信息表由路由选择协议维护，以帮助选择到达某个目的网络的最佳路径。  
距离向量协议是比较古老的路由选择算法协议，它在最少跳的基础上选择路由。跳是在达到目的网络之前，必须经过的路由器的数量。距离相邻协议路由器向它们的相邻的路由器发送整个路由选择信息表。这个信息被拷贝到那个邻居的表中，并用新的跳进行重新计算，然后转发给其他的邻居。这意味着距离向量路由选择表是建立在第2手信息的基础之上的。距离向量协议由于其路由选择表计算的简单性和路由选择算法的简单性，而降低了CPU的开销。距离向量路由选择协议很容易被路由选择循环骚扰，但是通常通过水平分割、破坏逆转，或者暂停间隔以对付这种问题。  
链路状态协议用于大型网络。它们在路由选择中使用代价度，并且在链路状态数据库中保存路由选择信息。当一个链路状态路由器位于网络上时，它向它的邻居发送呼叫数据包。邻居用它所了解的和它相连的链路及相关的代价信息来响应。起源路由器在来自邻居的信息的基础上建立它自己的链路状态数据库。一个链路状态路由器将定期向它的邻居发送链路状态通告，包括那个路由器的链路和相关代价。每个邻居复制数据包，并且将LSA转发到下一个邻居，这个过程称为泛洪。因为路由器并不在泛洪LSA之前再次计算路由选择数据库，减少了收敛时间。
##### RIP配置
RIP(Routing information Protocol)是应用较早、使用较普遍的内部网关协议(Interior Gateway Protocol,简称IGP)，适用于小型同类网络，是典型的距离向量(distance-vector)协议。在TCP/IP协议中实际上有两个版本的RIP。版本1是原始的，版本2是更新的版本。版本2由于其增强的功能而得到广泛的使用。  
RIP通过广播UDP报文来交换路由信息，每30秒发送一次路由信息更新。RIP提供跳跃计数(hop count)作为尺度来衡量路由距离，跳跃计数是一个包到达目标所必须经过的路由器的数目。如果到相同目标有二个不等速或不同带宽的路由器，但跳跃计数相同，则RIP认为两个路由是等距离的。RIP最多支持的跳数为15，即在源和目的网间所要经过的最多路由器的数目为15，跳数16表示不可达。  
RIP相关配置命令如下：
- 指定使用RIP协议：router rip
- 指定RIP版本 ：version {1|2}
- 指定与该路由器相连的网络 ：network network
- 配置好以后使用“show ip route”或者“show ip protocols”命令进行查看。

##### IGRP配置
IGRP (Interior Gateway Routing Protocol)是一种动态距离向量路由协议，它由Cisco公司八十年代中期设计。使用组合用户配置尺度，包括延迟、带宽、可靠性和负载。 缺省情况下，IGRP每90秒发送一次路由更新广播，在3个更新周期内(即270秒)，没有从路由中的第一个路由器接收到更新，则宣布路由不可访问。在7个更新周期即630秒后，Cisco IOS 软件从路由表中清除路由。  
IGRP相关配置命令如下：
- 指定使用IGRP协议 ：router igrp autonomous-system1
- 指定与该路由器相连的网络 ：network network
- 指定与该路由器相邻的节点地址 ：neighbor ip-address
- 同样的，配置好以后使用“show ip route”或者“show ip protocols”命令进行查看。

##### OSPF配置
OSPF(Open Shortest Path First)是一个内部网关协议(Interior Gateway Protocol,简称IGP)，用于在单一自治系统(autonomous system,AS)内决策路由。与RIP相对，OSPF是链路状态路有协议，而RIP是距离向量路由协议。  
链路是路由器接口的另一种说法，因此OSPF也称为接口状态路由协议。OSPF通过路由器之间通告网络接口的状态来建立链路状态数据库，生成最短路径树，每个OSPF路由器使用这些最短路径构造路由表。  
OSPF配置命令如下：
- 指定使用OSPF协议 ：router ospf process-id1
- 指定与该路由器相连的网络 ：network address wildcard-mask area area-id2
- 指定与该路由器相邻的节点地址 ：neighbor ip-address
- 配置OSPF代价：ip ospf cost{代价}
- 修改链路状态通告再传输之间的秒数：ip ospf retransmit- interval {秒数}
- 规定传输链路状态通告时间：ip ospf transmit- delay {秒数}。
- 设置优先类：ip ospf priority {编号}规定呼叫数据包之间的秒数：ip ospf呼叫- interval {秒数}
- 确定某个路由器的呼叫数据包丢失的秒数：ip ospf dead interval {秒数}
- OSPF简单密码鉴别：ip ospf authentication - key {密匙}
- OSPF MD5鉴别：ip ospf mesagedigest - key {密匙标识符} MD5 {密匙}。

##### BGP配置
BGP（Border Gateway Protocol）是运行于 TCP 上的一种自治系统的路由协议。 BGP 是唯一一个用来处理像因特网大小的网络的协议，也是唯一能够妥善处理好不相关路由域间的多路连接的协议。 BGP 构建在 EGP 的经验之上。 BGP 系统的主要功能是和其他的 BGP 系统交换网络可达信息。网络可达信息包括列出的自治系统（AS）的信息。这些信息有效地构造了 AS 互联的拓朴图并由此清除了路由环路，同时在 AS 级别上可实施策略决策。  
BGP相关配置命令如下：
- 指定使用IGRP协议 ：router igrp autonomous-system1
- 指定与该路由器相连的网络 ：network network
- 指定与该路由器相邻的节点地址 ：neighbor ip-address remote-as number2
- 查看配置：show ip bgp
- 清除BGP协议：clear ip bgp

#### 广域网协议配置
##### HDLC配置
HDLC（High-Level Data Link Control）是点到点串行线路上（同步电路）的帧封装格式，其帧格式与以太网帧格式有很大的差别，HDLC帧没有源MAC地址和目的MAC地址。HDLC是CISCO路由器使用的缺省协议，一台新路由器在未指定封装协议时默认使用HDLC封装。  
配置方法如下：  
- 在特权模式下输入config t命令进入配置模式。
- 配置连接的WAN接口。如：interface serial 1,进入config-if模式。
- HDLC协议封装。输入encapsulation HDLC，按回车。
- 设置带宽。输入bandwidth 传输速率[kilobits/second]，如56K，则输入bandwidth 56。

##### PPP配置
PPP（Point to Point Protocol）和HDLC一样，PPP也是串行线路上（同步电路或者异步电路）的一种帧封装格式，但是PPP可以提供对多种网络层协议的支持。PPP支持认证、多线路捆绑、回拨、压缩等功能。PPP经过4个过程在一个点到点的链路上建立通信连接：
1. 链路的建立和配置协调：通信的发起方发送LCP帧来配置和检测数据链路
2. 链路质量检测：在链路已经建立、协调之后进行，这一阶段是可选的
3. 网络层协议配置协调：通信的发起方发送NCP帧以选择并配置网络层协议  

CHAP（Challenge Handshake Authentication Protocol）和PAP（Password Authentication Protocol）通常被用于在PPP封装的串行线路上提供安全性认证。使用CHAP和PAP认证,每个路由器通过名字来识别，可以防止未经授权的访问。  
配置方法如下：  
- 特权模式下输入config t命令，进入配置模式。
- 配置WAN接口。输入具体接口号，如interface serial 0，进入config –if模式。
- 封装PPP协议，输入encapsulation PPP。
- 设置带宽，输入bandwidth 带宽。
- Ctrl+Z，结束配置。

#### NAT配置
NAT（Network Address Translation，网络地址转换），当在专用网内部的一些主机本来已经分配到了本地IP地址（即仅在本专用网内使用的专用地址），但现在又想和因特网上的主机通信（并不需要加密）时，可使用NAT方法。  
这种方法需要在专用网连接到因特网的路由器上安装NAT软件。装有NAT软件的路由器叫做NAT路由器，它至少有一个有效的外部全球IP地址。这样，所有使用本地地址的主机在和外界通信时，都要在NAT路由器上将其本地地址转换成全球IP地址，才能和因特网连接。另外，这种通过使用少量的公有IP 地址代表较多的私有IP 地址的方式，将有助于减缓可用的IP地址空间的枯竭。  
NAT相关配置命令如下：  
- 接口配置命令：ip nat {inside|outside}
- 全局配置命令：ip nat inside source static local-ip global-ip
- 显示当前存在的NAT转换信息：show ip nat translations
- 查看NAT的统计信息：show ip nat statistics
- 显示当前存在的NAT转换的详细信息：show ip nat translations verbose
- 显示出每个被转换的数据包：debug ip nat
- 删除NAT映射表中的所有内容： Clear ip nat translations
- ip nat inside source list access-list-number pool pool-name [overload]：使用该命令定义访问控制列表与NAT内部全局地址池之间的映射
- ip nat outside source list access-list-number pool pool-name [overload]：使用该命令定义访问控制列表与NAT外部局部地址池之间的映射
- ip nat inside destination list access-list-number pool pool-name：使用该命令定义访问控制列表与终端NAT地址池之间的映射  

### 路由器配置实验
#### 静态路由协议的配置
<div align="center">
<img src="https://github.com/focus7eleven/TeamOfNetwork/blob/master/Homework_1/src/router_lll/static-route-config-experiment.png?raw=true" style="max-width:700px;"/>
<p align="center">图1.1 静态路由配置实验</p>
</div>

R1的配置：
```
R1#conf ter
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#int s0/0
R1(config-if)#ip address 192.168.1.1 255.255.255.0
R1(config-if)#clock rate 56000
R1(config-if)#no shut

R1(config)#ip route 192.168.2.0 255.255.255.0 192.168.1.2  //静态路由配置
```
R2的配置：
```
R2(config)#int s0/0
R2(config-if)#ip add 192.168.1.2 255.255.255.0
R2(config-if)#no shut

R2(config-if)#int s0/1
R2(config-if)#ip add 192.168.2.1 255.255.255.0
R2(config-if)#no shut
```
R3的配置：
```
R3(config)#int s0/0
R3(config-if)#ip address 192.168.2.2 255.255.255.0
R3(config-if)#clock rate 56000
R3(config-if)#no shut

R3(config)#ip route 192.168.1.0 255.255.255.0 192.168.2.1  //静态路由配置
```
此时网络连通，在R3执行ping 192.168.1.1指令能够成功Ping通。

#### 路由协议的配置实验
##### RIP配置实验
<div align="center">
<img src="https://github.com/focus7eleven/TeamOfNetwork/blob/master/Homework_1/src/router_lll/RIP-experiment.png?raw=true" style="max-width:700px;"/>
<p align="center">图1.1 RIP实验</p>
</div>

各端口的ip地址配置如上，此时要连通网络还需要配置RIP动态路由协议。
```
R1：
R1(config)#router rip
R1(config-router)#network 192.168.1.0

R2：
R2(config)#router rip
R2(config-router)#network 192.168.1.0
R2(config-router)#network 192.168.2.0

R3：
R3(config)#router rip
R3(config-router)#network 192.168.2.0
```
此时在R1应该能够ping通R3. (执行命令ping 192.168.2.2)
##### IGRP配置实验
<div align="center">
<img src="https://github.com/focus7eleven/TeamOfNetwork/blob/master/Homework_1/src/router_lll/IGRP-experiment.png?raw=true" style="max-width:700px;"/>
<p align="center">图5.3 IGRP实验</p>
</div>

端口的ip地址按如图5.3所示配置（具体命令参见5.1.1）。
```
R1:
R1(config)#router eigrp 55
R1(config-router)#network 192.168.1.0

R2:
R2(config)#router eigrp 55
R2(config-router)#network 192.168.1.0
R2(config-router)#network 192.168.2.0
R2(config-router)#exit
R2(config)#end
R3:
R3(config)#router eigrp 55
R3(config-router)#network 192.168.2.0
```
此时在R1应该能够ping通R3.  (执行命令ping 192.168.2.2)

##### OSPF配置实验  
<div align="center">
<img src="https://github.com/focus7eleven/TeamOfNetwork/blob/master/Homework_1/src/router_lll/OSPFexperiment.png?raw=true" style="max-width:700px;"/>
<p align="center">图5.4 OSPF实验</p>
</div>

各端口的ip地址配置如图5.4所示。（具体的ip地址配置命令参见5.1.1）  
如图5.4是三个路由器的简单拓扑图，我们在进行OSPF实验的时候可以把这三个路由器都放在同一个区域内，就可以做单区域的OSPF配置实验。  
```
R1：
R1#configure terminal
Enter configuration commands, one per line. End with CNTL/Z.
R1(config)#router ospf 1
R1(config-router)#network 192.168.1.0 0.0.0.255 area 0
R1(config-router)#exit
R1(config)#end
R2：
R2#configure terminal
Enter configuration commands, one per line. End with CNTL/Z.
R2(config)#router ospf 1
R2(config-router)#network 192.168.1.0 0.0.0.255 area 0
R2(config-router)#network 192.168.2.0 0.0.0.255 area 0
R2(config-router)#exit
R2(config)#end
R3：
R3#configure terminal
Enter configuration commands, one per line. End with CNTL/Z.
R3(config)#router ospf 1
R3(config-router)#network 192.168.2.0 0.0.0.255 area 0
R3(config-router)#exit
R3(config)#end
```
之后，我们再在R3上加入一条外部路由，命令如下：
```
R3#configure terminal
Enter configuration commands, one per line. End with CNTL/Z.
R3(config)#ip route 199.9.9.0 255.255.255.0 null0
R3 (config)#router ospf 1
R3 (config-router)#redistribute static
R3(config)#end
```
加入外部路由以后可以使用“show ip route”查看路由表。在OSPF实验中，我们还可以进行区域间路由归纳与外部路由归纳，命令如下：  
在R1上进行回环地址的配置，可以配置三个回环地址，再进行区域间路由归纳。
```
R1 (config)#int lo0
R1 (config-if)#ip address 172.16.1.1 255.255.255.0
R1 (config-if)#exit
R1 (config)#int lo1
R1 (config-if)#ip address 172.16.2.1 255.255.255.0
R1 (config-if)#exit
R1 (config)#exit
R1 (config)#int lo2
R1 (config-if)#ip address 172.16.3.1 255.255.255.0
R1 (config-if)#end
R1 (config)#router ospf 1
R1 (config-router)#area 1 range 172.16.0.0 255.255.252.0
R1 (config-router)#exit
```
按照上面命令所配置，R1的三个回环地址就可以归纳为172.16.0.0/24了。而对于外部路由归纳，我们可以按照下面命令配置：
```
R3 (config)#router ospf 1
R3 (config-router)#summary-address 199.9.0.0 255.255.0.0
R3 (config-router)#end
```
##### BGP实验  
<div align="center">
<img src="https://github.com/focus7eleven/TeamOfNetwork/blob/master/Homework_1/src/router_lll/BGP-experiment.png?raw=true" style="max-width:700px;"/>
<p align="center">图1.1 BGP实验</p>
</div>

如图所示RTA和RTC是连接外部Internet的ISP路由器，RTB是XYZ公司的边路网关路由器，首先为RTA和RTC这两台ISP路由器配置环回接口IP地址。这些环回接口将用于模拟通过ISP可以达到的真实网络。
```
RTA (config)#int lo0
RTA (config-if)#ip address 10.10.10.10 255.0.0.0
RTA (config-if)#exit

RTB(config)#int lo0
RTB (config-if)#ip address 172.16.1.1 255.255.0.0
RTB (config-if)#exit

配置ISP路由器：
RTA (config)#router bgp 100
RTA (config-router)#neighbor 192.168.1.6  remote-as 24
RTA (config-router)#network 10.0.0.0
RTC (config)#router bgp 24
RTC (config-router)#neighbor 172.24.1.17  remote-as 24
RTC (config-router)#network 172.16.0.0

配置路由器RTB与两家服务供应商运行BGP协议：
RTB (config)#router bgp 24
RTB (config-router)#neighbor 192.168.1.5  remote-as 100
RTB (config-router)#network 172.24.1.18   remote-as 200
RTB (config-router)#network 200.100.50.0
```
#### 广域网协议(PPP和HDLC) 配置实验
<div align="center">
<img src="https://github.com/focus7eleven/TeamOfNetwork/blob/master/Homework_1/src/router_lll/border-network-experiment.png?raw=true" style="max-width:700px;"/>
<p align="center">图5.5 广域网实验</p>
</div>

按图5.5所示配置好各端口的ip地址。（具体的ip地址配置命令参见5.1.1）  
此时思科路由器使用默认的hdlc协议，可以用show int s0/0查看。  
```
R1#show int s0/0
Serial0/0 is up, line protocol is up
  Hardware is GT96K Serial
  Internet address is 192.168.1.1/24
  MTU 1500 bytes, BW 1544 Kbit, DLY 20000 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation HDLC, loopback not set
  Keepalive set (10 sec)
  CRC checking enabled
  Last input 00:00:02, output 00:00:04, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: weighted fair
  Output queue: 0/1000/64/0 (size/max total/threshold/drops)
     Conversations  0/1/256 (active/max active/max total)
     Reserved Conversations 0/0 (allocated/max allocated)
     Available Bandwidth 1158 kilobits/sec
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     1878 packets input, 121397 bytes, 0 no buffer
     Received 901 broadcasts, 0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort
     1715 packets output, 111742 bytes, 0 underruns
     0 output errors, 0 collisions, 14 interface resets
     0 output buffer failures, 0 output buffers swapped out
     0 carrier transitions
     DCD=up  DSR=up  DTR=up  RTS=up  CTS=up
```
若要改为PPP协议，可以用encapsulation ppp指令进行修改。（注意，此处链路两边需要同步修改）
```
R1#conf ter
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#int s0/0
R1(config-if)#encapsulation ppp

R2# conf ter
Enter configuration commands, one per line.  End with CNTL/Z.
R2(config)#int s0/0
R2(config-if)#encapsulation ppp
```
此时，链路联通，且链路层协议改为PPP协议，可用show int s0/0进行查看确认。
```
R1#show int s0/0
Serial0/0 is up, line protocol is up
  Hardware is GT96K Serial
  Internet address is 192.168.1.1/24
  MTU 1500 bytes, BW 1544 Kbit, DLY 20000 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation PPP, LCP Open
  Open: IPCP, CDPCP, loopback not set
  Keepalive set (10 sec)
  CRC checking enabled
  Last input 00:00:01, output 00:00:01, output hang never
  Last clearing of "show interface" counters 00:06:40
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: weighted fair
  Output queue: 0/1000/64/0 (size/max total/threshold/drops)
     Conversations  0/2/256 (active/max active/max total)
     Reserved Conversations 0/0 (allocated/max allocated)
     Available Bandwidth 1158 kilobits/sec
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     167 packets input, 9284 bytes, 0 no buffer
     Received 0 broadcasts, 0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort
     173 packets output, 8938 bytes, 0 underruns
     0 output errors, 0 collisions, 5 interface resets
     0 output buffer failures, 0 output buffers swapped out
     0 carrier transitions
     DCD=up  DSR=up  DTR=up  RTS=up  CTS=up
```
如要改回HDLC协议，可输入类似的命令修改：
```
R1#conf ter
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#int s0/0
R1(config-if)#encapsulation hdlc

R2# conf ter
Enter configuration commands, one per line.  End with CNTL/Z.
R2(config)#int s0/0
R2(config-if)#encapsulation hdlc
```
#### NAT实验配置  
<div align="center">
<img src="https://github.com/focus7eleven/TeamOfNetwork/blob/master/Homework_1/src/router_lll/NAT-experiment.jpg?raw=true" style="max-width:700px;"/>
<p align="center">图1.1 NAT实验</p>
</div>
在Router0及Router3处配置Nat，替换192.168.1.0处的地址。各处地址配置如图。  
使用RIP动态路由，配置如下：  
```
Router0:
router rip
network 200.200.200.0
Router1:
router rip
  network 200.200.200.0
  network 200.200.201.0
Router3:
  router rip
  network 200.200.201.0
```
##### 静态NAT实验
在Router0处配置NAT  
```
Router>enable
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#ip nat inside source static 192.168.1.2 200.200.200.120
Router(config)#ip nat inside source static 192.168.1.3 200.200.200.121
Router(config)#int s0/0
Router(config-if)#ip nat outside
Router(config-if)#int f0/0
Router(config-if)#ip nat inside
Router(config-if)#end
```
输入show ip nat translations命令查看目前的NAT表
```
Router#show ip nat translations
Pro  Inside global     Inside local       Outside local      Outside global
---  200.200.200.120   192.168.1.2        ---                ---
---  200.200.200.121   192.168.1.3        ---                ---
```
上图可知，内部网络地址已经成功转为可用的外部网络地址。但是外部网络的目的地址还没有，是因为此时还没有ping任何一个外部网络，故NAT转发表还未建立。  
使用pc0  ping 路由器1(Route1)
```
Router#show ip nat translations
Pro  Inside global     Inside local       Outside local      Outside global
icmp 200.200.200.120:10192.168.1.2:10     200.200.201.100:10 200.200.201.100:10
icmp 200.200.200.120:11192.168.1.2:11     200.200.201.100:11 200.200.201.100:11
icmp 200.200.200.120:12192.168.1.2:12     200.200.201.100:12 200.200.201.100:12
icmp 200.200.200.120:9 192.168.1.2:9      200.200.201.100:9  200.200.201.100:9
---  200.200.200.120   192.168.1.2        ---                ---
---  200.200.200.121   192.168.1.3        ---                ---
```
由上图可知，在pc0上192.168.1.2已经成功建立了NAT转发表，pc1上192.168.1.3因为没有ping任何外部网络，所以它的转发表为空。  
在Router3处配置NAT  
类似于Router0的配置。  
##### 动态NAT配置
```
Router0：
Router0(config)#int f0/0
Router0(config-if)#ip address 192.168.1.1 255.255.255.0
Router0(config-if)#no shut
Router0(config-if)#ip nat inside
Router0(config-if)#int s0/0
Router0(config-if)#ip address 200.200.200.100 255.255.255.0
Router0(config-if)#clock 56000
Router0(config-if)#no shut
Router0(config-if)#ip nat outside
Router0(config-if)#exit
Router0(config)#access-list 1 permit 192.168.1.0 0.0.0.255 (取所有192.168.1.0段的值)
Router0(config)#ip nat inside source list 1 int s0/0 overload
```
此时Router0处的Nat配置已完成，从PC0(PC1)能ping通Router1和Router3。  
Router3执行类似配置：
```
Router3(config)#int f0/0
Router3(config-if)#ip address 192.168.1.5 255.255.255.0
Router3(config-if)#no shut
Router3(config-if)#ip nat inside
Router3(config-if)#int s0/0
Router3(config-if)#ip address 200.200.201.101 255.255.255.0
Router3(config-if)#clock 56000
Router3(config-if)#no shut
Router3(config-if)#ip nat outside
Router3(config-if)#exit
Router3(config)#access-list 1 permit 192.168.1.0 0.0.0.255 (取所有192.168.1.0段的值)
Router3(config)#ip nat inside source list 1 int s0/0 overload
```
此时Router3处的Nat配置已完成，从PC6(PC8)能ping通Router0和Router1。
