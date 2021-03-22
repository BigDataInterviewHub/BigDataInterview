## Impala

* [1.Impala是什么](#1impala是什么)
* [2.Impala特点](#2impala特点)
* [3.Impala的缺点](#3impala的缺点)
* [4.说下Impala的核心组件](#4说下impala的核心组件)
* [5.Impala的整体架构流程了解吗？](#5impala的整体架构流程了解吗)
* [6.Impala与hive的异同了解吗](#6impala与hive的异同了解吗)
* [参考资料](#参考资料)

#### 1.Impala是什么

Impala是由Cloudera公司开发的新型查询系统，能够对存储在HDFS、HBase以及S3上的数据进行快速的交互式SQL查询。
另外，impala与Hive使用了统一的存储系统、同样的元数据库、SQL语法(Hive SQL)、ODBC驱动和用户交互接口(Hue)，
Impala对实时的或者面向批处理的查询提供了一个统一的平台，Impala在性能上比Hive高出3~30倍。


#### 2.Impala特点

基于内存进行计算，能够对PB级数据进行交互实时查询、分析
无需转换为MR，直接读取HDFS数据，大大降低了延迟
C++编写
兼容HiveSQL
具有数据仓库的特性，可以对hive数据直接做数据分析
支持数据本地化
支持列式存储
支持JDBC远程访问

#### 3.Impala的缺点

对内存依赖大
分区超过1w时，性能严重下降
不提供对序列化和反序列化的支持

#### 4.说下Impala的核心组件

Statestore Daemon
负责收集分布在集群中各个impalad进程的资源信息、各节点的健康状况，同步节点信息
负责query的调度

Catalog Daemon
从hive元数据库中同步元数据，分发表的元数据信息到各个impalad中
接收来自statestore的所有请求

Impala Daemon（impalad）
具有数据本地化的特性所以放在DataNode上
接收client、hue、jdbc的请求，query执行并返回给中心协调节点
子节点上的守护进程，负责向statestore保持通信，汇报工作
Impala daemon执行计算。因内存依赖大，所以最好不要和impala的其他组件放到同一节点
考虑到集群性能问题，一般将StateStoreDaemon与Catalog Daemon放在同一个节点，因为之间要做通信

#### 5.Impala的整体架构流程了解吗？

1）客户端向某个impalad发送一个query（SQL）

impalad会与StateStore通信，确定impala集群哪些impalad是否健康可用；与NameNode得到数据元数据信息；每个impalad通过catalog可知道表元数据信息

2）impalad将query解析为具体的执行计划planner，交给当前机器coordinator（中心协调节点）

Impalad通过jni，将query传送给java前端，由java前端完成语法分析和生成执行计划(Planner)，并将执行计划封装成thrift格式返回执行计划分为多个阶段，每一个阶段叫做一个（计划片段）PlanFragment，每一个PlanFragment在执行时可以由多个Impalad实例并行执行(有些PlanFragment只能由一个Impalad实例执行)

3）Coordinator根据执行计划Planner，通知本机Executor执行，并转发给其他有数据的Impalad用Executor执行

4）impalad的executor之间可进行通信，需要一些数据的处理

5）各个impalad的executor执行完成后，将结果返回给中心协调节点

6）由中心协调节点Coordinator将汇聚的查询结果返回给客户端

#### 6.Impala与hive的异同了解吗

数据存储
都把数据存储于HDFS

元数据
使用相同的元数据

SQL解释处理
使用通过词法解析生成执行计划

执行计划
hive：依赖于MapReduce框架，如果一个Query被翻译成多轮MapReduce，则会有更多的写中间结果。由于MapReduce执行框架本身的特点，过多的中间过程会增加整个query的执行时间
impala：把执行计划表现为一颗完整的可执行计划树，可以更加自然的分发执行计划到各个impalad执行查询，保证impalad有更好的并发性和避免不必要的中间sort和shuffle

数据流
hive：采用推的方式，每一个计算节点结算完成后将数据主动推给后续节点
impala：采用拉的方式，后续节点通过getNext向前面节点获取数据

内存使用
hive：在执行过程中如果内存放不下所有数据，则会使用外存
Impala：在遇到内存放不下数据时，当前版本1.0.1是直接返回错误

调度
hive：依赖于hadoop的能错能力，做到恨到的failover
impala：不存在任何容错逻辑，如果执行过程中发生故障，则直接返回错误


#### 参考资料

https://blog.csdn.net/ifenggege/article/details/107461089

https://blog.csdn.net/zhazhagu/article/details/106753223