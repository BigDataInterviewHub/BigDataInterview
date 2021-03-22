## Storm


* [1.什么是 storm？](#1-什么是-storm)
* [2.提高并发度](#2提高并发度)
* [3.当 Nimbus 或 Supervisor 守护进程死亡时会发生什么？](#3当-nimbus-或-supervisor-守护进程死亡时会发生什么)
* [4.Nimbus 是单点故障吗？](#4nimbus-是单点故障吗)
* [5.Storm 如何保证数据处理？](#5storm-如何保证数据处理)
* [6.storm 的可靠性如何实现，包括 spout 和 bolt 两部分？](#6storm-的可靠性如何实现包括-spout-和-bolt-两部分)
* [7.storm 分组策略方式？](#7storm-分组策略方式)
* [8.Storm 的物理架构？](#8storm-的物理架构)
* [9.Storm 实时低延迟的原因](#9storm-实时低延迟的原因)
* [10.离线计算是什么？流式计算是什么？](#10离线计算是什么流式计算是什么)
* [11.Storm 与 Hadoop 的区别](#11storm-与-hadoop-的区别)
* [12.Storm 核心组件](#12storm-核心组件)
* [13.当一个 worker 挂掉时会发生什么?](#13当一个-worker-挂掉时会发生什么)
* [14.当一个 node（节点）挂掉时会发生什么?](#14-当一个-node节点挂掉时会发生什么)
* [15.当 Nimbus 或 Supervisor 守护进程死亡时会发生什么？](#15当-nimbus-或-supervisor-守护进程死亡时会发生什么)
* [16.流的模式是什么？默认是什么？](#16流的模式是什么默认是什么)
* [17.Storm Group 分类](#17storm-group-分类)
* [18.Storm 的特点和特性是什么？](#18storm-的特点和特性是什么)
* [19.storm 编程模型？](#19storm-编程模型)
* [20.Spark Streaming 和 Storm 有什么不同？](#20spark-streaming-和-storm-有什么不同)
* [参考链接](#参考链接)


#### 1.什么是 storm？
- Storm 是 Twitter 开源的分布式实时大数据处理框架，被业界称为实时版 Hadoop。随着越来越多的场景对 Hadoop 的 MapReduce 高延迟无法容忍，比如网站统计、推荐系统、预警系统、金融系统 (高频交易、股票) 等等，大数据实时处理解决方案 (流计算) 的应用日趋广泛, 目前已是分布式技术领域最新爆发点，而 Storm 更是流计算技术中的佼佼者和主流。
- 按照 storm 作者的说法， Storm 对于实时计算的意义类似于 Hadoop 对于批处理的意义。Hadoop 提供了 map、reduce 原语，使我们的批处理程序变得简单和高效。同样，Storm 也为实时计算提供了一些简单高效的原语， 而且 Storm 的 Trident 是基于 Storm 原语更高级的抽象框架，类似于基于 Hadoop 的 Pig 框架，让开发更加便利和高效。

Apache Storm 核心概念

- Nimbus：Storm 集群主节点，负责资源分配和任务调度。我们提交任务和截止任务都是在 Nimbus 上操作的。一个 Storm 集群只有一个 Nimbus 节点。

- Supervisor：Storm 集群工作节点，接受 Nimbus 分配任务，管理所有 Worker。

- Worker：工作进程，每个工作进程中都有多个 Task。

- Task：任务，每个 Spout 和 Bolt 都是一个任务，每个任务都是一个线程。

- Topology：计算拓扑，包含了应用程序的逻辑。

- Stream：消息流，关键抽象，是没有边界的 Tuple 序列。

- Spout：消息流的源头，Topology 的消息生产者。

- Bolt：消息处理单元，可以过滤、聚合、查询数据库。

- Stream grouping：消息分发策略，一共 6 种，定义每个 Bolt 接受何种输入。

- Reliability：可靠性，Storm 保证每个 Tuple 都会被处理。

  ![img](https://img-blog.csdnimg.cn/20210203161226112.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM0MjMzMg==,size_16,color_FFFFFF,t_70)

#### 2.提高并发度

- 工作进程的数量 topology.workers : 1
- 执行器的数量
- 任务的数量

#### 3.当 Nimbus 或 Supervisor 守护进程死亡时会发生什么？

Nimbus 或 supervisor 的死亡不会影响 worker 流程，遇到任何意外情况时进程自毁，所有状态保存在 Zookeeper 或磁盘上

#### 4.Nimbus 是单点故障吗？
如果丢失了 Nimbus 节点，worker 仍将继续运行. 如果 worker 死掉，nimbus 将继续重新 worker。但是，如果没有 Nimbus，worker 将不会在必要时重新分配给其他计算机

#### 5.Storm 如何保证数据处理？

- Storm 提供了保证数据处理的机制
- worker 死了，nimbus 可以启动 worker. 如果它在启动时连续失败并且无法接受 Nimbus 的心跳，Nimbus 将重新安排 worker
- 节点死了，分配给该计算机的任务将超时，Nimbus 会将这些任务重新分配给其他计算机

#### 6.storm 的可靠性如何实现，包括 spout 和 bolt 两部分？

- spout 端 ：
  - ack 机制与 fail 机制
  - 对于 tuple 树上的每个 bolt 进行确认应答，spout 才会调用 ack 来表明这个消息被完全处理，如果任何一个 bolt 处理的 tuple 报错。调用 ack.
- bolt 端：
  - ack 机制与 fail 机制

#### 7.storm 分组策略方式？

- Shuffle Grouping: 随机分组，轮询，平均分配
- Fields Grouping：按字段分组
- All Grouping：广播发送
- Global Grouping：全局分组
- Non Grouping：不分组
- Direct Grouping：直接分组
- Local or shuffle grouping
- 自定义分组

#### 8.Storm 的物理架构？

![img](https://img-blog.csdnimg.cn/20201012113925637.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM0MjMzMg==,size_16,color_FFFFFF,t_70#pic_center)


**nimbus:**
●Storm 的 Master, 负责资源分配和任务调度。一个 Storm 集群只有一个 Nimbus。
●集群的主节点，对整个集群的资源使用情况进行管理
●但是 nimbus 是一个无状态的节点， 所有的一切都存储在 Zookeeper
**supervisor:**
●Storm 的 Slave，负责接收 Nimbus 分配的任务，管理所有 Worker
●一个 Supervisor 节点中包含多个 Worker 进程，默认是 4 个
●一般情况下一个 topology 对应一个 worker
**woker:**
●工作进程，每个工作进程中都有多个 Task
**Task**
●在 Storm 集群中每个 Spout 和 Bolt 都由若干个任务 (tasks) 来执行。
●worker 中每一个 spout/bolt 的线程称为一个 task
●同一个 spout/bolt 的 task 可能会共享一个物理线程该线程称为 executor

与 Hadoop 主从架构一样, Storm 也采用 Master/Slave 体系结构, 分布式计算由 Nimbus 和 Supervisor 两类服务进程实现, Nimbus 进程运行在集群的主节点, 负责任务的指派和分发, Supervisor 运行在集群的从节点, 负责执行任务的具体部分。
storm 架构中使用 Spout/Bolt 编程模型来对消息进行流式处理｡消息流是 storm 中对数据的基本抽象, 一个消息流是对一条输入数据的封装, 源源不断输入的消息流以分布式的方式被处理，Spout 组件是消息生产者，是 storm 架构中的数据输入源头，它可以从多种异构数据源读取数据，并发射消息流，Bolt 组件负责接收 Spout 组件发射的信息流，并完成具体的处理逻辑｡在复杂的业务逻辑中可以串联多个 Bolt 组件，在每个 Bolt 组件中编写各自不同的功能，从而实现整体的处理逻辑。

#### 9.Storm 实时低延迟的原因

Storm 实时低延迟，主要有两个原因：
1）Storm 进程是常驻内存的，不像 hadoop 里面是不断的启停的，就没有不断启停的开销。
2）Storm 的数据是不经过磁盘的，都是在内存里面，处理完就没有了，数据的交换经过网络，这样就避免磁盘 IO 的开销，所以 Storm 可以很低的延迟。

#### 10.离线计算是什么？流式计算是什么？

离线计算：批量获取数据、批量传输数据、周期性批量计算数据、数据展示。
代表技术：Sqoop 批量导入数据、HDFS 批量存储数据、MapReduce 批量计算数据、Hive 批量计算数据。

流式计算：数据实时产生、数据实时传输、数据实时计算、实时展示。
代表技术：Flume 实时获取数据、Kafka 实时数据存储、Storm/JStorm 实时数据计算、Redis 实时结果缓存、持久化存储 (mysql)。

离线计算与实时计算最大的区别: 实时收集、实时计算、实时展示。

#### 11.Storm 与 Hadoop 的区别
1）Storm 用于实时计算，Hadoop 用于离线计算。
2）Storm 处理的数据保存在内存中，源源不断; Hadoop 处理的数据保存在文件系统中，一批一批处理。
3）Storm 的数据通过网络传输进来；Hadoop 的数据保存在磁盘中。
4）Storm 与 Hadoop 的编程模型相似。

#### 12.Storm 核心组件

![img](https://img-blog.csdnimg.cn/20201023001730368.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM0MjMzMg==,size_16,color_FFFFFF,t_70#pic_center)


**Nimbus（主节点）**：负责资源分配和任务调度。
**Supervisor（从节点）**：负责接受 nimbus 分配的任务，启动和停止属于自己管理的 worker 进程。(通过配置文件设置当前 supervisor 上启动多少个 worker。worker 的数量根据端口号来的！)
**Worker（进程）**：运行具体处理组件逻辑的进程（其实就是一个 JVM）。Worker 运行的任务类型只有两种，一种是 Spout 任务，一种是 Bolt 任务。
**Task（线程）**：worker 中每一个 spout/bolt 的线程称为一个 task. 在 storm0.8 之后，task 不再与物理线程对应，不同 spout/bolt 的 task 可能会共享一个物理线程，该线程称为 executor。task = 线程 = executor
**Zookeeper（分布式协调服务）** : 保存任务分配的信息、心跳信息、元数据信息。

nimbus 是整个集群的控管核心，负责 topology 的提交、运行状态监控、任务重新分配等工作。zk 就是一个管理者，监控者。
总体描述：nimbus 下命令 (分配任务)，zk 监督执行 (心跳监控，worker、supurvisor 的心跳都归它管)，supervisor 领旨 (下载代码)，招募人马 (创建 worker 和线程等)，worker、 executor 就给我干活! task 就是具体要干的活。
执行器 (Executor) ：一个线程就是一个 executor，一个线程会处理一个或多个任务 (task)。

#### 13.当一个 worker 挂掉时会发生什么?
当一个 worker 挂掉时, supervisor 将会重启它。如果在启动它时继续发生故障并且没有发送 hearbeat（心跳）给 Nimbus，那么 Nimbus 将会重新调度 worker。

#### 14.当一个 node（节点）挂掉时会发生什么?
分配给该机器的 task（任务）将超时，Nimbus 将这些 task（任务）重新分配给其他机器。

#### 15.当 Nimbus 或 Supervisor 守护进程死亡时会发生什么？

Nimbus 和 Supervisor 守护进程是为 fail-fast（快速失败）（遇到任何意外情况时进程自毁）和无状态（所有状态都保存在 Zookeeper 或磁盘上）而设计的。Nimbus 和 Supervisor 守护进程必须使用 daemontools 或 monit 工具进行监督。所以如果 Nimbus 或 Supervisor 守护进程挂掉后, 它们会像什么都没发生一样重启。

最值得注意的是，没有 worker 进程受到 Nimbus 或 Supervisors 挂掉的影响。这与 Hadoop 相反, 如果 JobTracker 挂掉, 所有正在运行的 job 作业都将丢失。

16.Nimbus 是单点故障的吗?
如果你失去了 Nimbus 节点, workers 仍然会继续工作。此外，supervisors 如果挂掉, 将继续重新启动 workers。但是，如果没有 Nimbus，worker 在必要时不会重新分配给其他机器（如失去 worker 机器）。

Storm Nimbus 自 1.0.0 以来是 highly available（高可用的）。

#### 16.流的模式是什么？默认是什么？

　　流是 Storm 中的核心抽象。一个流由无限的元组序列组成，这些元组会被分布式并行地创建和处理。通过流中元组包含的字段名称来定义这个流。
　　每个流声明时都被赋予了一个 ID。只有一个流的 Spout 和 Bolt 非常常见，所以 OutputFieldsDeclarer 提供了不需要指定 ID 来声明一个流的函数 (Spout 和 Bolt 都需要声明输出的流)。这种情况下，流的 ID 是默认的 “default”。

#### 17.Storm Group 分类
**Shuffle Grouping**：随机分组， 随机派发 stream 里面的 tuple， 保证 bolt 中的每个任务接收到的 tuple 数目相同。(它能实现较好的负载均衡)。

**Fields Grouping**：按字段分组， 比如按 userid 来分组， 具有同样 userid 的 tuple 会被分到同一任务， 而不同的 userid 则会被分配到不同的任务。

**All Grouping：**：广播发送，对于每一个 tuple，Bolts 中的所有任务都会收到。

**Global Grouping**：全局分组，这个 tuple 被分配到 storm 中的一个 bolt 的其中一个 task，再具体一点就是分配给 id 值最低的那个 task。

**Non Grouping**：随机分派，意思是说 stream 不关心到底谁会收到它的 tuple. 目前他和 Shuffle grouping 是一样的效果,

**Direct Grouping**：直接分组, 这是一种比较特别的分组方法，用这种分组意味着消息的发送者具体由消息接收者的哪个 task 处理这个消息。只有被声明为 Direct Stream 的消息流可以声明这种分组方法，而且这种消息 tuple 必须使用 emitDirect 方法来发射。消息处理者可以通过 TopologyContext 来或者处理它的消息的 taskid (OutputCollector.emit 方法也会返回 taskid)。

**localOrShuffleGrouping**：是指如果目标 Bolt 中的一个或者多个 Task 和当前产生数据的 Task 在同一个 Worker 进程里面，那么就走内部的线程间通信，将 Tuple 直接发给在当前 Worker 进程的目的 Task。否则，同 shuffleGrouping。（在工作中使用的频率还是比较高的）

**CustomStreamGrouping**：自定义流式分组。

#### 18.Storm 的特点和特性是什么？
编程简单：开发人员只需要关注应用逻辑，而且跟 Hadoop 类似，Storm 提供的编程原语也很简单；
高性能，低延迟：可以应用于广告搜索引擎这种要求对广告主的操作进行实时响应的场景；
分布式：可以轻松应对数据量大，单机搞不定的场景；
可扩展： 随着业务发展，数据量和计算量越来越大，系统可水平扩展；
容错：单个节点挂了不影响应用；
消息不丢失：保证消息处理；

#### 19.storm 编程模型？

![img](https://img-blog.csdnimg.cn/20210203160656219.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM0MjMzMg==,size_16,color_FFFFFF,t_70)

**Topology**：Storm 中运行的一个实时应用程序的名称。（拓扑）
**Spout**：在一个 topology 中获取源数据流的组件。 通常情况下 spout 会从外部数据源中读取数据，然后转换为 topology 内部的源数据。
**Bolt**：接受数据然后执行处理的组件, 用户可以在其中执行自己想要的操作。
**Tuple**：一次消息传递的基本单元，理解为一组消息就是一个 Tuple。
**Stream**：表示数据的流向。

#### 20.Spark Streaming 和 Storm 有什么不同？

![img](https://img-blog.csdnimg.cn/20210203162216481.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM0MjMzMg==,size_16,color_FFFFFF,t_70)


区别是 SparkStreaming 的**吞吐量**非常高，秒级准实时处理，Storm 是**容错性**非常高，毫秒级实时处理
解释：sparkStreaming 是一次处理某个间隔的数据，比如 5 秒内的数据，批量处理，所以吞吐量高。
Storm 是来一条处理一条，所以速度快，不存在丢失数据。
应用场景：对于数据非常重要不能丢失数据的，不能有延迟的，比如股票，金融之类场景的使用 Storm。
对于没那么高精度，但是要处理大量的数据，可以用 sparkSremaing。





SparkStreaming 是流式处理框架，是 Spark API 的扩展，支持可扩展、高吞吐量、容错的实时数据流处理，实时数据的来源可以是：Kafka（Kafka 和 SparkStreaming 是黄金组合）, Flume, Twitter, ZeroMQ 或者 TCP sockets，并且可以使用高级功能的复杂算子来处理流数据。例如：map,reduce,join,window 。最终，处理后的数据可以存放在文件系统，数据库等，方便实时展现
同样作为流式处理框架，SparkStreaming 和 Storm 的区别在于：



- Storm 是实时处理数据，SparkStreaming 是微批处理数据，因此 SparkStreaming 的吞吐量要比 Storm 高；

- Storm 适合处理实时数据，SparkStreaming 适合处理流数据。SparkStreaming 的高吞吐量，使得其计算逻辑必然可以处理复杂业务；

- Storm 的事务更加完善（ack 保障机制，数据有 100 条就直接处理 100 条），SparkStreaming 可以管理事务（100 条数据处理完 50 条，可以手动管理处理剩下的 50 条）；

- Storm 和 SparkStreaming 都支持动态资源调度，不过最好别开启（资源一旦释放掉，有可能就要不回来了）。

  ![img](https://img-blog.csdnimg.cn/20210203162646464.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM0MjMzMg==,size_16,color_FFFFFF,t_70)

#### 参考链接

https://blog.csdn.net/elpsyco/article/details/102711171

https://blog.csdn.net/weixin_44342332/article/details/113415623