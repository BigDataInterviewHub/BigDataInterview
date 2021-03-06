# BigDataInterview


这个Repo主要用来分享大数据面试题，目前已经涵盖 Hadoop、HBase、CBoard & Kylin、Cassandra、Flink、Flume、Hive、Impala、Oozie、Presto、Spark、Sqoop、Storm等内容，后续还会不断更新。

如果对于有所帮助，可以给个star。有纰漏的地方，欢迎给我们提PR。

如果想获取本Repo的PDF版本，可以用微信扫描下方二维码，回复 “bd” ，即可获取。如果二维码加载不出来，可以在微信搜索公众号 “程序员百科全书”，回复 “bd” ，即可获取PDF版本。


<img src="https://github.com/JavaInterviewHub/JavaInterview/blob/main/imgs/%E7%A8%8B%E5%BA%8F%E5%91%98%E7%99%BE%E7%A7%91%E5%85%A8%E4%B9%A6.jpg"/>



## Cassandra

* [1.向Cassandra讲解](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cassandra.md#1向cassandra讲解)
* [2.Cassandra用哪种语言写？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cassandra.md#2cassandra用哪种语言写)
* [3.Cassandra(Cassandra)的原始作者是谁？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cassandra.md#3cassandracassandra的原始作者是谁)
* [4.Cassandra数据库中使用哪种查询语言？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cassandra.md#4cassandra数据库中使用哪种查询语言)
* [5.Cassandra的优点/优点是什么？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cassandra.md#5cassandra的优点优点是什么)
* [6.是否提到了Cassandra数据模型的一些重要组成部分？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cassandra.md#6是否提到了cassandra数据模型的一些重要组成部分)
* [5.数据模型（Data Model）](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cassandra.md#5数据模型data-model)
* [6.列（Colunmn）](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cassandra.md#6列colunmn)
* [7.列族（Column Family）](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cassandra.md#7列族column-family)
* [8.超列族（Super Column Family）](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cassandra.md#8超列族super-column-family)
* [9.KeySpaces](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cassandra.md#9keyspaces)
* [10.Clusters](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cassandra.md#10clusters)
* [11.Cassandra的其他成分是什么？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cassandra.md#11cassandra的其他成分是什么)
* [12.Cassandra中有哪些不同的组合键？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cassandra.md#12cassandra中有哪些不同的组合键)
* [13.什么是Cassandra中的数据复制？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cassandra.md#13什么是cassandra中的数据复制)
* [14.Cassandra中的数据中心是什么意思？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cassandra.md#14cassandra中的数据中心是什么意思)
* [15.cassandra用了哪些端口？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cassandra.md#15cassandra用了哪些端口)
* [16.是不是单个seed意味着单点故障？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cassandra.md#16是不是单个seed意味着单点故障)
* [17.为什么不可以在jconsole里调用某个jmx方法呢？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cassandra.md#17为什么不可以在jconsole里调用某个jmx方法呢)
* [18.为什么我会在日志文件里看到 “… messages dropped …”这样的信息？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cassandra.md#18为什么我会在日志文件里看到--messages-dropped-这样的信息)
* [19.Cassandra因为java.lang.OutOfMemoryError: Map failed挂掉了](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cassandra.md#19cassandra因为javalangoutofmemoryerror-map-failed挂掉了)
* [20.如果再同一时刻发生两次更新会发生什么？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cassandra.md#20如果再同一时刻发生两次更新会发生什么)
* [21.为什么在加入一个新节点的时候，会有Stream failed错误？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cassandra.md#21为什么在加入一个新节点的时候会有stream-failed错误)
* [参考链接](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cassandra.md#参考链接)

## CBoard & Kylin

* [1.CBoard](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cboard_Kylin.md#1cboard)
* [2.CBoard特性](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cboard_Kylin.md#2cboard特性)
* [3.Kylin的优点和缺点？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cboard_Kylin.md#3kylin的优点和缺点)
* [4.Kylin的rowkey如何设计？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cboard_Kylin.md#4kylin的rowkey如何设计)
* [5.Kylin的cuboid,cube和segment的关系？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cboard_Kylin.md#5kylin的cuboidcube和segment的关系)
* [6.一张hive宽表有5个维度，kylin构建cube的时候我选了4个维度，我select *的时候会有几个维度字段？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cboard_Kylin.md#6一张hive宽表有5个维度kylin构建cube的时候我选了4个维度我select-的时候会有几个维度字段)
* [7.其他olap工具有了解过吗？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cboard_Kylin.md#7其他olap工具有了解过吗)
* [8.kylin你一般怎么调优](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cboard_Kylin.md#8kylin你一般怎么调优)
* [9.kylin的原理和优化？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cboard_Kylin.md#9kylin的原理和优化)
* [10.为什么kylin的维度不建议过多？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cboard_Kylin.md#10为什么kylin的维度不建议过多)
* [11.Kylin cube的构建过程是怎么样的？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cboard_Kylin.md#11kylin-cube的构建过程是怎么样的)
* [12.Kylin的构建算法](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cboard_Kylin.md#12kylin的构建算法)
* [13.cube优化？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cboard_Kylin.md#13cube优化)
* [14.什么叫全量构建？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cboard_Kylin.md#14什么叫全量构建)
* [15.怎么样实现自动增量构建?](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cboard_Kylin.md#15怎么样实现自动增量构建)
* [16.怎样实现在自己的web系统中查询kylin 的数据？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cboard_Kylin.md#16怎样实现在自己的web系统中查询kylin-的数据)
* [参考链接](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Cboard_Kylin.md#参考链接)


## Flink

- [1.简单介绍一下 Flink](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#1简单介绍一下-flink)

* [2.Flink 相比传统的 Spark Streaming 有什么区别?](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#2flink-相比传统的-spark-streaming-有什么区别)
* [3.Flink 的运行必须依赖 Hadoop组件吗？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#3flink-的运行必须依赖-hadoop组件吗)
* [4.Flink集群有哪些角色？各自有什么作用？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#4flink集群有哪些角色各自有什么作用)
* [5.说说 Flink 资源管理中 Task Slot 的概念](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#5说说-flink-资源管理中-task-slot-的概念)
* [6.说说 Flink 的常用算子？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#6说说-flink-的常用算子)
* [7.说说你知道的Flink分区策略？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#7说说你知道的flink分区策略)
* [8.Flink的并行度了解吗？Flink的并行度设置是怎样的？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#8flink的并行度了解吗flink的并行度设置是怎样的)
* [9.Flink的Slot和parallelism有什么区别？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#9flink的slot和parallelism有什么区别)
* [10.Flink有没有重启策略？说说有哪几种？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#10flink有没有重启策略说说有哪几种)
* [11.用过Flink中的分布式缓存吗？如何使用？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#11用过flink中的分布式缓存吗如何使用)
* [12.说说Flink中的广播变量，使用时需要注意什么？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#12说说flink中的广播变量使用时需要注意什么)
* [13.说说Flink中的窗口？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#13说说flink中的窗口)
* [14.说说Flink中的状态存储？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#14说说flink中的状态存储)
* [15.Flink 中的时间有哪几类?](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#15flink-中的时间有哪几类)
* [16.Flink 中水印是什么概念，起到什么作用？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#16flink-中水印是什么概念起到什么作用)
* [17.Flink Table &amp; SQL 熟悉吗？TableEnvironment这个类有什么作用?](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#17flink-table--sql-熟悉吗tableenvironment这个类有什么作用)
* [18.Flink SQL的实现原理是什么？是如何实现 SQL 解析的呢？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#18flink-sql的实现原理是什么是如何实现-sql-解析的呢)
* [19.Flink是如何做到高效的数据交换的？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#19flink是如何做到高效的数据交换的)
* [20.Flink是如何做容错的？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#20flink是如何做容错的)
* [21.Flink 分布式快照的原理是什么？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#21flink-分布式快照的原理是什么)
* [22.Flink 是如何保证Exactly-once语义的？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#22flink-是如何保证exactly-once语义的)
* [23.Flink 的 kafka 连接器有什么特别的地方？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#23flink-的-kafka-连接器有什么特别的地方)
* [24.说说 Flink的内存管理是如何做的?](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#24说说-flink的内存管理是如何做的)
* [25.说说 Flink的序列化如何做的?](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#25说说-flink的序列化如何做的)
* [26.Flink中的Window出现了数据倾斜，你有什么解决办法？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#26flink中的window出现了数据倾斜你有什么解决办法)
* [27.Flink中在使用聚合函数 GroupBy、Distinct、KeyBy 等函数时出现数据热点该如何解决？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#27flink中在使用聚合函数-groupbydistinctkeyby-等函数时出现数据热点该如何解决)
* [28.Flink任务延迟高，想解决这个问题，你会如何入手？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#28flink任务延迟高想解决这个问题你会如何入手)
* [29.Flink是如何处理反压的？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#29flink是如何处理反压的)
* [30.Flink的反压和Strom有哪些不同？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#30flink的反压和strom有哪些不同)
* [31.Operator Chains（算子链）这个概念你了解吗？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#31operator-chains算子链这个概念你了解吗)
* [32.Flink什么情况下才会把Operator chain在一起形成算子链？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#32flink什么情况下才会把operator-chain在一起形成算子链)
* [33.消费kafka数据的时候，如何处理脏数据？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#33消费kafka数据的时候如何处理脏数据)
* [参考资料](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flink.md#参考资料)


## Flume

- [1、什么是 Flume？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flume.md#1什么是-flume)

* [2、Flume 特点？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flume.md#2flume-特点)
* [3、flume 组成，Put 事物，Task 事务？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flume.md#3flume-组成put-事物task-事务)
* [4、Flume 拦截器？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flume.md#4flume-拦截器)
* [5.flume 和 kafka 采集日志区别，采集日志时中间停了，怎么记录之前的日志？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flume.md#5flume-和-kafka-采集日志区别采集日志时中间停了怎么记录之前的日志)
* [6、Flume 采集数据会丢失吗？(防止丢失机制)](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flume.md#6flume-采集数据会丢失吗防止丢失机制)
* [7、Flume 内存？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flume.md#7flume-内存)
* [8、FlumeChannel 优化？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flume.md#8flumechannel-优化)
* [9.Flume数据传输的监控的](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flume.md#9flume数据传输的监控的)
* [10.描述Flume拦截器开发过程中的核心方法有哪几个以及各自作用是什么？拦截器带来的优缺点各是什么](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flume.md#10描述flume拦截器开发过程中的核心方法有哪几个以及各自作用是什么拦截器带来的优缺点各是什么)
* [11、flume 管道内存，flume 宕机了数据丢失怎么解决？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flume.md#11flume-管道内存flume-宕机了数据丢失怎么解决)
* [12、flume 和 kafka 采集日志区别，采集日志时中间停了，怎么记录之前的日志？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flume.md#12flume-和-kafka-采集日志区别采集日志时中间停了怎么记录之前的日志)
* [13、flume 有哪些组件，flume 的 source、channel、sink 具体是做什么的？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flume.md#13flume-有哪些组件flume-的-sourcechannelsink-具体是做什么的)
* [14.Channel Selector中的replicating和multiplexxing各是什么含义](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flume.md#14channel-selector中的replicating和multiplexxing各是什么含义)
* [15.自定义开发实现TailDirSource支持递归文件夹数据的实时收集](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flume.md#15自定义开发实现taildirsource支持递归文件夹数据的实时收集)
* [16. Flume 的 Channel](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flume.md#16-flume-的-channel)
* [17.了解 Flume 的负载均衡和故障转移吗](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flume.md#17了解-flume-的负载均衡和故障转移吗)
* [18.Flume参数调优](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flume.md#18flume参数调优)
* [19.Flume的事务机制](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flume.md#19flume的事务机制)
* [20.Flume Event 是数据流的基本单元](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flume.md#20flume-event-是数据流的基本单元)
* [参考链接](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Flume.md#参考链接)

## HBase

* [1.Hbase是什么？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/HBase.md#1hbase是什么)
* [2.HBase 的特点是什么？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/HBase.md#2hbase-的特点是什么)
* [3.HBase 和 Hive 的区别？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/HBase.md#3hbase-和-hive-的区别)
* [4.HBase 适用于怎样的情景？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/HBase.md#4hbase-适用于怎样的情景)
* [5.描述 HBase 的 rowKey 的设计原则？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/HBase.md#5描述-hbase-的-rowkey-的设计原则)
* [6.描述 HBase 中 scan 和 get 的功能以及实现的异同？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/HBase.md#6描述-hbase-中-scan-和-get-的功能以及实现的异同)
* [7.请详细描述 HBase 中一个 cell 的结构？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/HBase.md#7请详细描述-hbase-中一个-cell-的结构)
* [8.简述 HBase 中 compact 用途是什么，什么时候触发，分为哪两种，有什么区别，有哪些相关配置参数。](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/HBase.md#8简述-hbase-中-compact-用途是什么什么时候触发分为哪两种有什么区别有哪些相关配置参数)
* [9.HBase 如何优化？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/HBase.md#9hbase-如何优化)
* [10.Region 如何预建分区？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/HBase.md#10region-如何预建分区)
* [11.HRegionServer 宕机如何处理？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/HBase.md#11hregionserver-宕机如何处理)
* [12.HBase 读写流程？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/HBase.md#12hbase-读写流程)
* [13.HBase 内部机制是什么？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/HBase.md#13hbase-内部机制是什么)
* [14.HBase 在进行模型设计时重点在什么地方？一张表中定义多少个 Column Family 最合适？为什么？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/HBase.md#14hbase-在进行模型设计时重点在什么地方一张表中定义多少个-column-family-最合适为什么)
* [15.如何提高 HBase 客户端的读写性能？请举例说明。](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/HBase.md#15如何提高-hbase-客户端的读写性能请举例说明)
* [16.直接将时间戳作为行健，在写入单个 region 时候会发生热点问题，为什么呢？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/HBase.md#16直接将时间戳作为行健在写入单个-region-时候会发生热点问题为什么呢)
* [17.请描述如何解决 HBase 中 region 太小和 region 太大带来的冲突？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/HBase.md#17请描述如何解决-hbase-中-region-太小和-region-太大带来的冲突)
* [参考链接](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/HBase.md#参考链接)


## Hadoop

- [1、简要描述如何安装配置一个 apache 开源版 hadoop，描述即可，列出步骤更好](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hadoop.md#1简要描述如何安装配置一个-apache-开源版-hadoop描述即可列出步骤更好)

* [2、请列出正常工作的 hadoop 集群中 hadoop 都需要启动哪些进程，他们的作用分别是什么？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hadoop.md#2请列出正常工作的-hadoop-集群中-hadoop-都需要启动哪些进程他们的作用分别是什么)
* [3、启动 hadoop 报如下错误，该如何解决？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hadoop.md#3启动-hadoop-报如下错误该如何解决)
* [4、请列出你所知道的 hadoop 调度器，并简要说明其工作方法？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hadoop.md#4请列出你所知道的-hadoop-调度器并简要说明其工作方法)
* [5、当前日志采样格式为如下，请编写 MapReduce 计算第四列每个元素出现的个数](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hadoop.md#5当前日志采样格式为如下请编写-mapreduce-计算第四列每个元素出现的个数)
* [6、hive 有哪些方式保存元数据，各有哪些特点？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hadoop.md#6hive-有哪些方式保存元数据各有哪些特点)
* [7、请简述 hadoop 怎么样实现二级排序？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hadoop.md#7请简述-hadoop-怎么样实现二级排序)
* [8、用非递归方法实现二分查找](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hadoop.md#8用非递归方法实现二分查找)
* [9、请简述 mapreduce 中，combiner，partition 作用？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hadoop.md#9请简述-mapreduce-中combinerpartition-作用)
* [10、HDFS 数据写入实现机制](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hadoop.md#10hdfs-数据写入实现机制)
* [11、hadoop 节点的动态上线下线的大概操作](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hadoop.md#11hadoop-节点的动态上线下线的大概操作)
* [12.MapTask 并行机制是由什么决定的？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hadoop.md#12maptask-并行机制是由什么决定的)
* [13.MR 是干什么的？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hadoop.md#13mr-是干什么的)
* [14.combiner 和 partition 的作用：](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hadoop.md#14combiner-和-partition-的作用)
* [15.什么是 shuffle](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hadoop.md#15什么是-shuffle)
* [16.列举几个 hadoop 生态圈的组件并做简要描述](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hadoop.md#16列举几个-hadoop-生态圈的组件并做简要描述)
* [17.NameNode 的 Safemode 是怎么回事? 如何才能退出 safemode？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hadoop.md#17namenode-的-safemode-是怎么回事-如何才能退出-safemode)
* [18.SecondaryNameNode 的主要职责是什么？简述其工作机制](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hadoop.md#18secondarynamenode-的主要职责是什么简述其工作机制)
* [19.一个 datanode 宕机，怎么恢复，简单说一下恢复流程？(运维)](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hadoop.md#19一个-datanode-宕机怎么恢复简单说一下恢复流程运维)
* [20.hadoop 的 namenode 宕机，怎么解决？（运维）](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hadoop.md#20hadoop-的-namenode-宕机怎么解决运维)
* [21.简述 hadoop 安装？（运维）](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hadoop.md#21简述-hadoop-安装运维)
* [22.Hadoop 中需要哪些配置文件，其作用是什么？（运维）](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hadoop.md#22hadoop-中需要哪些配置文件其作用是什么运维)
* [23. 请列出 hadoop 正常工作时要启动哪些进程，并写出各自的作用](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hadoop.md#23-请列出-hadoop-正常工作时要启动哪些进程并写出各自的作用)
* [参考链接](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hadoop.md#参考链接)



## Hive
* [1.Hive与传统数据库的区别](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hive.md#1hive与传统数据库的区别)
* [2.Hive内部表和外部表的区别](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hive.md#2hive内部表和外部表的区别)
* [3.Hive中order by，sort by，distribute by和cluster by的区别](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hive.md#3hive中order-bysort-bydistribute-by和cluster-by的区别)
* [4.row_number()，rank()和dense_rank()的区别](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hive.md#4row_numberrank和dense_rank的区别)
* [5.Hive中常用的系统函数有哪些](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hive.md#5hive中常用的系统函数有哪些)
* [6.Hive如何实现分区](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hive.md#6hive如何实现分区)
* [7.Hive导入数据的五种方式](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hive.md#7hive导入数据的五种方式)
* [8.Hive导出数据的五种方式](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hive.md#8hive导出数据的五种方式)
* [9.Hive 表关联查询，如何解决数据倾斜的问题？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hive.md#9hive-表关联查询如何解决数据倾斜的问题)
* [10.写出hive 中split、coalesce 及collect_list 函数的用法（可举例）？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hive.md#10写出hive-中splitcoalesce-及collect_list-函数的用法可举例)
* [11.Hive 有哪些方式保存元数据，各有哪些特点？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hive.md#11hive-有哪些方式保存元数据各有哪些特点)
* [12.Hive 的HSQL 转换为MapReduce 的过程？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hive.md#12hive-的hsql-转换为mapreduce-的过程)
* [13.Hive join 过程中大表小表的放置顺序？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hive.md#13hive-join-过程中大表小表的放置顺序)
* [14.Hive 的两张表关联，使用MapReduce 怎么实现？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hive.md#14hive-的两张表关联使用mapreduce-怎么实现)
* [15.所有的Hive 任务都会有MapReduce 的执行吗？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hive.md#15所有的hive-任务都会有mapreduce-的执行吗)
* [16.Hive 的函数：UDF、UDAF、UDTF 的区别？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hive.md#16hive-的函数udfudafudtf-的区别)
* [17.说说对Hive 桶表的理解？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hive.md#17说说对hive-桶表的理解)
* [18.Hive 自定义UDF 函数的流程?](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hive.md#18hive-自定义udf-函数的流程)
* [19.说下Hive的基本架构](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hive.md#19说下hive的基本架构)
* [20.hive分区和分桶的区别](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hive.md#20hive分区和分桶的区别)
* [21.hive的执行流程](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hive.md#21hive的执行流程)
* [参考资料](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Hive.md#参考资料)


## Impala

* [1.Impala是什么](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Impala.md#1impala是什么)
* [2.Impala特点](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Impala.md#2impala特点)
* [3.Impala的缺点](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Impala.md#3impala的缺点)
* [4.说下Impala的核心组件](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Impala.md#4说下impala的核心组件)
* [5.Impala的整体架构流程了解吗？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Impala.md#5impala的整体架构流程了解吗)
* [6.Impala与hive的异同了解吗](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Impala.md#6impala与hive的异同了解吗)
* [参考资料](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Impala.md#参考资料)


## Oozie

* [1.oozie 是什么](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Oozie.md#1oozie-是什么)
* [2.三个主要概念](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Oozie.md#2三个主要概念)
* [3.Workflow](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Oozie.md#3workflow)
* [4.Coordinator](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Oozie.md#4coordinator)
* [5.Bundle](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Oozie.md#5bundle)
* [6.oozie各个组件之间的关系](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Oozie.md#6oozie各个组件之间的关系)
* [7.节点类型](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Oozie.md#7节点类型)
* [8.  流程控制节点](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Oozie.md#8--流程控制节点)
* [9.动作节点](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Oozie.md#9动作节点)
* [10.Oozie Cli命令 启动任务](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Oozie.md#10oozie-cli命令-启动任务)
* [12.Oozie Cli命令 停止任务](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Oozie.md#12oozie-cli命令-停止任务)
* [13.Oozie Cli命令 提交任务](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Oozie.md#13oozie-cli命令-提交任务)
* [14.Oozie Cli命令 开始任务](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Oozie.md#14oozie-cli命令-开始任务)
* [15.Oozie Cli命令 查看任务执行情况](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Oozie.md#15oozie-cli命令-查看任务执行情况)
* [参考链接](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Oozie.md#参考链接)


## Presto

* [1.什么是presto](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Presto.md#1什么是presto)
* [2.presto优势](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Presto.md#2presto优势)
* [3.presto查询速度规模](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Presto.md#3presto查询速度规模)
* [4.presto数据模型](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Presto.md#4presto数据模型)
* [5.presto架构](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Presto.md#5presto架构)
* [6.presto 接入方式](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Presto.md#6presto-接入方式)
* [7.preto缺点](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Presto.md#7preto缺点)
* [8.Coordinator](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Presto.md#8coordinator)
* [9.Worker](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Presto.md#9worker)
* [10.Connector](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Presto.md#10connector)
* [11.Catalog](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Presto.md#11catalog)
* [12.Schema](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Presto.md#12schema)
* [13.Table](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Presto.md#13table)
* [14.Statement](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Presto.md#14statement)
* [15.Query](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Presto.md#15query)
* [16.Stage](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Presto.md#16stage)
* [17.Task](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Presto.md#17task)
* [18.Split](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Presto.md#18split)
* [19.Driver](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Presto.md#19driver)
* [20. Operator](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Presto.md#20-operator)
* [21. Exchange](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Presto.md#21-exchange)
* [参考链接](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Presto.md#参考链接)


## Spark


* [1.spark有几种部署模式，每种模式的特点？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#1spark有几种部署模式每种模式的特点)
* [2.Spark技术栈有哪些组件，每个组件都有什么功能，适合什么应用场景？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#2spark技术栈有哪些组件每个组件都有什么功能适合什么应用场景)
* [3.spark有哪些组件](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#3spark有哪些组件)
* [4.spark工作机制](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#4spark工作机制)
* [5.Spark应用程序的执行过程](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#5spark应用程序的执行过程)
* [6.driver的功能是什么？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#6driver的功能是什么)
* [7.Spark中Worker的主要工作是什么？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#7spark中worker的主要工作是什么)
* [8.task有几种类型？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#8task有几种类型)
* [9.什么是shuffle，以及为什么需要shuffle？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#9什么是shuffle以及为什么需要shuffle)
* [10.Spark master HA 主从切换过程不会影响集群已有的作业运行，为什么？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#10spark-master-ha-主从切换过程不会影响集群已有的作业运行为什么)
* [11.Spark并行度怎么设置比较合适](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#11spark并行度怎么设置比较合适)
* [12.Spark程序执行，有时候默认为什么会产生很多task，怎么修改默认task执行个数？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#12spark程序执行有时候默认为什么会产生很多task怎么修改默认task执行个数)
* [13.Spark中数据的位置是被谁管理的？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#13spark中数据的位置是被谁管理的)
* [14.为什么要进行序列化](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#14为什么要进行序列化)
* [15.Spark如何处理不能被序列化的对象？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#15spark如何处理不能被序列化的对象)
* [16.Spark提交你的jar包时所用的命令是什么？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#16spark提交你的jar包时所用的命令是什么)
* [17.Mapreduce和Spark的相同和区别](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#17mapreduce和spark的相同和区别)
* [18.简单说一下hadoop和spark的shuffle相同和差异？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#18简单说一下hadoop和spark的shuffle相同和差异)
* [19. 简单说一下hadoop和spark的shuffle过程](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#19-简单说一下hadoop和spark的shuffle过程)
* [20.partition和block的关联](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#20partition和block的关联)
* [21.Spark为什么比mapreduce快？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#21spark为什么比mapreduce快)
* [22.Mapreduce操作的mapper和reducer阶段相当于spark中的哪几个算子？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#22mapreduce操作的mapper和reducer阶段相当于spark中的哪几个算子)
* [23.RDD机制](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#23rdd机制)
* [24.RDD的弹性表现在哪几点？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#24rdd的弹性表现在哪几点)
* [25.RDD有哪些缺陷？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#25rdd有哪些缺陷)
* [26.什么是RDD宽依赖和窄依赖？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#26什么是rdd宽依赖和窄依赖)
* [27.rdd有几种操作类型？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#27rdd有几种操作类型)
* [28.Spark累加器有哪些特点？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#28spark累加器有哪些特点)
* [29.spark hashParitioner的弊端](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#29spark-hashparitioner的弊端)
* [30.RangePartitioner分区的原理](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#30rangepartitioner分区的原理)
* [参考资料](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Spark.md#参考资料)


## Sqoop


* [1.Sqoop 在工作中的定位是会用就行](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Sqoop.md#1sqoop-在工作中的定位是会用就行)
* [2.Sqoop导入hive时的参数](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Sqoop.md#2sqoop导入hive时的参数)
* [3.Rdbms中的增量数据如何导入？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Sqoop.md#3rdbms中的增量数据如何导入)
* [4.Sqoop导入导出Null存储一致性问题](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Sqoop.md#4sqoop导入导出null存储一致性问题)
* [5.Sqoop数据导出一致性问题](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Sqoop.md#5sqoop数据导出一致性问题)
* [6.Sqoop底层运行的任务是什么](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Sqoop.md#6sqoop底层运行的任务是什么)
* [7.Map task并行度设置大于1的问题](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Sqoop.md#7map-task并行度设置大于1的问题)
* [8.Sqoop数据导出的时候一次执行多长时间](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Sqoop.md#8sqoop数据导出的时候一次执行多长时间)
* [9.sqoop 导入数据到HDFS注意事项](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Sqoop.md#9sqoop-导入数据到hdfs注意事项)
* [10.Sqoop1和sqoop2优缺点：](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Sqoop.md#10sqoop1和sqoop2优缺点)
* [参考链接](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Sqoop.md#参考链接)


## Storm


* [1.什么是 storm？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Storm.md#1-什么是-storm)
* [2.提高并发度](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Storm.md#2提高并发度)
* [3.当 Nimbus 或 Supervisor 守护进程死亡时会发生什么？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Storm.md#3当-nimbus-或-supervisor-守护进程死亡时会发生什么)
* [4.Nimbus 是单点故障吗？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Storm.md#4nimbus-是单点故障吗)
* [5.Storm 如何保证数据处理？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Storm.md#5storm-如何保证数据处理)
* [6.storm 的可靠性如何实现，包括 spout 和 bolt 两部分？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Storm.md#6storm-的可靠性如何实现包括-spout-和-bolt-两部分)
* [7.storm 分组策略方式？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Storm.md#7storm-分组策略方式)
* [8.Storm 的物理架构？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Storm.md#8storm-的物理架构)
* [9.Storm 实时低延迟的原因](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Storm.md#9storm-实时低延迟的原因)
* [10.离线计算是什么？流式计算是什么？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Storm.md#10离线计算是什么流式计算是什么)
* [11.Storm 与 Hadoop 的区别](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Storm.md#11storm-与-hadoop-的区别)
* [12.Storm 核心组件](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Storm.md#12storm-核心组件)
* [13.当一个 worker 挂掉时会发生什么?](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Storm.md#13当一个-worker-挂掉时会发生什么)
* [14.当一个 node（节点）挂掉时会发生什么?](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Storm.md#14-当一个-node节点挂掉时会发生什么)
* [15.当 Nimbus 或 Supervisor 守护进程死亡时会发生什么？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Storm.md#15当-nimbus-或-supervisor-守护进程死亡时会发生什么)
* [16.流的模式是什么？默认是什么？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Storm.md#16流的模式是什么默认是什么)
* [17.Storm Group 分类](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Storm.md#17storm-group-分类)
* [18.Storm 的特点和特性是什么？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Storm.md#18storm-的特点和特性是什么)
* [19.storm 编程模型？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Storm.md#19storm-编程模型)
* [20.Spark Streaming 和 Storm 有什么不同？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Storm.md#20spark-streaming-和-storm-有什么不同)
* [参考链接](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Storm.md#参考链接)


## Yarn

* [1.简单介绍 yarn？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Yarn.md#1简单介绍-yarn)
* [2.Yarn 有什么特点？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Yarn.md#2yarn-有什么特点)
* [3.为什么要使用 Yarn。](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Yarn.md#3为什么要使用-yarn)
* [4.yarn 主要作用](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Yarn.md#4yarn-主要作用)
* [5.yarn 的结构](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Yarn.md#5yarn-的结构)
* [6.Yarn 在运行过程中负责给应用分配资源的是什么](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Yarn.md#6yarn-在运行过程中负责给应用分配资源的是什么)
* [7.yarn 的工作流程](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Yarn.md#7yarn-的工作流程)
* [8.yarn 的调度器](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Yarn.md#8yarn-的调度器)
* [9.YARN 高可用](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Yarn.md#9yarn-高可用)
* [10.什么是 container？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Yarn.md#10什么是-container)
* [11.Yarn支持的调度器和硬件资源种类？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Yarn.md#11yarn支持的调度器和硬件资源种类)
* [12.请问RM节点上有Container容器的这种说法吗？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Yarn.md#12请问rm节点上有container容器的这种说法吗)
* [13.在AM中，job已经被分成一系列的task，并且是为每个task来startContainer。为什么NM上要存一个application的数据结构呢？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Yarn.md#13在am中job已经被分成一系列的task并且是为每个task来startcontainer为什么nm上要存一个application的数据结构呢)
* [14.是否只有负责启动ApplicationMaster的NodeManager才会维护一个Application对象？其他的NodeManager是否是根据ApplicationMaster发起的请求来启动属于这个Application的其他Container，这些NodeManager不需要维护Application的状态机？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Yarn.md#14是否只有负责启动applicationmaster的nodemanager才会维护一个application对象其他的nodemanager是否是根据applicationmaster发起的请求来启动属于这个application的其他container这些nodemanager不需要维护application的状态机)
* [15.Container的节点随机性？](https://github.com/BigDataInterviewHub/BigDataInterview/blob/main/Yarn.md#15container的节点随机性)
