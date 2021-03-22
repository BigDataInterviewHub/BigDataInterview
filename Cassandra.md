## Cassandra

* [1.向Cassandra讲解](#1向cassandra讲解)
* [2.Cassandra用哪种语言写？](#2cassandra用哪种语言写)
* [3.Cassandra(Cassandra)的原始作者是谁？](#3cassandracassandra的原始作者是谁)
* [4.Cassandra数据库中使用哪种查询语言？](#4cassandra数据库中使用哪种查询语言)
* [5.Cassandra的优点/优点是什么？](#5cassandra的优点优点是什么)
* [6.是否提到了Cassandra数据模型的一些重要组成部分？](#6是否提到了cassandra数据模型的一些重要组成部分)
* [5.数据模型（Data Model）](#5数据模型data-model)
* [6.列（Colunmn）](#6列colunmn)
* [7.列族（Column Family）](#7列族column-family)
* [8.超列族（Super Column Family）](#8超列族super-column-family)
* [9.KeySpaces](#9keyspaces)
* [10.Clusters](#10clusters)
* [11.Cassandra的其他成分是什么？](#11cassandra的其他成分是什么)
* [12.Cassandra中有哪些不同的组合键？](#12cassandra中有哪些不同的组合键)
* [13.什么是Cassandra中的数据复制？](#13什么是cassandra中的数据复制)
* [14.Cassandra中的数据中心是什么意思？](#14cassandra中的数据中心是什么意思)
* [15.cassandra用了哪些端口？](#15cassandra用了哪些端口)
* [16.是不是单个seed意味着单点故障？](#16是不是单个seed意味着单点故障)
* [17.为什么不可以在jconsole里调用某个jmx方法呢？](#17为什么不可以在jconsole里调用某个jmx方法呢)
* [18.为什么我会在日志文件里看到 “… messages dropped …”这样的信息？](#18为什么我会在日志文件里看到--messages-dropped-这样的信息)
* [19.Cassandra因为java.lang.OutOfMemoryError: Map failed挂掉了](#19cassandra因为javalangoutofmemoryerror-map-failed挂掉了)
* [20.如果再同一时刻发生两次更新会发生什么？](#20如果再同一时刻发生两次更新会发生什么)
* [21.为什么在加入一个新节点的时候，会有Stream failed错误？](#21为什么在加入一个新节点的时候会有stream-failed错误)
* [参考链接](#参考链接)





#### 1.向Cassandra讲解

Cassandra是一种流行的NOSQL数据库管理系统, 用于处理大量数据。它是免费的开源分布式数据库, 可提供高可用性而不会发生任何故障。

#### 2.Cassandra用哪种语言写？

Cassandra用Java编写。它最初由Facebook设计, 由灵活的架构组成。它对于大数据具有高度可扩展性。

#### 3.Cassandra(Cassandra)的原始作者是谁？

Cassandra的原始作者是Avinash Lakshman和Prashant Malik。它最初是在Facebook上开发的, 用于支持Facebook收件箱搜索功能。

#### 4.Cassandra数据库中使用哪种查询语言？

Cassandra引入了自己的Cassandra查询语言(CQL)。 CQL是访问Cassandra的简单接口, 是传统的结构化查询语言(SQL)的替代方法。

#### 5.Cassandra的优点/优点是什么？

- Cassandra提供实时性能, 简化了开发人员, 管理员, 数据分析师和软件工程师的工作。
- 它提供了可扩展的可伸缩性, 并且可以根据要求轻松地按比例放大和缩小。
- 数据可以复制到多个节点以实现容错。
- 作为分布式管理系统, 没有单点故障。
- 集群中的每个节点都包含不同的数据, 并且能够满足任何请求。

#### 6.是否提到了Cassandra数据模型的一些重要组成部分？

这些是Cassandra数据模型的一些关键组成部分：-

- 表(列集合)
- 节点
- 簇
- 键空间

#### 5.数据模型（Data Model）

在关系型数据库中的一些常见术语在Cassandra中则有不同的定义。如果你能暂时忘记常规的定义，则阅读本文可能会更加顺利。

#### 6.列（Colunmn）

“列”由一个name，一个相关的value和一个时间戳组成。name和value可以是任何类型，name不需要是字符串。一个列就是一个Name-Value-Timestamp集合。

#### 7.列族（Column Family）

可以想象成在关系数据库中一行所包含的一个key和一些列。

#### 8.超列族（Super Column Family）

就是一个列族，这个列族中每个列的值就是一个列的集合。

大家可能会有些疑惑，可以看看下面的图片：

![cassandra_overview_01](http://ifeve.com/wp-content/uploads/2017/01/cassandra_overview_01.png)

![cassandra_overview_02](http://ifeve.com/wp-content/uploads/2017/01/cassandra_overview_02.png)

对Column Family再多说两句。一个column family 可以被定义为一个有序rows的集合，每个row都包含了一个有序的columns的集合。Cassandra是一个自由的模式，之后你可以在任意时刻添加任意column到任意column family中。Columns不需要相同，并且每行都不需要有相同数量的columns。尽管这样，在相同的column family有相同的columns还是比较好的，因为每个column family都存储在一个单独的文件中。

一个column family 有一个name和一个比较器，比较器的的值决定了columns的排序。每行中的key被用来决定该行存储在集群中哪个节点中。



#### 9.KeySpaces

是数据最外层的容器。可以把它想象成RDBMS中的一个数据库。尽管不是必须的，但是为每个应用创建一个单独的keyspace比较好。一个keyspace有一个name和一些别的属性（比如说复制因子，复制策略等）。

#### 10.Clusters

Cassandra运行在多个节点（一个集群）之上，它在集群间复制数据，所以当有一个节点宕机，另外的节点能接管工作。这让Cassandra是高可用的。

简而言之，Cassandra的数据模型像下面这样：

Cluster => Keyspace => Column Families (Standard or Super) => Column => (Name, Value, Timestamp)

#### 11.Cassandra的其他成分是什么？

Cassandra的其他一些组件是：

- 节点
- 数据中心
- 提交日志
- 内存表
- SS表
- 布隆过滤器

#### 12.Cassandra中有哪些不同的组合键？

在Cassandra中, 复合键用于定义键或具有不同类型数据串联的列名。 Cassandra中有两种复合键：

- 行键
- 栏名

#### 13.什么是Cassandra中的数据复制？

数据复制是将数据从一台计算机或服务器中的数据库电子复制到另一台计算机或服务器中的数据库, 以便所有用户可以共享相同级别的信息。 Cassandra将副本存储在多个节点上, 以确保可靠性和容错能力。复制策略决定放置副本的节点。

#### 14.Cassandra中的数据中心是什么意思？

数据中心是集群的完整数据。

#### 15.cassandra用了哪些端口？

默认Cassandra使用7000作为集群通信端口（如果开启了SSL就是7001端口）。9042端口用于native协议的客户端连接。7199端口用于JMX，9160端口用于废弃的Thrift接口。内部节点通信以及native协议的端口在cassandra配置文件里可以配置。JMX端口可以在cassandra-env.sh配置（通过JVM的参数)。所有端口都是TCP的。

#### 16.是不是单个seed意味着单点故障？

即便没有seed节点，集群也可以运行和重启，但是不能再往集群里增加节点。还是推荐在生产系统中配置多个种子节点。

#### 17.为什么不可以在jconsole里调用某个jmx方法呢？

一些JMX操作的参数是个数组，而Jconsole并不支持数组型参数。对于那些不能用jconsole调用的操作（在jconsole里点击按钮无效）。有需要自己写一个JMX客户端去调用，或者使用一个支持数组的jmx监控工具。

#### 18.为什么我会在日志文件里看到 “… messages dropped …”这样的信息？

这是cassandra在面对超出自己处理能力的请求时，为了保护自己做出的流控措施。
一个节点接收到其它节点发送过来的消息，但是在他们合适的超时时间内不能得到处理（具体参考 read_request_timeout, write_request_timeout等配置项）。就会被丢弃，而不是处理（因为接受用户请求的节点也就是协调节点不会再等这个响应返回）

对于写，这意味着它的请求不会被写入所有的副本，这个一致性将会被读修复、hints或者是人工修复等方式修复。因为这，一个写操作返回给客户端的结果就是超时的。

对于读，意味着一个读请求可能没有完成。负载流控是cassandra架构的一部分，如果这个问题一直持续下去，这标志着你的节点或者集群已经超载了。

#### 19.Cassandra因为java.lang.OutOfMemoryError: Map failed挂掉了

如果cassandra挂掉的时候输出“Map failed”的消息，表示操作系统不允许java锁定更多的内存。在linux里，内存锁定是有限制的，你可以通过/proc//limits确认，并提高它（比如适应ulimit命令）。还有vm.max_map_cout参数。注意debian安装包会自动为你调整这些参数。

#### 20.如果再同一时刻发生两次更新会发生什么？

更新顺序必须是可交换的，因为他们很有可能到达不同副本的顺序是不一样的。只要cassandra有一个确定的方法选出这个赢家（相同的时间戳），那么这在其它节点也是一样的，这是一个重要的实现细节。也就是说，对于相同时间戳的操作，Cassandra遵循以下两个原则：第一：删除要优先于更新和插入，第二：如果两个都是更新，那个在语法上比较大的更新会被选中。

#### 21.为什么在加入一个新节点的时候，会有Stream failed错误？

两个可能性：
GC可能导致的长时间暂停可能会扰乱传输进程
在后台执行的压缩会导致传输时间太长从而TCP连接断开。

对于第一种情况，建议在应用中经常的进行GC优化，
第二种情况，你需要设置系统的TCP keepalive参数短一些（linux中默认是很长的），尝试执行下面的语句：

```
 sudo /sbin/sysctl -w net.ipv4.tcp_keepalive_time=60 net.ipv4.tcp_keepalive_intvl=60 net.ipv4.tcp_keepalive_probes=5

```



#### 参考链接

http://www.srcmini.com/33560.html

https://ifeve.com/cassandra-overview/

https://blog.csdn.net/zhuwinmin/article/details/76066642

https://blog.csdn.net/zhuwinmin/article/details/76066690