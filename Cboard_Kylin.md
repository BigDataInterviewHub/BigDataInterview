## CBoard & Kylin

* [1.CBoard](#1cboard)
* [2.CBoard特性](#2cboard特性)
* [3.Kylin的优点和缺点？](#3kylin的优点和缺点)
* [4.Kylin的rowkey如何设计？](#4kylin的rowkey如何设计)
* [5.Kylin的cuboid,cube和segment的关系？](#5kylin的cuboidcube和segment的关系)
* [6.一张hive宽表有5个维度，kylin构建cube的时候我选了4个维度，我select *的时候会有几个维度字段？](#6一张hive宽表有5个维度kylin构建cube的时候我选了4个维度我select-的时候会有几个维度字段)
* [7.其他olap工具有了解过吗？](#7其他olap工具有了解过吗)
* [8.kylin你一般怎么调优](#8kylin你一般怎么调优)
* [9.kylin的原理和优化？](#9kylin的原理和优化)
* [10.为什么kylin的维度不建议过多？](#10为什么kylin的维度不建议过多)
* [11.Kylin cube的构建过程是怎么样的？](#11kylin-cube的构建过程是怎么样的)
* [12.Kylin的构建算法](#12kylin的构建算法)
* [13.cube优化？](#13cube优化)
* [14.什么叫全量构建？](#14什么叫全量构建)
* [15.怎么样实现自动增量构建?](#15怎么样实现自动增量构建)
* [16.怎样实现在自己的web系统中查询kylin 的数据？](#16怎样实现在自己的web系统中查询kylin-的数据)
* [参考链接](#参考链接)

### 

#### 1.CBoard

CBoard由[上海楚果信息技术有限公司](http://www.chuguotech.com/)主导开源, 它不仅仅是一款自助BI数据分析产品, 还是开放的BI产品开发平台:

- 用户只需简单妥妥拽拽就能自助完成数据多维分析与报表设计
- 开发者能够简单扩展连接所有你的Java程序能够触及的数据

#### 2.CBoard特性

简洁美观的界面, 简单友好的交互模式
交互式自服务拖拽多维分析用户体验, 数据切块, 切片, 排序无所不能
一个数据集根据您的拖拽衍生无数不同粒度数据聚合 + 20余种不同展现形式的图表
图表数据准实时刷新
图表级别权限控制
支持多图表数据看板与看板定时邮件发送
多种数据源接入
JDBC(几乎所有实现了JDBC协议的数据库或数据产品都能轻松接入)
多版本原生Elasticsearch: 1.x, 2.x, 5.x
多版本原生Kylin接入: 1.6, 2.0, 2.1
离线文本文件, JSON文本
轻量级的技术架构, 简洁的业务代码, 不依赖任何第三方多维分析引擎, 如果您还在纠结很难玩转Mondrian, 那么CBoard绝对是您很好的一个替代方案
数据源轻松扩展接入, 大数据时代纷繁的数据产品层出不穷, 任何昂贵的商业产品也做不到出厂遍支持所有类型数据源的连接, 但是如果你能用Java程序获取您的数据, 那么恭喜你有80%的概率能够把数据源接到CBoard了

#### 3.Kylin的优点和缺点？

优点:预计算，界面可视化

缺点：依赖较多，属于重量级方案，运维成本很高

不适合做即席查询

预计算量大，非常消耗资源

#### 4.Kylin的rowkey如何设计？

Kylin rowkey的编码和压缩选择

维度在rowkey中顺序的调整，

将过滤频率较高的列放置在过滤频率较低的列之前，

将基数高的列放置在基数低的列之前。

在查询中被用作过滤条件的维度有可能放在其他维度的前面。

充分利用过滤条件来缩小在HBase中扫描的范围， 从而提高查询的效率。 


#### 5.Kylin的cuboid,cube和segment的关系？

Cube是所有cubiod的组合，一个cube包含一个或者多个cuboid

Cuboid 在 Kylin 中特指在某一种维度组合下所计算的数据。

Cube Segment 是指针对源数据中的某一片段，全量构建的cube只存在唯一的segment，该segment没有分割时间的概念，增量构建的cube，不同时间的数据分布在不同的segment中


#### 6.一张hive宽表有5个维度，kylin构建cube的时候我选了4个维度，我select \*的时候会有几个维度字段？

```
select * from wedw_dw.t_kylin_test_df
```

![图片](https://img-blog.csdnimg.cn/img_convert/3cbd5fe8f4fb5f0b99d4628f7ff5eb4f.png)

所以只能查询出4个字段

#### 7.其他olap工具有了解过吗？

了解过，kylin，druid

#### 8.kylin你一般怎么调优

Cube调优

l剪枝优化(衍生维度，聚合组，强制维度，层级维度，联合维度)

l并发粒度优化

lRowkeys优化(编码，按维度分片，调整维度顺序)

l降低度量精度

l及时清理无用的segment

 

Rowkey调优

lKylin rowkey的编码和压缩选择

l维度在rowkey中顺序的调整，

l将过滤频率较高的列放置在过滤频率较低的列之前，

l将基数高的列放置在基数低的列之前。

l在查询中被用作过滤条件的维度有可能放在其他维度的前面。

充分利用过滤条件来缩小在HBase中扫描的范围， 从而提高查询的效率。 


#### 9.kylin的原理和优化？

原理：预计算

优化同上

 

#### 10.为什么kylin的维度不建议过多？

Cube 的最大物理维度数量 (不包括衍生维度) 是 63，但是不推荐使用大于 30 个维度的 Cube，会引起维度灾难。

#### 11.Kylin cube的构建过程是怎么样的？

- 衍生维度
- 聚合组
- 强制维度
- 层级维度
- 联合维度

#### 12.Kylin的构建算法

快速构建算法（inmem）

也被称作“逐段”(By Segment) 或“逐块”(By Split) 算法，从1.5.x开始引入该算法，利用Mapper端计算先完成大部分聚合，再将聚合后的结果交给Reducer，从而降低对网络瓶颈的压力。该算法的主要思想是，对Mapper所分配的数据块，将它计算成一个完整的小Cube 段（包含所有Cuboid）；每个Mapper将计算完的Cube段输出给Reducer做合并，生成大Cube，也就是最终结果；如图所示解释了此流程。
![图片](https://img-blog.csdnimg.cn/img_convert/ac0796ac1e4b33714ce890b3d814ef9e.png)

与旧算法相比，快速算法主要有两点不同：

Mapper会利用内存做预聚合，算出所有组合；Mapper输出的每个Key都是不同的，这样会减少输出到Hadoop MapReduce的数据量，Combiner也不再需要；

一轮MapReduce便会完成所有层次的计算，减少Hadoop任务的调配。




#### 13.cube优化？

- 剪枝优化(衍生维度，聚合组，强制维度，层级维度，联合维度)
- 并发粒度优化
- Rowkeys优化(编码，按维度分片，调整维度顺序)
- 降低度量精度
- 及时清理无用的segment

#### 14.什么叫全量构建？

针对数据源所有数据进行cube计算，就是全量构建

什么叫做增量构建？什么叫做segment？
针对数据源表的一个分区或者某一段进行cube计算，就是增量构建，
对于此次产生构建的结果就是segment

#### 15.怎么样实现自动增量构建?

通过curl 命令调用 kylin的 restapi接口实现构建增量表，然后将命令编写成shell脚本，放入azkaban中调度，就可以自动构建增量表。

#### 16.怎样实现在自己的web系统中查询kylin 的数据？

通过jdbc连接kylin 使用sql查询

#### 参考链接

https://blog.csdn.net/py_123456/article/details/83501512

https://blog.csdn.net/young_0609/article/details/112367511



https://blog.csdn.net/qq_39660507/article/details/109437200