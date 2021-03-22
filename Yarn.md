## Yarn

* [1.简单介绍 yarn？](#1简单介绍-yarn)
* [2.Yarn 有什么特点？](#2yarn-有什么特点)
* [3.为什么要使用 Yarn。](#3为什么要使用-yarn)
* [4.yarn 主要作用](#4yarn-主要作用)
* [5.yarn 的结构](#5yarn-的结构)
* [6.Yarn 在运行过程中负责给应用分配资源的是什么](#6yarn-在运行过程中负责给应用分配资源的是什么)
* [7.yarn 的工作流程](#7yarn-的工作流程)
* [8.yarn 的调度器](#8yarn-的调度器)
* [9.YARN 高可用](#9yarn-高可用)
* [10.什么是 container？](#10什么是-container)
* [11.Yarn支持的调度器和硬件资源种类？](#11yarn支持的调度器和硬件资源种类)
* [12.请问RM节点上有Container容器的这种说法吗？](#12请问rm节点上有container容器的这种说法吗)
* [13.在AM中，job已经被分成一系列的task，并且是为每个task来startContainer。为什么NM上要存一个application的数据结构呢？](#13在am中job已经被分成一系列的task并且是为每个task来startcontainer为什么nm上要存一个application的数据结构呢)
* [14.是否只有负责启动ApplicationMaster的NodeManager才会维护一个Application对象？其他的NodeManager是否是根据ApplicationMaster发起的请求来启动属于这个Application的其他Container，这些NodeManager不需要维护Application的状态机？](#14是否只有负责启动applicationmaster的nodemanager才会维护一个application对象其他的nodemanager是否是根据applicationmaster发起的请求来启动属于这个application的其他container这些nodemanager不需要维护application的状态机)
* [15.Container的节点随机性？](#15container的节点随机性)

#### 1.简单介绍 yarn？

 yarn 是一个资源管理、任务调度的框架。主要包含三个模块：resourceManger、nodeManger、ApplicationMater。

####  2.Yarn 有什么特点？

1、支持多计算框架

2、资源利用率高、运行成本低、数据共享。

#### 3.为什么要使用 Yarn。

在 Hadoop1.x 时代没有 Yarn 调度，离线业务和实时业务需要两套集群

而 Hadoop2.x 增加了 Yarn，**Yarn 可以实现让离线业务和实时业务运行在一套集群上。**

 **降低了企业硬件的成本（多个集群变成一个集群），减少了资源的了浪费，运营成本低。**

#### 4.yarn 主要作用

资源管理，任务调度

#### 5.yarn 的结构

yarn 总体上是 master/slave 结构，主要由 ResourceManager、NodeManager、ApplicationMaster 和 Container 等几个组件组成。

- 2.1 RM
  RM 负责处理客户端请求，对各 NM 上的资源进行统一管理和调度，给 AplicationMaster 分配空闲的 Container 运行并监控其运行状态，主要由调度器和应用程序管理器构成。
  调度器仅根据各个应用程序的资源需求进行资源分配，而资源分配的单位是 Container，调度器不负责监控或者跟踪应用程序的状态。
  应用程序管理器主要负责整个系统中的所有的应用程序，包括应用程序的提交、与调度器协商资源以及启动 AppMaster，监控 AppMaster 运行状态并在失败的时候重新启动等。
- 2.2 NodeManager
  NM 是每个节点上的资源和任务管理器。它会定时地向 RM 汇报本节点上的资源使用情况和各个 Container 的运行情况。同时接收并处理来自 AppMaster 的 Container 启动停止等请求。
- 2.3 AppMaster
  用户提交的应用程序均包含一个 AppMaster，负责应用的监控，跟踪应用执行状态、重启失败任务等。AppMaster 是应用框架，它负责向 RM 协调资源，并且与 NodeManager 协同工作完成 Task 的执行和监控。
- 2.4 Container
  Yarn 中的资源抽象，它封装了某个节点上的多维度资源，如内存，CPU，磁盘，网络等。AppMaster 向 NodeManager 申请资源的时候，资源是以 container 的形式表示的。

#### 6.Yarn 在运行过程中负责给应用分配资源的是什么

在 Yarn 的 ResouceManger 的有一个 Schedule 和一个 ApplicationsManger。

其中 Schedule 负责给应用分配资源

ApplicationsManger 负责应用的管理与监控

#### 7.yarn 的工作流程

![img](https://img-blog.csdnimg.cn/20200329095837385.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RsY185OTY=,size_16,color_FFFFFF,t_70)

1. client 向 RM 提交应用程序，其中包括启动该应用的 ApplicationMaster 的必须信息，例如 ApplicationMaster 程序、            启动 ApplicationMaster 的命令、用户程序等。
2. ResourceManager 启动一个 container 用于运行 ApplicationMaster。
3. 启动中的 ApplicationMaster 向 ResourceManager 注册自己，启动成功后与 RM 保持心跳。
4. ApplicationMaster 向 ResourceManager 发送请求，申请相应数目的 container。
5. 申请成功的 container，由 ApplicationMaster 进行初始化。container 的启动信息初始化后，AM 与对应的 NodeManager 通信，要求 NM 启动 container。
6. NM 启动启动 container。
7. container 运行期间，ApplicationMaster 对 container 进行监控。container 通过 RPC 协议向对应的 AM 汇报自己的进度和状态等信息。
8. 应用运行结束后，ApplicationMaster 向 ResourceManager 注销自己，并允许属于它的 container 被收回。



#### 8.yarn 的调度器

4.1 FIFO Scheduler（队列调度器）
按任务提交的顺序排成一个队列，这是一个先进先出队列。在进行资源分配的时候，先给队列中最头上的任务分配资源，然后再分配给下一个。这是最简单也是最容易理解的调度器，但是它不适用与共享集群，大的任务会占用所有的集群资源，这就导致其它任务被阻塞。
4.2 Capacity Scheduler（容量调度器）
Capacity 调度器允许多个组织共享整个集群，每个组织可以获得集群的一部分计算能力。通过为每个组织分配专门的队列，然后再为每个队列分配一定的集群资源，这样整个集群就可以通过设置多个队列的方式为多个组织提供服务了。除此之外，队列内部又可以垂直划分，这样一个组织内部的多个成员就可以共享这个队列资源，在一个队列的内部，资源的调度采用的是先进先出策略。

![img](https://img-blog.csdnimg.cn/20210306182722396.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hvbmdtb2ZhbmcxMA==,size_16,color_FFFFFF,t_70)





4.3 Fair Scheduler(公平调度器)

![img](https://img-blog.csdnimg.cn/20210306182732637.png)

#### 9.YARN 高可用

YARN ResourceManager 的高可用与 HDFS NameNode 的高可用类似，但是 ResourceManager 不像 NameNode ，没有那么多的元数据信息需要维护，所以它的状态信息可以直接写到 Zookeeper 上，并依赖 Zookeeper 来进行主备选举。

![img](https://img-blog.csdnimg.cn/20210312203629459.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hvbmdtb2ZhbmcxMA==,size_16,color_FFFFFF,t_70)



#### 10.什么是 container？

 是一个抽象概念，称之为容器，包含任务运行时所需的资源（包括内存、硬盘、cpu 等）和环境（包含启动命令、环境变量等）



#### 11.Yarn支持的调度器和硬件资源种类？

Yarn自带的三种资源调度器，分辨是FIFO、Capacity Scheduler 和Fair Scheduler，其中，，FIFO是默认调度器，属于批量处理调度器，而后两个属于多租户调度器，它采用树形多队列的形式组织资源，更适合公司应用场景

YARN支持内存和CPU两种资源类型的管理和分配：YARN对内存资源和CPU资源采用了不同的资源隔离方案。对于内存资源，为了能够更灵活的控制内存使用量，YARN采用了进程监控的方案控制内存使用，即每个NodeManager会启动一个额外监控线程监控每个container内存资源使用量，一旦发现它超过约定的资源量，则会将其杀死；对于CPU资源，则采用了Cgroups进行资源隔离

#### 12.请问RM节点上有Container容器的这种说法吗？

这种说法是错误的，Container容器只运行在 Node Manager上面。

#### 13.在AM中，job已经被分成一系列的task，并且是为每个task来startContainer。为什么NM上要存一个application的数据结构呢？

在YARN看来，他所维护的所有应用程序叫appliction，但是到了计算框架这一层，各自有各自的名字，mapreduce叫job，storm叫topology等等，YARN是资源管理系统，不仅仅运行mapreduce，还有其他应用程序，mapreduce只是一种计算应用。但是yarn内部设有应用程序到计算框架应用程序的映射关系（通常是id的映射），你这里所说的应用程序，job属于不同层面的概念，切莫混淆，要记住，YARN是资源管理系统，可看做云操作系统，其他的东西，比如mapreduce，只是跑在yarn上的application，但是，mapreduce是应用层的东西，它可以有自己的属于，比如job task，但是yarn专业一层是不知道或者说看不到的。
#### 14.是否只有负责启动ApplicationMaster的NodeManager才会维护一个Application对象？其他的NodeManager是否是根据ApplicationMaster发起的请求来启动属于这个Application的其他Container，这些NodeManager不需要维护Application的状态机？
都需要维护，通过Application状态机可将节点上属于这个App的所有Container聚集在一起，当需要特殊操作，比如杀死Application时，可以将对应的所有Container销毁。
另外，需要注意，一个应用程序的ApplicationMaster所在的节点也可以运行它的container，这都是随机的。

#### 15.Container的节点随机性？

Container的节点随机性，我的理解是Container运行的节点是由分配资源时集群中哪些节点正好是空闲的来决定的，ResourceManager在为ApplicationMaster分配所需的Container的时候,完全有可能出现ApplicationMaster的本地节点上出现了空闲资源，这样，如果分配成功之后，ApplicationMaster就和所属的Container运行在一个节点上了。

#### 

https://blog.csdn.net/hongmofang10/article/details/114450888

https://blog.csdn.net/dlc_996/article/details/105176644

https://www.jianshu.com/p/db0b02d6f0fa

https://blog.csdn.net/qq_42246689/article/details/84590445

https://blog.csdn.net/shell33168/article/details/87928001