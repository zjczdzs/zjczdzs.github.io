# IP协议第一章笔记


<!--more-->

# 一、概述

## 1.1引言

TCP/IP起源于60年代末美国政府资助的一个分组交换网络研究项目，到90年代已发展成为计算机之间最常应用的组网形式。

------

## 1.2分层

网络协议通常分不同层次进行开发，每一层分别负责不同的通信功能。一个协议族，比如TCP/IP，是一组不同层次上的多个协议的组合。TCP/IP通常被认为是一个四层协议系统。

![](https://img-blog.csdnimg.cn/46aaff4245b94c6f8e07e85fd4fe661e.png)



1) 链路层，有时也称作数据链路层或网络接口层，通常包括操作系统中的设备驱动程序和计算机中对应的网络接口卡。它们一起处理与电缆（或其他任何传输媒介）的物理接口细节。

2) 网络层，有时也称作互联网层，处理分组在网络中的活动，例如分组的选路。在TCP/IP协议族中，网络层协议包括IP协议（网际协议），ICMP协议（Internet互联网控制报文协议），以及IGMP协议（Internet组管理协议）。
3)  运输层主要为两台主机上的应用程序提供端到端的通信。在TCP/IP协议族中，有两个互不相同的传输协议：TCP（传输控制协议）和UDP（用户数据报协议）
   - TCP为两台主机提供高可靠性的数据通信。它所做的工作包括把应用程序交给它的数据分成合适的小块交给下面的网络层，确认接收到的分组，设置发送最后确认分组的超时时钟等。
   - UDP则为应用层提供一种非常简单的服务。它只是把称作数据报的分组从一台主机发送到另一台主机，但并不保证该数据报能到达另一端。任何必需的可靠性必须由应用层来提供。
4) 应用层负责处理特定的应用程序细节。几乎各种不同的TCP/IP实现都会提供下面这些通用的应用程序：
   -  Telnet远程登录。
   -  FTP文件传输协议。
   - SMTP简单邮件传送协议。
   - SNMP简单网络管理协议。

		我们注意到应用程序通常是一个用户进程，而下三层则一般在（操作系统）内核中执行。尽管这不是必需的，但通常都是这样处理的，例如UNIX操作系统。

		顶层与下三层之间还有另一个关键的不同之处。应用层关心的是应用程序的细节，而不是数据在网络中的传输活动。下三层对应用程序一无所知，但它们要处理所有的通信细节。

		TCP/IP协议族是一组不同的协议组合在一起构成的协议族。尽管通常称该协议族为TCP/IP，但TCP和IP只是其中的两种协议而已（该协议族的另一个名字是Internet协议族(InternetProtocolSuite)）。

- **路由器**

		构造互连网最简单的方法是把两个或多个网络通过路由器进行连接。它是一种特殊的用于网络互连的硬件盒。路由器的好处是为不同类型的物理网络提供连接：以太网、令牌环网、点对点的链接和FDDI（光纤分布式数据接口）等等。
	
		也称作IP路由器（IPRouter），但我们这里使用路由器(Router)这个术语。从历史上说，这些盒子称作网关（gateway），在很多TCP/IP文献中都使用这个术语。现在*网关这个术语只用来表示应用层网关*：一个连接两种不同协议族的进程（例如，TCP/IP和IBM的SNA）
	
		任何具有多个接口的系统，英文都称作是多接口的(multihomed)。一个主机也可以有多个接口，但一般不称作路由器,除非它的功能只是单纯地把分组从一个接口传送到另一个接口。同样，路由器并不一定指那种在互联网中用来转发分组的特殊硬件盒。大多数的TCP/IP实现也允许一个多接口主机来担当路由器的功能，但是主机为此必须进行特殊的配置。在这种情况下，我们既可以称该系统为主机（当它运行某一应用程序时，如FTP或Telnet），也可以称之为路由器（当它把分组从一个网络转发到另一个网络时）。在不同的场合下使用不同的术语。
	
		在TCP/IP协议族中，网络层IP提供的是一种不可靠的服务。也就是说，它只是尽可能快地把分组从源结点送到目的结点，但是并不提供任何可靠性保证。而另一方面，TCP在不可靠的IP层上提供了一个可靠的运输层。为了提供这种可靠的服务，TCP采用了超时重传、发送和接收端到端的确认分组等机制。

- **网桥**

连接网络的另一个途径是使用网桥。网桥是在链路层上对网络进行互连，而路由器则是在网络层上对网络进行互连。网桥使得多个局域网（LAN）组合在一起，这样对上层来说就好像是一个局域网。TCP/IP倾向于使用路由器而不是网桥来连接网络，因此我们将着重介绍路由器。

------

## 1.3TCP/IP的分层

![在这里插入图片描述](https://img-blog.csdnimg.cn/2143c0f97c8640828a8c48cb3148d144.png)


- IP是网络层上的主要协议，同时被TCP和UDP使用。TCP和UDP的每组数据都通过端系统和每个中间路由器中的IP层在互联网中进行传输。在图1-4中，我们给出了一个直接访问IP的应用程序。这是很少见的，但也是可能的（一些较老的选路协议就是以这种方式来实现的。当然新的运输层协议也有可能使用这种方式）。

- ICMP是IP协议的附属协议。IP层用它来与其他主机或路由器交换错误报文和其他重要信息。尽管ICMP主要被IP使用，但应用程序也有可能访问它。将分析两个流行的诊断工具，Ping和Traceroute，它们都使用了ICMP。

- IGMP是Internet组管理协议。它用来把一个UDP数据报多播到多个主机。广播（把一个UDP数据报发送到某个指定网络上的所有主机）和多播的一般特性。

- ARP（地址解析协议）和RARP（逆地址解析协议）是某些网络接口（如以太网和令牌环网）使用的特殊协议，用来转换IP层和网络接口层使用的地址。

  ------

## 1.4互联网的地址

互联网每个接口必须有一个唯一的Internet地址（IP地址）。IP地址长32bit。IP地址具有一定的结构，五类不同的互联网地址如图所示。

![在这里插入图片描述](https://img-blog.csdnimg.cn/4cda7cb639ac4519ad60d7270f8c7127.png)


这些32位的地址通常写成4个十进制的数，其中每个整数对应一个字节。这种表示方法称作”点分十进制表示法“



> 多借口主机有多个IP地址，每个接口都对应一个IP地址。

唯一的IP地址由一个叫”互联网络信息中心“的管理机构分配。简称”InterNIC“。其只分配网络号，主机号由系统管理员负责分配。

> InterNIC由三部分组成：注册服务，目录和数据库服务，以及信息服务。
>
> 一开始只有NIC负责分配IP（nic.ddn.mil）,后来InterNIC成立。目前NIC只处理国防数据网的注册请求，其他由InterNIC负责

三类IP地址：

- 单播地址（目的端位单个主机）
- 广播地址（目的端位给定网络上的所有主机）、
- 多播地址（目的端位同一组内的所有主机）

------

## 1.5DNS域名系统

尽管通过IP地址可以识别主机上的网络接口，进而访问主机，但是人们最喜欢使用的还是主机名。

> DNS是一个分布的数据库，由它来提供IP地址和主机名之间的映射信息。

> 作用：我们用Telnet进行远程登录时，既可以指定一个主机名，也可以指定一个IP地址。

------

## 1.6封装

用TCP传送数据时，数据被送入协议栈中，然后逐个通过每一层直到被当作一串比特流送入网络。其中每一层对收到的数据都要增加一些首部信息（有时还要增加尾部信息）

> TCP传给IP的数据单元称作TCP报文段或简称为TCP段（TCPsegment）。IP传给网络接口层的数据单元称作IP数据报(IPdatagram)。通过以太网传输的比特流称作帧(Frame)。



![在这里插入图片描述](https://img-blog.csdnimg.cn/6f67b5b4d1674bf69411d196870e513e.png)


更准确地说，图1-7中IP和网络接口层之间传送的数据单元应该是分组（packet）。分组既可以是一个IP数据报，也可以是IP数据报的一个片（fragment）。

UDP数据与TCP数据基本一致。唯一的不同是UDP传给IP的信息单元称作UDP数据报（UDPdatagram），而且UDP的首部长为8字节。

> 由于TCP、UDP、ICMP和IGMP都要向IP传送数据，因此IP必须在生成的IP首部中加入某种标识，以表明数据属于哪一层。为此，IP在首部中存入一个长度为8bit的数值，称作协议域。1表示为ICMP协议，2表示为IGMP协议，6表示为TCP协议，17表示为UDP协议。
>
> 许多应用程序都可以使用TCP或UDP来传送数据。运输层协议在生成报文首部时要存入一个应用程序的标识符。TCP和UDP都用一个16bit的端口号来表示不同的应用程序。TCP和UDP把源端口号和目的端口号分别存入报文首部中。
>
> 网络接口分别要发送和接收IP、ARP和RARP数据，因此也必须在以太网的帧首部中加入应用程序某种形式的标识，以指明生成数据的网络层协议。为此，以太网的帧首部也有一个16bit的帧类型域。

------

## 1.7分用

分用是根据1.6封装规则进行解析，协议确实是通过目的端口号、源IP地址和源端口号进行解包的。

当目的主机收到一个以太网数据帧时，数据就开始从协议栈中由底向上升，同时去掉各层协议加上的报文首部。每层协议盒都要去检查报文首部中的协议标识，以确定接收数据的上层协议。这个过程称作分用。

![在这里插入图片描述](https://img-blog.csdnimg.cn/21767d06b4374ac99a813b939eff66ca.png)


协议ICMP和IGMP定位一直是一件很棘手的事情。在图1-4中，把它们与IP放在同一层上，那是因为事实上它们是IP的附属协议。但是在这里，我们又把它们放在IP层的上面，这是因为ICMP和IGMP报文都被封装在IP数据报中。对于ARP和RARP，我们也遇到类似的难题。在这里把它们放在以太网设备驱动程序的上方，这是因为它们和IP数据报一样，都有各自的以太网数据帧类型。但在图2-4中，我们又把ARP作为以太网设备驱动程序的一部分，放在IP层的下面。

------

## 1.8C/S模型

大部分网络应用程序使用C/S架构，C/S分为两种类型。

- 重复型

  - 1.等待一个客户请求的到来。

  - 2.处理客户请求。

  - 3.发送响应给发送请求的客户

  - 4.返回第一步

    > 重复型服务器**主要问题**在第二步，这时不能位其他客户机提供服务。

- 并发型

  - 1.等待一个客户请求的到来。

  - 2.启动一个新的服务器来处理这个客户的请求。在这期间可能生成一个新的进程、任务或线程、协程，并依赖底层操作系统的支持。这个步骤如何进行取决于操作系统。生成的新服务器对客户的全部请求进行处理。处理结束后，终止这个新服务器。

  - 3.返回第一步

    > 并发服务器**优点**是对每个用户请求单独生成一个服务器处理。如果操作系统允许多任务（基本都允许），那么久可以同时位多个客户服务。

一般来说TCP服务是并发的，UDP服务是重复的。（有例外）

- 为什么只对服务器进行分类而不是对用户分类？

  > 因为对于一个用户来说，它通常并不能够辨别自己是与一个重复型还是并发型服务器对话。

------

## 1.9端口号

服务器一般都是通过知名端口号来识别的。例如，对于每个TCP/IP实现来说，FTP服务器的TCP端口号都是21，每个Telnet服务器的TCP端口号都是23，每个TFTP(简单文件传送协议)服务器的UDP端口号都是69。任何TCP/IP实现所提供的服务都用知名的1～1023之间的端口号。这些知名端口号由Internet号分配机构（InternetAssignedNumbersAuthority,IANA）来管理。

客户端通常对它所使用的端口号并不关心，只需保证该端口号在本机上是唯一的就可以了。客户端口号又称作**临时端口号**（即存在时间很短暂）。这是因为它通常只是在用户运行该客户程序时才存在，而服务器则只要主机开着的，其服务就运行。

大多数TCP/IP实现给临时端口分配1024～5000之间的端口号。大于5000的端口号是为其下载他服务器预留的（Internet上并不常用的服务)。我们可以在后面看见许多这样的给临时端口分配端口号的例子。

端口号可以分为三个范围：“已知端口”、“注册端口”以及“动态和/或专用端口”。

- “已知端口”是从0到1023的端口。
- “注册端口”是从1024到49151的端口。
- “动态和/或专用端口”是从49152到65535的端口。理论上，不应为服务分配这些端口。

> 保留端口号
>
> Unix系统有保留端口号的概念。只有具有超级用户特权的进程才允许给它自己分配一个保留端口号。这些端口号介于1～1023间，一些应用程序（如有名的Rlogin，26.2节）将它作为客户与服务器之间身份认证的一部分。

------

## 1.10标准化过程

有四个小组在负责Internet技术的规范与研究

1. Internet协会（ISOC）：是一个推动、支持和促进Internet不断增长和发展的专业组织，它把Internet作为全球研究通信的基础设施。
2. Internet体系结构委员会（IAB）：是一个技术监督和协调的机构。它由国际上来自不同专业的15个志愿者组成，其职能是负责Internet标准的最后编辑和技术审核。IAB隶属于ISOC。
3. Internet工程专门小组(IETF)是一个面向近期标准的组织，分为9个领域（应用、寻径和寻址、安全等）。IETF开发成为Internet标准的规范。
4. Internet研究专门小组（IRIF）：主要对长远的项目进行研究。是为了帮助IETF主席成立的。

IRIF和IETF都属于IAB。

------

## 1.11RFC

所有关于Internet的正式标准都以RFC（RequestforComment）文档出版。另外，大量的RFC并不是正式的标准，出版的目的只是为了提供信息。RFC的篇幅从1页到200页不等。每一项都用一个数字来标识，如RFC1122，数字越大说明RFC的内容越新。

有四种重要的RFC文档：

1. 复制RFC：列出了所有Internet协议里使用到的数字和常数。所有端口号都有。
2. Internet正式协议标准：描述了各种Internet协议的标准化现状。每种协议都处于下面几种标准化状态之一：标准、草案标准、提议标准、实验标准、信息标准和历史标准。
3. 主机需求RFC：1122针对链路层、网络层、运输层。1123针对应用层。它们列出了协议中关于“必须”、“应该”、“可以”、“不应该”或者“不能”等特性及其实现细节。
4. 路由器需求RFC：与主机RFC需求类似，只单独描述了路由器的需求。

## 1.12标准的简单服务

有一些标准的简单服务几乎每种实现都要提供。

![在这里插入图片描述](https://img-blog.csdnimg.cn/a260fafb217b498c8b6404985be4b8bd.png)


> 标准的简单服务以及其他标准的TCP/IP服务（如Telnet、FTP、SMTP等）的端口号时，我们发现它们都是奇数。这是有历史原因的，因为这些端口号都是从NCP端口号派生出来的（NCP，即网络控制协议，是ARPANET的运输层协议，是TCP的前身）。NCP是单工的，不是全双工的，因此每个应用程序需要两个连接，需预留一对奇数和偶数端口号。当TCP和UDP成为标准的运输层协议时，每个应用程序只需要一个端口号，因此就使用了NCP中的奇数。

## 1.13互联网

**世界范围内的互联网—Internet，internet这个词第一个字母是否大写决定了它具有不同的含义。**

internet意思是用一个共同的协议族把多个网络连接在一起。而Internet指的是世界范围内通过TCP/IP互相通信的所有主机集合（超过100万台）。Internet是一个internet，但internet不等于Internet。

## 1.14实现

既成事实标准的TCP/IP软件实现来自于位于伯克利的加利福尼亚大学的计算机系统研究小组。从历史上看，软件是随同4.xBSD系统（BerkeleySoftwareDistribution）的网络版一起发布的。它的源代码是许多其他实现的基础。图1-10列举了各种BSD版本发布的时间，并标注了重要的TCP/IP特性。列在左边的BSD网络版，其所有的网络源代码可以公开得到：包括协议本身以及许多应用程序和工具（如Telnet和FTP）。

![在这里插入图片描述](https://img-blog.csdnimg.cn/c80b059d9f054de5a09bc3f0ffc79a35.png)


## 1.15 应用编程接口

使用TCP/IP协议的应用程序通常采用两种应用编程：socket和TLI。前者有时称作“Berkeley socket”，表明它是从伯克利版发展而来的。后者起初是由 AT & T开发的，有时称作 XTI（X/Open运输层接口），以承认X/Open这个自己定义标准的国际计算机生产商所做的工作。 XTI实际上是TLI的一个超集。

## 1.16 测试网络

图1 - 11是本书中所有的例子运行的测试网络。

![在这里插入图片描述](https://img-blog.csdnimg.cn/5eac5b785e0c47b19202d03fa9fc6763.png)


## 1.17 小结

TCP / IP协议族分为四层：**链路层**、**网络层**、**运输层**和应用层，每一层各有不同的责任。

在TCP / IP中，网络层和运输层之间的区别是最为关键的：**网络层（ IP）提供点到点的服务**，而**运输层（TCP和UDP）提供端到端的服务**。一个互联网是网络的网络。**构造互联网的共同基石是路由器**，它们在 IP层把网络连在一起。第一个字母大写的Internet是指分布在世界各地的大型互联网，其中包括 1万多个网络和超过100万台主机。

在一个互联网上，每个接口都用 IP地址来标识，尽管用户习惯使用主机名而不是 IP地址。域名系统为主机名和 IP地址之间提供动态的映射。端口号用来标识互相通信的应用程序。服务器使用知名端口号，而客户使用临时设定的端口号。

