### 4. 交换机的具体实践(交换机配置)
#### 概要
交换机是一种用于电信号转发的网络设备。它可以为接入交换机的任意两个网络节点提供独享的电信号通路。交换机的主要功能包括物理编址、网络拓扑结构、错误校验、帧序列以及流控。交换技术的发展，也加快了新的交换技术（VLAN）的应用速度。通过将企业网络划分为虚拟网络VLAN网段，可以强化网络管理和网络安全，控制不必要的数据广播。
本文分为三个部分，主要介绍了交换机的工作原理、交换机使用的生成树协议以及VLAN的相关知识，提供了配置交换机的整体概念。
#### 4.1 交换机的工作机制
##### 4.1.1 交换机的工作原理
###### 4.1.1.1 构造及主要功能
交换机是一种基于MAC地址识别，能完成封装转发数据包功能的网络设备。交换机可以“学习”MAC地址，并把其存放在内部地址表中，通过在数据帧的始发者和目标接收者之间建立临时的交换路径，使数据帧直接由源地址到达目的地址。
传统的交换机工作在OSI 模型中的第二层，类似于一台专用的特殊计算机，主要包括中央处理器(CPU)、随机存储器(RAM) 和操作系统。它利用专门设计的芯片ASIC(Application SpecificIntegrated Circuits)使交换机以线路速率在所有的端口并行进行转发，因此，它比同在二层利用软件进行转发的网桥速度快的多。
交换机拥有一条很高带宽的背部总线和内部交换矩阵。交换机的所有的端口都挂接在这条背部总线上，控制电路收到数据包以后，处理端口会查找内存中的地址对照表以确定目的MAC（网卡的硬件地址）的NIC（网卡）挂接在哪个端口上，通过内部交换矩阵迅速将数据包传送到目的端口，目的MAC若不存在才广播到所有的端口，接收端口回应后交换机会“学习”新的地址，并把它添加入内部MAC地址表中。
交换机使用一种虚拟连接技术来连接通信的双方。所谓虚拟连接，就是指通信时通信双方建立一个逻辑上的专用连接，这个连接直到数据传送至目的节点后结束。虚拟连接是通过交换机的端口-地址表来实现的：交换机在工作过程中不断地建立和维护它本身的一个地址表，这个地址表标明了节点的MAC 地址和交换机端口的对应关系。当交换机收到一个数据包，它便会去查看自身的地址表以验明数据包中的目的MAC 地址究竟对应于哪个端口。一旦验证完毕，就将发送节点与该端口建立一个专用连接，发送方的数据仅发送到目的MAC 地址所对应的交换机端口。
交换机在同一时刻可进行多个端口对之间的数据传输。每一端口都可视为独立的网段，连接在其上的网络设备独自享有全部的带宽，无须同其他设备竞争使用。
###### 4.1.1.2 二层交换机的工作原理
交换机（二层交换）的工作原理交换机和网桥一样，是工作在链路层的联网设备，它的各个端口都具有桥接功能，每个端口可以连接一个LAN或一台高性能网站或服务器，能够通过自学习来了解每个端口的设备连接情况。所有端口由专用处理器进行控制，并经过控制管理总线转发信息。
二层交换机的原理很简单，它检测从以太端口来的数据包的源和目的地的MAC（介质访问层）地址，然后与系统内部的动态查找表进行比较，若数据包的MAC 层地址不在查找表中，则将该地址加入查找表中，并将数据包发送给相应的目的端口。其工作流程为：
1. 当交换机从某个端口收到一个数据包，它先读取包头中的源MAC 地址，这样它就知道源MAC 地址的机器是连在哪个端口上的；
2. 再去读取包头中的目的MAC 地址，并在地址表中查找相应的端口；
3. 如表中有与这目的MAC 地址对应的端口，把数据包直接复制到这端口上；
4. 如表中找不到相应的端口则把数据包广播到所有端口上，当目的机器对源机器回应时，交换机又可以学习目的MAC 地址与哪个端口对应，在下次传送数据时就不再需要对所有端口进行广播了。

不断地循环这个过程，学习全网的MAC 地址信息，二层交换机就是这样建立和维护它自己的地址表。
交换机使用两种模式进行帧的转发：直通交换和存储转发。
直通交换：交换机在执行直通交换时，当它接收到帧时，只读取目的地址，然后，在整个帧到达之前，帧就被转发了出去。这种模式减少了传输延时，，但也减弱了错误检测。
存储转发：当交换机执行存储转发交换时，在转发之前必须接收到整个帧。然后，交换机读取目的或源地址，并且在帧发送之前进行过滤。在交换机接收帧的过程中，会发生延迟。帧越大，延迟越长，因为需要更长的时间来读出整个帧。采用这种方式时，错误可以检测出来，因为交换机在等待整个帧接收完成的过程中，它有时间来检查。
###### 4.1.1.3 三层交换机的工作原理
传统的交换机本质上是具有流量控制能力的多端口网桥，即传统的（二层）交换机。把路由技术引入交换机，可以完成网络层路由选择，故称为三层交换，这是交换机的新进展。
三层交换是相对于传统交换概念而提出的。众所周知，传统的交换技术是在OSI网络标准模型中的第二层——数据链路层进行操作的，而三层交换技术是在网络模型中的第三层实现了数据包的高速转发。简单地说，三层交换技术就是：二层交换技术+三层转发技术。
第三层交换机工作在OSI七层网络模型中的第三层即网络层，它不仅使用第二层MAC地址信息来做出转发决策，而且还可以使用IP地址信息。它是利用第三层协议中的IP包的报头信息来对后续数据业务流进行标记，具有同一标记的业务流的后续报文被交换到第二层数据链路层，从而打通源IP地址和目的IP地址之间的一条通路。这条通路经过第二层链路层。有了这条通路，三层交换机就没有必要每次将接收到的数据包进行拆包来判断路由，而是直接将数据包进行转发，将数据流进行交换。
其原理是：假设两个使用IP协议的站点A、B通过第三层交换机进行通信，发送站点A在开始发送时，把自己的IP地址与B站的IP地址比较，判断B站是否与自己在同一子网内。若目的站B与发送站A在同一子网内，则进行二层的转发。若两个站点不在同一子网内，如发送站A要与目的站B通信，发送站A要向“缺省网关”发出ARP(地址解析)封包，而“缺省网关”的IP地址其实是三层交换机的三层交换模块。当发送站A对“缺省网关”的IP地址广播出一个ARP请求时，如果三层交换模块在以前的通信过程中已经知道B站的MAC地址，则向发送站A回复B的MAC地址。否则三层交换模块根据路由信息向B站广播一个ARP请求，B站得到此ARP请求后向三层交换模块回复其MAC地址，三层交换模块保存此地址并回复给发送站A，同时将B站的MAC地址发送到二层交换引擎的MAC地址表中。从这以后，当A向B发送的数据包便全部交给二层交换处理，信息得以高速交换。由于仅仅在路由过程中才需要三层处理，绝大部分数据都通过二层交换转发，因此三层交换机的速度很快，接近二层交换机的速度，同时比相同路由器的价格低很多。
##### 4.1.2 交换机的常规配置
以Cisco交换机为例，说明交换机的一些常规配置。
###### 4.1.2.1 切换命令行界面模式
作为一项安全功能，Cisco IOS软件将EXEC（执行）会话分成以下两种访问级别。
用户执行：只允许用户访问有限量的基本监视命令。用户执行模式是在从CLI登录到Cisco交换机后所进入的默认模式。用户执行模式由">"提示符标识。
特权执行：允许用户访问所有设备命令，如用于配置和管理的命令，特权执行模式可采用口令加以保护，使得只有获得授权的用户才能访问设备。特权执行模式由#提示符标识。
要从用户执行模式切换到特权执行模式，输入enable命令。要从特权执行模式切换到用户执行模式，输入disable命令。在实际网络中，交换机将提示输入口令。默认情况下未配置口令。表1-1中显示了用于在用户执行模式和特权执行模式之间来回切换的Cisco IOS命令。
<div align="center">
<p>表4-1 执行模式切换</p>
<table align="center">
<tr>
	<td>说    明</td>
	<td>CLI</td>
</tr>
<tr>
	<td>从用户执行模式切换到特权执行模式</td>
	<td>Switch>enable</td>
</tr>
<tr>
	<td>如果已为特权执行模式设置口令，则系统将提示您现在输入口令</td>
	<td>Password:password</td>
</tr>
<tr>
	<td>#提示符表示已处于特权执行模式</td>
	<td>Switch#</td>
</tr>
<tr>
	<td>从特权执行模式切换到用户执行模式</td>
	<td>Switch#disable</td>
</tr>
<tr>
	<td>>提示符表示已处于用户执行模式</td>
	<td>Switch></td>
</tr>
</table>
</div>
在Cisco交换机上进入特权执行模式之后，就可以访问其他配置模式。Cisco IOS软件的命令模式结构采用分层的命令结构。

###### 4.1.2.2 基本交换机配置
局域网中的第2层交换机的一些关键配置序列通常是在实施过程中进行的。这些配置序列包括配置交换机管理界面，默认网关，全双工和活动接口上的网速设置，对HTTP访问的支持和对MAC地址表的管理。

- 1.管理接口  
接入层交换机需要配置IP地址、子网掩码和默认网关。要使用TCP/IP来远程管理交换机，就需要为交换机分配IP地址。此IP地址将分配给称为虚拟LAN（VLAN）的虚拟接口，然后必须确保VLAN分配到计算机上的一个或多个特定端口。  
要在交换机的管理VLAN上配置IP地址和子网掩码，必须处在VLAN接口配置模式下。先使用命令interface vlan 99，再输入ip address配置命令。必须使用no shutdown接口配置命令来使此第3层接口正常工作。当看到"interface VLAN x"时，这是指与VLAN x 关联的第3层接口。只有管理VLAN才有与之关联的"interface VLAN"。表1-2列出了Catalyst 2960交换机上的管理接口配置。
<div align="center">
<p>表4-2 管理接口配置</p>
<table align="center">
<tr>
	<td>说    明</td>
	<td>命    令</td>
</tr>
<tr>
	<td>进入全局配置模式</td>
	<td>S1#configure terminal</td>
</tr>
<tr>
	<td>进入VLAN99接口的接口配置模式</td>
	<td>S1(config)#interface vlan 99</td>
</tr>
<tr>
	<td>配置接口IP地址</td>
	<td>S1(config-if)#ip address 172.17.99.11 255.255.255.0</td>
</tr>
<tr>
	<td>启动接口</td>
	<td>S1(config-if)#no shutdown</td>
</tr>
<tr>
	<td>返回特权执行模式</td>
	<td>S1(config-if)#end</td>
</tr>
<tr>
	<td>进入全局配置模式</td>
	<td>S1#configure terminal</td>
</tr>
<tr>
	<td>输入要分配VLAN的接口</td>
	<td>S1(config)#interface fastethernet 0/18</td>
</tr>
<tr>
	<td>定义端口的VLAN成员模式</td>
	<td>S1(config-if)#switchport mode access</td>
</tr>
<tr>
	<td>将端口分配给VLAN</td>
	<td>S1(config-if)#switchport access vlan 99</td>
</tr>
<tr>
	<td>返回特权执行模式</td>
	<td>S1(config-if)#end</td>
</tr>
<tr>
	<td>将运行配置保存为交换机启动配置</td>
	<td>S1#copy running-config startup-config</td>
</tr>
</table>
</div>

- 2.默认网关  
默认网关是用于将IP数据包转发到远程网络的机制。交换机将目的IP地址位于本地网络之外的IP数据包转发到默认网关。  
使用ip default-gateway命令，为交换机配置默认网关。输入与需要配置默认网关的交换机直接相连的下一跳路由器接口的IP地址。
- 3.双工和速度  
<div align="center">
<img src="https://github.com/focus7eleven/TeamOfNetwork/blob/master/Homework_1/src/switch_gbc/duplex_and_speed.jpg?raw=true" style="max-width:700px;"/>
<p align="center">图1-1 双工和速度</p>
</div>
可以使用duplex接口配置命令指定交换机端口的双工操作模式。可以手动设置交换机端口的双工模式和速度，以避免厂商间的自动协商问题。在将交换机端口双工设置配置为auto时可能出现问题，在图1-1中，S1和S2交换机有着相同的双工设置和速度，这是例1-1中的配置产生的。  
在交换机S1上配置端口F0/1的具体步骤描述如例1-1所示。

例1-1 duplex和speed命令:
```
S1# configure terminal
S1(config)# interface fastethernet 0/1
S1(config-if)# duplex auto
S1(config-if)# speed auto
S1(config-if)# end
```
- 4.HTTP访问  
如例1-2是启用HTTP访问的基本配置，ip http authentication enable 是全局配置命令模式。  
例1-2 启用HTTP访问的基本配置:
```
S1# configure terminal
S1(config)# ip http authentication enable
S1(config)# ip http server
```
- 5.管理MAC地址表  
交换机使用MAC地址表来确定如何在端口间转发流量。这些MAC表包含动态地址和静态地址。用show mac-address-table命令显示MAC地址表，其输出包含静态和动态MAC地址。  
使用mac-address-table static MAC-address vlan vlan-id interface interface-id命令可在MAC地址表中创建静态映射。使用no mac-address-table static MAC-address vlan vlan-id interface interface-id命令可移除MAC地址表中的静态映射。
###### 4.1.2.3 验证交换机配置
执行初始交换机配置之后，可使用不同的show命令来验证交换机是否已正确配置。
show命令从特权执行模式下执行。表1-3列出了show命令的一些关键选项，它们可用于验证几乎所有可配置的交换机功能。
<div align="center">
<p>表4-3 show命令</p>
<table align="center">
<tr>
	<td>说    明</td>
	<td>命    令</td>
</tr>
<tr>
	<td>显示交换机上单个或全部可用接口的接口状态和配置</td>
	<td>show interface {interface-id  ¦cr}</td>
</tr>
<tr>
	<td>显示启动配置的内容</td>
	<td>show startup-config</td>
</tr>
<tr>
	<td>显示当前运行配置</td>
	<td>show running-config</td>
</tr>
<tr>
	<td>显示关于flash：文件系统的信息</td>
	<td>show flash:</td>
</tr>
<tr>
	<td>显示系统硬件和软件状态</td>
	<td>show version</td>
</tr>
<tr>
	<td>显示会话命令历史记录</td>
	<td>show history</td>
</tr>
<tr>
	<td>显示IP信息<br>Interface选项显示IP接口状态和配置<br>http选项显示有关正在交换机上运行的Device<br>Manager的HTTP信息<br>arp选项显示IP ARP表
</td>
	<td>show ip {interface | http |arp }</td>
</tr>
<tr>
	<td>显示MAC转发表</td>
	<td>show mac-address-table</td>
</tr>
</table>
</div>

###### 4.1.2.4 基本交换机管理
交换机启动并运行之后，网络技术人员必须对交换机进行维护，这就包括备份和恢复交换机的配置文件，清除配置信息和删除配置文件。
- 1.备份和恢复交换机配置文件  
使用copy running-config startup-config特权执行命令备份了目前创建的配置。如果想在设备上保留多个不同的startup-config文件，则可以使用copy startup-config flash:filename命令将配置复制到不同文件名的多个文件中。存储多个startup-config版本可用于在配置出现问题时回滚到某个时间点。  
恢复配置是一个简单的过程。只需用已存配置覆盖当前配置即可。例如，如果有名为config.bak1的已存配置，则输入Cisco IOS命令copy flash:config.bak1 startup-config即可覆盖现有start-config并恢复config.bak1的配置。当配置恢复到startup-config中后，可在特权执行模式下使用reload命令重新启动交换机，如表1-4所示，使计算机重新加载新的启动配置。reload命令将使系统停止。应在配置信息已输入到文件并保存到启动配置之后再使用reload命令。
<div align="center">
<p>表4-4恢复备份文件</p>
<table align="center">
<tr>
	<td>说    明</td>
	<td>CLI</td>
</tr>
<tr>
	<td>将存储在闪存中的config.bak1文件复制到存储在闪存中的启动配置中。按Enter键接受，使用Ctrl+C组合键取消</td>
	<td>S1#copy flash:config.bak1 startup-config
Destination filename [startup-config]?
</td>
</tr>
<tr>
	<td>使Cisco IOS执行重新启动交换机。如果修改了运行配置文件，系统将询问是否保存。请按“y”或“n”确认。要确认重新装入，请按Enter键接受，使用Ctrl+C组合键取消</td>
	<td>S1#reload
System configuration has been modified.
Save?[yes/no]:n
Proceed with reload?[confirm]
</td>
</tr>
</table>
</div>

- 2.清除配置信息  
交换机配置的清除使用erase nvram:或erase startup-config特权执行命令实现。当网络技术人员可能执行了一项很复杂的配置任务，并在闪存中存储了文件的很多备份副本，此时要从闪存中删除文件，要使用delete flash:filename特权执行命令。根据file prompt全局配置命令的设置，系统可能在技术人员删除文件之前提示确认。默认情况下，在删除文件时，交换机都会提示确认。抹除或删除配置之后，即可重新加载交换机以启动交换机的新配置。