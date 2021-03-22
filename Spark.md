## Spark


* [1.spark有几种部署模式，每种模式的特点？](#1spark有几种部署模式每种模式的特点)
* [2.Spark技术栈有哪些组件，每个组件都有什么功能，适合什么应用场景？](#2spark技术栈有哪些组件每个组件都有什么功能适合什么应用场景)
* [3.spark有哪些组件](#3spark有哪些组件)
* [4.spark工作机制](#4spark工作机制)
* [5.Spark应用程序的执行过程](#5spark应用程序的执行过程)
* [6.driver的功能是什么？](#6driver的功能是什么)
* [7.Spark中Worker的主要工作是什么？](#7spark中worker的主要工作是什么)
* [8.task有几种类型？](#8task有几种类型)
* [9.什么是shuffle，以及为什么需要shuffle？](#9什么是shuffle以及为什么需要shuffle)
* [10.Spark master HA 主从切换过程不会影响集群已有的作业运行，为什么？](#10spark-master-ha-主从切换过程不会影响集群已有的作业运行为什么)
* [11.Spark并行度怎么设置比较合适](#11spark并行度怎么设置比较合适)
* [12.Spark程序执行，有时候默认为什么会产生很多task，怎么修改默认task执行个数？](#12spark程序执行有时候默认为什么会产生很多task怎么修改默认task执行个数)
* [13.Spark中数据的位置是被谁管理的？](#13spark中数据的位置是被谁管理的)
* [14.为什么要进行序列化](#14为什么要进行序列化)
* [15.Spark如何处理不能被序列化的对象？](#15spark如何处理不能被序列化的对象)
* [16.Spark提交你的jar包时所用的命令是什么？](#16spark提交你的jar包时所用的命令是什么)
* [17.Mapreduce和Spark的相同和区别](#17mapreduce和spark的相同和区别)
* [18.简单说一下hadoop和spark的shuffle相同和差异？](#18简单说一下hadoop和spark的shuffle相同和差异)
* [19. 简单说一下hadoop和spark的shuffle过程](#19-简单说一下hadoop和spark的shuffle过程)
* [20.partition和block的关联](#20partition和block的关联)
* [21.Spark为什么比mapreduce快？](#21spark为什么比mapreduce快)
* [22.Mapreduce操作的mapper和reducer阶段相当于spark中的哪几个算子？](#22mapreduce操作的mapper和reducer阶段相当于spark中的哪几个算子)
* [23.RDD机制](#23rdd机制)
* [24.RDD的弹性表现在哪几点？](#24rdd的弹性表现在哪几点)
* [25.RDD有哪些缺陷？](#25rdd有哪些缺陷)
* [26.什么是RDD宽依赖和窄依赖？](#26什么是rdd宽依赖和窄依赖)
* [27.rdd有几种操作类型？](#27rdd有几种操作类型)
* [28.Spark累加器有哪些特点？](#28spark累加器有哪些特点)
* [29.spark hashParitioner的弊端](#29spark-hashparitioner的弊端)
* [30.RangePartitioner分区的原理](#30rangepartitioner分区的原理)
* [参考资料](#参考资料)


#### 1.spark有几种部署模式，每种模式的特点？

本地模式
Spark不一定非要跑在hadoop集群，可以在本地，起多个线程的方式来指定。方便调试，本地模式分三类
local：只启动一个executor
local[k]: 启动k个executor
local：启动跟cpu数目相同的 executor

standalone模式
分布式部署集群，自带完整的服务，资源管理和任务监控是Spark自己监控，这个模式也是其他模式的基础

Spark on yarn模式
分布式部署集群，资源和任务监控交给yarn管理
粗粒度资源分配方式，包含cluster和client运行模式
cluster 适合生产，driver运行在集群子节点，具有容错功能
client 适合调试，dirver运行在客户端

Spark On Mesos模式

#### 2.Spark技术栈有哪些组件，每个组件都有什么功能，适合什么应用场景？

Spark core
是其它组件的基础，spark的内核
主要包含：有向循环图、RDD、Lingage、Cache、broadcast等

SparkStreaming
是一个对实时数据流进行高通量、容错处理的流式处理系统
将流式计算分解成一系列短小的批处理作业

Spark sql
能够统一处理关系表和RDD，使得开发人员可以轻松地使用SQL命令进行外部查询

MLBase
是Spark生态圈的一部分专注于机器学习，让机器学习的门槛更低
MLBase分为四部分：MLlib、MLI、ML Optimizer和MLRuntime。

GraphX
是Spark中用于图和图并行计算

#### 3.spark有哪些组件

master：管理集群和节点，不参与计算。
worker：计算节点，进程本身不参与计算，和master汇报。
Driver：运行程序的main方法，创建spark context对象。
spark context：控制整个application的生命周期，包括dagsheduler和task scheduler等组件。
client：用户提交程序的入口。

#### 4.spark工作机制

用户在client端提交作业后，会由Driver运行main方法并创建spark context上下文。
执行add算子，形成dag图输入dagscheduler
按照add之间的依赖关系划分stage输入task scheduler
task scheduler会将stage划分为taskset分发到各个节点的executor中执行

#### 5.Spark应用程序的执行过程

构建Spark Application的运行环境（启动SparkContext）
SparkContext向资源管理器（可以是Standalone、Mesos或YARN）注册并申请运行Executor资源；
资源管理器分配Executor资源，Executor运行情况将随着心跳发送到资源管理器上；
SparkContext构建成DAG图，将DAG图分解成Stage，并把Taskset发送给Task Scheduler
Executor向SparkContext申请Task，Task Scheduler将Task发放给Executor运行，SparkContext将应用程序代码发放给Executor。
Task在Executor上运行，运行完毕释放所有资源。

#### 6.driver的功能是什么？

一个Spark作业运行时包括一个Driver进程，也是作业的主进程，具有main函数，并且有SparkContext的实例，是程序的人口点；

功能：
向集群申请资源
负责了作业的调度和解析
生成Stage并调度Task到Executor上（包括DAGScheduler，TaskScheduler）

#### 7.Spark中Worker的主要工作是什么？

管理当前节点内存，CPU的使用状况，接收master分配过来的资源指令，通过ExecutorRunner启动程序分配任务
worker就类似于包工头，管理分配新进程，做计算的服务，相当于process服务
worker不会运行代码，具体运行的是Executor是可以运行具体appliaction写的业务逻辑代码

#### 8.task有几种类型？

resultTask类型，最后一个task
shuffleMapTask类型，除了最后一个task都是

#### 9.什么是shuffle，以及为什么需要shuffle？

shuffle中文翻译为洗牌，需要shuffle的原因是：某种具有共同特征的数据汇聚到一个计算节点上进行计算

#### 10.Spark master HA 主从切换过程不会影响集群已有的作业运行，为什么？

因为程序在运行之前，已经申请过资源了，driver和Executors通讯，不需要和master进行通讯的。

#### 11.Spark并行度怎么设置比较合适

spark并行度，每个core承载2~4个partition（并行度）
并行读和数据规模无关，只和内存和cpu有关

#### 12.Spark程序执行，有时候默认为什么会产生很多task，怎么修改默认task执行个数？

有很多小文件的时候，有多少个输入block就会有多少个task启动
spark中有partition的概念，每个partition都会对应一个task，task越多，在处理大规模数据的时候，就会越有效率

#### 13.Spark中数据的位置是被谁管理的？

每个数据分片都对应具体物理位置，数据的位置是被blockManager管理

#### 14.为什么要进行序列化

减少存储空间，高效存储和传输数据，缺点：使用时需要反序列化，非常消耗CPU

#### 15.Spark如何处理不能被序列化的对象？

封装成object

#### 16.Spark提交你的jar包时所用的命令是什么？

spark-submit

#### 17.Mapreduce和Spark的相同和区别

1）两者都是用mr模型来进行并行计算
2）hadoop的一个作业：job
job分为map task和reduce task，每个task都是在自己的进程中运行的
当task结束时，进程也会结束
3）spark用户提交的任务：application
一个application对应一个sparkcontext，app中存在多个job
每触发一次action操作就会产生一个job
这些job可以并行或串行执行
每个job中有多个stage，stage是shuffle过程中DAGSchaduler通过RDD之间的依赖关系划分job而来的
每个stage里面有多个task，组成taskset有TaskSchaduler分发到各个executor中执行
executor的生命周期是和app一样的，即使没有job运行也是存在的，所以task可以快速启动读取内存进行计算。
4）hadoop的job只有map和reduce操作，表达能力比较欠缺
在mr过程中会重复的读写hdfs，造成大量的io操作，多个job需要自己管理关系。
5）spark的迭代计算都是在内存中进行的
API中提供了大量的RDD操作如join，groupby等
通过DAG图可以实现良好的容错

#### 18.简单说一下hadoop和spark的shuffle相同和差异？

1）high-level 角度：
两者并没有大的差别 都是将 mapper（Spark: ShuffleMapTask）的输出进行 partition，不同的 partition 送到不同的 reducer（Spark 里 reducer 可能是下一个 stage 里的 ShuffleMapTask，也可能是 ResultTask）
Reducer 以内存作缓冲区，边 shuffle 边 aggregate 数据，等到数据 aggregate 好以后进行 reduce()。

2）low-level 角度：
Hadoop MapReduce 是 sort-based，进入 combine() 和 reduce() 的 records 必须先 sort。
好处：combine/reduce() 可以处理大规模的数据
因为其输入数据可以通过外排得到
mapper 对每段数据先做排序
reducer 的 shuffle 对排好序的每段数据做归并
Spark 默认选择的是 hash-based，通常使用 HashMap 来对 shuffle 来的数据进行 aggregate，不提前排序
如果用户需要经过排序的数据：sortByKey()

3）实现角度：
Hadoop MapReduce 将处理流程划分出明显的几个阶段：map(), spilt, merge, shuffle, sort, reduce()
Spark 没有这样功能明确的阶段，只有不同的 stage 和一系列的 transformation()，spill, merge, aggregate 等操作需要蕴含在 transformation() 中

#### 19. 简单说一下hadoop和spark的shuffle过程

hadoop：map端保存分片数据，通过网络收集到reduce端
spark：spark的shuffle是在DAGSchedular划分Stage的时候产生的，TaskSchedule要分发Stage到各个worker的executor，减少shuffle可以提高性能

#### 20.partition和block的关联

hdfs中的block是分布式存储的最小单元，等分，可设置冗余，这样设计有一部分磁盘空间的浪费，但是整齐的block大小，便于快速找到、读取对应的内容
Spark中的partition是RDD的最小单元，RDD是由分布在各个节点上的partition组成的。
partition是指的spark在计算过程中，生成的数据在计算空间内最小单元
同一份数据（RDD）的partion大小不一，数量不定，是根据application里的算子和最初读入的数据分块数量决定
block位于存储空间；partion位于计算空间，block的大小是固定的、partion大小是不固定的，是从2个不同的角度去看数据。

#### 21.Spark为什么比mapreduce快？

基于内存计算，减少低效的磁盘交互
高效的调度算法，基于DAG
容错机制Linage

#### 22.Mapreduce操作的mapper和reducer阶段相当于spark中的哪几个算子？

相当于spark中的map算子和reduceByKey算子，区别：MR会自动进行排序的，spark要看具体partitioner

#### 23.RDD机制

分布式弹性数据集，简单的理解成一种数据结构，是spark框架上的通用货币
所有算子都是基于rdd来执行的
rdd执行过程中会形成dag图，然后形成lineage保证容错性等
从物理的角度来看rdd存储的是block和node之间的映射

#### 24.RDD的弹性表现在哪几点？

自动的进行内存和磁盘的存储切换；
基于Lingage的高效容错；
task如果失败会自动进行特定次数的重试；
stage如果失败会自动进行特定次数的重试，而且只会计算失败的分片；
checkpoint和persist，数据计算之后持久化缓存
数据调度弹性，DAG TASK调度和资源无关
数据分片的高度弹性，a.分片很多碎片可以合并成大的，b.par

#### 25.RDD有哪些缺陷？

不支持细粒度的写和更新操作（如网络爬虫）
spark写数据是粗粒度的，所谓粗粒度，就是批量写入数据 （批量写）
但是读数据是细粒度的也就是说可以一条条的读 （一条条读）

不支持增量迭代计算，Flink支持

#### 26.什么是RDD宽依赖和窄依赖？

RDD和它依赖的parent RDD(s)的关系有两种不同的类型
窄依赖：每一个parent RDD的Partition最多被子RDD的一个Partition使用 （一父一子）
宽依赖：多个子RDD的Partition会依赖同一个parent RDD的Partition （一父多子）

#### 27.rdd有几种操作类型？

transformation，rdd由一种转为另一种rdd
action
cronroller，控制算子(cache/persist) 对性能和效率的有很好的支持

#### 28.Spark累加器有哪些特点？

全局的，只增不减，记录全局集群的唯一状态
在exe中修改它，在driver读取
executor级别共享的，广播变量是task级别的共享
两个application不可以共享累加器，但是同一个app不同的job可以共享

#### 29.spark hashParitioner的弊端

分区原理：对于给定的key，计算其hashCode
弊端是数据不均匀，容易导致数据倾斜

#### 30.RangePartitioner分区的原理

尽量保证每个分区中数据量的均匀，而且分区与分区之间是有序的，也就是说一个分区中的元素肯定都是比另一个分区内的元素小或者大
分区内的元素是不能保证顺序的
简单的说就是将一定范围内的数映射到某一个分区内

#### 参考资料

https://www.jianshu.com/p/7a8fca3838a4