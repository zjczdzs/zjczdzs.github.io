# Hdfs高可用与高拓展机制分析


<!--more-->

# 一、元数据服务高可用

## 1.1高可用的需求

故障类型：

- 软件故障
- 硬件故障
- 人为故障

灾难：数据中心级别不可用

故障不可避免，灾难有时发生

如果HDFS不可用，业务停止的损失极大，所以高可用就至关重要

## 1.2高可用形式

服务高可用有

- 热备份：有另一个备份节点，发生故障时可直接切换
- 冷备份：将关键性文件切换到另外位置，发生故障时通过备份数据进行恢复。

故障恢复操作：

- 人工切换
- 自动切换

人工的反应、决策时间都更长，高可用需要让系统自动决策。
HDFS的设计中，采用了中心化的元数据管理节点NameNode。NameNode容易成为故障中的单点(single point of failure)。

## 1.3HDFS NameNode高可用架构
![在这里插入图片描述](https://img-blog.csdnimg.cn/6836bf880fa14908a744f6e62a762377.png)


组件介绍：

- ActiveNamenode:主节点，提供服务，生产日志
- StandbyNamenode:备节点，消费日志
- ZooKeeper:为自动选主提供统一协调服务
- BookKeeper:提供日志存储服务
- ZKFC: NameNode探活、触发主备切换
- HA Client:提供了自动切换的客户端
- edit log:操作的日志

## 1.4理论基础：状态机复制和日志

状态机复制是实现容错的常用方法

组件：状态机以及其副本、变更日志、共识协议

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/4717341c42ff499ca748532d7a509d20.png)


## 1.5NameNode状态持久化 
![在这里插入图片描述](https://img-blog.csdnimg.cn/db71bcc0f8794751a33fe7afd2467eb9.png)

FSImage：是保存文件目录树的日志

EditLog：是保存对文件操作的日志

checkpoint机制会合并两者生成一个新的目录树日志 
![在这里插入图片描述](https://img-blog.csdnimg.cn/0658ba6c17a544859785ceccc15a1924.png)


## 1.6NameNode操作日志的生产消费

Active生产，Standby (可能有多个)消费

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/f16ab94fd9c24007a115c3cf18d1d4c4.png)


## 1.7NameNode块维护

区别

-  Active即接收，也发起变更·
- Standby只接收，不发起变更

![在这里插入图片描述](https://img-blog.csdnimg.cn/d06bdb4f59694dc1a658c9502328d9bd.png)


## 1.8自动主备切换--server

ZKFailoverController：作为外部组件，驱动HDFS NameNode的主备切换

HA核心机制:Watch

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/612299feb8f2419db791491c1ae0d461.png)


## 1.9自动主备切换--client

核心机制-standbyexception

client自动处理

## 1.10BookKeeper架构

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/57ad57189ed24a1bbbf0f068e656ceec.png)


## 1.11Quorum机制：多副本一致性读写

场景:多副本对象存储，用版本号标识数据新旧

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/4b824c0b2bd344bc9c94a37925a2142b.png)


## 1.12BookKeeper Quorum

日志场景:顺序追加、只写

Write Quorum:写入副本数

Ack Quorum:响应副本数

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/c6d3aa616b844c6f8897c0f98bfe264b.png)


## 1.13BookKeeper Ensemble

 

均衡加载数据

Round-Robin Load Balancer· 

- 第一轮: 1,2,3
- 第二轮: 2,3,4

- 第三轮: 3,4,1
- 第四轮: 4,1,2

# 二、数据存储高可用

## 2.1单机存储--RAID

Redundant Array of Independent Disks
图:提供RAID功能的NAS设备

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/7353f43a9478492691cee97dcbd6b4ad.png)


## 2.2RAID方案讲解

RAID 0：条带化-把数据按顺序依次分配在每个储存空间里

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/437e78597cbd456e9da5a14a4f72713c.png)


RAID 1: 冗余-把一份数据复制两份存储在每个储存空间里

![ ](https://img-blog.csdnimg.cn/286fc559c53b46cabe3a0a7a59f4c3be.png)


RAID 3:容错校验

把数据分成多个“块”，按照一定的容错算法，存放在N+1个硬盘上，实际数据占用的有效空间为N个硬盘的空间总和，而第N+1个硬盘上存储的数据是校验容错信息，当这N+1个硬盘中的其中一个硬盘出现故障时，从其它N个硬盘中的数据也可以恢复原始数据，这样，仅使用这N个硬盘也可以带伤继续工作（如采集和回放素材），当更换一个新硬盘后，系统可以重新恢复完整的校验容错信息。

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/64ee0c924d1947b18cebef5307de88cd.png)

## 2.3HDFS多副本

HDFS版本的RAID1多副本
![在这里插入图片描述](https://img-blog.csdnimg.cn/36aa4ef6a4d34a97845c5a8e698b608b.png)

 


优点
陵与路径简单
副本修复简单高可用

### Erasure Coding原理

HDFS版本的RAID2/3，常用Reed Solomon算法

算法原理

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/b49ef609275c44809323ba12cdf6b44e.png)


### HDFS Erasure Coding

HDFS版本的RAID2	

图:直接保存的EC和Stripe(条带化)后保存的EC

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/96acf710671d410a8c514b1313161274.png)


## 2.4网络架构

机架(Rack):放服务器的架子。
TOR(Top of Rack):机架顶部的交换机。
数据中心(Data Center):集中部署服务器的场所

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/0f216b7dc9c74ca6b107de624decfd8f.png)


2.5副本放置策略-机架感知

 

一个TOR故障导致整个机架不可用vs
降低跨rack流量

trade-off:一个本地、一个远端

# 三、元数据可拓展性

HDFS NameNode是个集中式服务，部署在单个机器上，内存和磁盘的容量、CPU的计算力都不能无限扩展。

拓展方法：

- 扩容单个服务器的能力

- 部署多个服务器来扩容

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/186c540f530345b984d601df2ae8cbce.png)




挑战：

- 名字空间分裂
- DataNode分裂
- 目录树结构复杂

## 3.1常见的Scale Out方案

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/90a6b000bdde4f19a0c79e1d1a369b4f.png)

图:三种数据路由方式

- 服务端侧
- 路由层
- 客户端侧

## 3.2社区解决方案  BlockPool

解决datenode同时服务多组NameNode的问题

文件服务分层

- Namespace
- Block Storage

用BlockPool来区分datenode的服务

- 数据块存储
- 心跳和块上报

## 3.3社区解决方案  viewsf

Federation架构:将多个不同集群组合起来，对外表现像一个集群一样。
图: viewfs通过在client-side的配置，指定不同的目录访问不同的NameNode。


![在这里插入图片描述](https://img-blog.csdnimg.cn/9ea108ebb055482da8801239fe5481ea.png)

 

## 3.4字节跳动的NNProxy

NNProxy是ByteDance自研的HDFS代理层，提供了路由服务。

NNProxy主要实现了路由管理和RPC转发·以及鉴权、限流、查询缓存等额外能力
图:NNProxy所在系统上下游

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/56bda54be6fd467b91bc4dd491e28867.png)


**NNProxy的路由规则保存**

 	![在这里插入图片描述](https://img-blog.csdnimg.cn/34fc748acbe14e4ea8d50377226dc2f9.png)


**NNProxy路由转发实现**

目录树视图

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/34d391b5ba1a432ca4627e987fccfc20.png)

# 四、存储数据高拓展性

## 4.1延迟的分布与长尾延迟

延迟的分布:用百分数来表示访问的延迟的统计特征例如p95延迟为1ms，代表95%的请求延迟要低于1ms,但后5%的请求延迟会大于1ms

长尾延迟:尾部(p99/p999/p999)的延迟，衡量系统最差的请求的情况。会显著的要差于平均值

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/45f6055d2a954a8e84e5027647177172.png)


 ![在这里插入图片描述](https://img-blog.csdnimg.cn/64c934c6731140f299674358432efedf.png)


木桶原理使尾部延迟放大:访问的服务变多，尾部的请求就会越发的慢。

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/12f6f56a7b8b45349fb1f48128e00cda.png)


尾部延迟放大，整个服务被Backend 6拖累

## 4.2长尾问题的表现-慢节点

慢节点:读取速度过慢，导致客户端阻塞。

慢节点的发生难以避免和预测

- 共享资源、后台维护活动、请求多级排队、功率限制。
- 固定的损耗:机器损坏率
- 混沌现象

离线任务也会遇到长尾问题

-  全部任务完成时间取决于最慢的任务什么时候完成。
- 集群规模变大，任务的数据量变大。
- 只要任何数据块的读取受到长尾影响，整个任务就会因此停滞.

集群扩大10倍，问题扩大N(>10)倍

## 4.3超大集群下的数据可靠性

条件一:超大集群下，有一部分机器是损坏来不及修理的。

条件二:副本放置策略完全随机。

条件三:DN的容量足够大
推论:必然有部分数据全部副本在损坏的机器上，发生数据丢失。

解决方法：Copyset

将DataNode分为若干个Copyset选块在copyset内部选择
原理:减少了副本放置的组合数，从而降低副本丢失的概率。

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/db330ebfdfc54ff8ac01249f2f86bcb9.png)


## 4.4超大集群的负载均衡和数据迁移

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/7039807e03f84e63872daf62f1ada03c.png)


新加入的DataNode会成为一个数据写入的热点，而老的datenode数据写入量就少了

数据的不均匀

- 节点容量不均匀
- 数据新日不均匀
- 访问类型不均匀

资源负载不均匀

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/e9fff67d1da748508aae04dd0f58270d.png)


典型场景

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/01738634e98b4234b8779bd6d13c39c9.png)


## 4.5数据迁移工具-跨NN迁移

**DistCopy**

- 基于MapReduce，通过一个个任务，将数据从一个NameNode拷
- 贝到另一个 NameNode。
- 需要拷贝数据，流量较大，速度较慢。

**FastCopy**

- 开源社区的无需拷贝数据的快速元数据迁移方案√前提条件:新旧集群的DN列表吻合

- 对于元数据，直接复制目录树的结构和块信息。

- 对于数据块，直接要求DataNode从源 BlockPool hardlink 到目标

  BlookPool，没有数据拷贝。

- hardlink:直接让两个路径指向同一块数据。

**balancer**

工具向DataNode 发起迁移命令，平衡各个DataNode的容量。

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/1abf01f6cc9d4519aff1516a3a26f551.png)


场景

- 单机房使用、多机房使用限流措施

评价标准

- 稳定性成本
- 可运维性
eNode。
- 需要拷贝数据，流量较大，速度较慢。

**FastCopy**

- 开源社区的无需拷贝数据的快速元数据迁移方案√前提条件:新旧集群的DN列表吻合

- 对于元数据，直接复制目录树的结构和块信息。

- 对于数据块，直接要求DataNode从源 BlockPool hardlink 到目标

  BlookPool，没有数据拷贝。

- hardlink:直接让两个路径指向同一块数据。

**balancer**

工具向DataNode 发起迁移命令，平衡各个DataNode的容量。

[外链图片转存中...(img-UrAfd3Lb-1660221746580)]

场景

- 单机房使用、多机房使用限流措施

评价标准

- 稳定性成本
- 可运维性
- 执行效率


要实现，一个分布式文件系统要考虑的太多了，毕竟hdfs是个代码量十多万的项目。但是要是想搭建基本框架的人可以参考[这个项目](http://itm-vm.shidler.hawaii.edu/HDFS/)
还有一个用go语言写的[简易gfs](https://github.com/last-saiyan/distributed-fs)

