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
![静态路由配置实验]("")  
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
![RIP实验]("")  
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
![IGRP实验]("")  
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
![OSPF实验]("")  
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
![BGP实验]("")
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
![广域网实验]("")  
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
![NAT实验]("")  
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
