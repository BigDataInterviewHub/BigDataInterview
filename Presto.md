## Presto

* [1.什么是presto](#1什么是presto)
* [2.presto优势](#2presto优势)
* [3.presto查询速度规模](#3presto查询速度规模)
* [4.presto数据模型](#4presto数据模型)
* [5.presto架构](#5presto架构)
* [6.presto 接入方式](#6presto-接入方式)
* [7.preto缺点](#7preto缺点)
* [8.Coordinator](#8coordinator)
* [9.Worker](#9worker)
* [10.Connector](#10connector)
* [11.Catalog](#11catalog)
* [12.Schema](#12schema)
* [13.Table](#13table)
* [14.Statement](#14statement)
* [15.Query](#15query)
* [16.Stage](#16stage)
* [17.Task](#17task)
* [18.Split](#18split)
* [19.Driver](#19driver)
* [20. Operator](#20-operator)
* [21. Exchange](#21-exchange)
* [参考链接](#参考链接)

#### 1.什么是presto
presto是一个开源的分布式的查询引擎，基于内存，它本身不接入数据，可以连接多种数据源，例如 Hive ,Mysql,Kafka,MongeDB等，一条Presto查询可以将多个数据源进行合并查询。
preto适合OLAP,而非OLTP,所以不要将preto当成数据库来使用。

#### 2.presto优势
低延迟，高并发，纯内存计算引擎，查询效率是hive的数十倍

#### 3.presto查询速度规模
数G到数P规模

#### 4.presto数据模型
分为Catalog,Schema,Table三层

Catalog: 数据源，例如是Hive，还是Mysql等等
Schema: 库
Table: 表

#### 5.presto架构
preso是一个Master-Slave架构，由一个Coordinator节点，一个Disovery Server节点及多个Worker节点组成
Coordinator：负责query解析和分发，work管理，meta管理
Disovery Server: 节点心跳，默认内嵌于Coordinator中，于Coordinator共享一台机器
Worker:计算节点，收到分发的task任务后，就会去对应的数据源取数

简单流程

Worker节点启动后向Discovery Server服务注册，Coordinator从Disovery Server获得正常工作的Worker节点，

#### 6.presto 接入方式
使用Prosot的方式有多种: presto-cli,jdbc,http等等

以presto-cli为例接入hive数据源：

./presto-cli.jar --server presto.xxx.com:9200 --catalog hive --schema xxx 就可以进入presto终端界面

#### 7.preto缺点
容错能力差： 当一个worker节点挂掉或者其他原因导致该worker节点上的查询失败的时候，整个query也会失败
内存限制：目前版本presto基于纯内存计算，内存不够也会失败，不会dump到磁盘上
并行查询：因所有的task都是并行计算，一个很慢会导致整个都很慢
并发限制: 因全内存操作，会导致同时处理的任务有限

#### 8.Coordinator

Presto coordinator（协调器）是负责解析语句、查询计划和管理 Presto Worker 节点的服务进程。它是 Presto 安装的“大脑”，也是客户端连接以提交语句并提供执行的节点。 每个 Presto 安装必须有一个 Presto Coordinator 和一个或多个 Presto Worker。出于开发或测试目的，可以将单个 Presto 实例配置作为这两个角色。 Coordinator跟踪每个 Worker 的活动并协调查询的执行。Coordinator 创建一个涉及一系列阶段的查询的逻辑模型，然后将其转换为在 Presto 工作集群上运行的一系列连接任务。 Coordinator 使用 REST API 与 Worker 和客户进行通信。

#### 9.Worker

Presto worker 是 Presto 安装中的负责执行任务和处理数据服务进程。Worker 从Connector获取数据并相互交换中间数据。Coordinator 负责从Worker那里获取结果并将最终结果返回给客户端。当Presto Worker进程启动时，它会将自己通告给 Coordinator，接下来Presto Coordinator就可以执行任务。 Worker 使用 REST API 与其他 Worker 和 Presto Coordinator 进行通信。

#### 10.Connector

Connector 将 Presto 适配到数据源，如Hive或关系数据库。您可以将 Connector 视为与数据库驱动程序相同的方式，这是 Presto SPI 的一种实现，它允许 Presto 使用标准 API 与资源进行交互。 Presto 包含几个内置连接器：JMX 的连接器 ，它是一个提供对内置系统表访问的系统连接器；Hive连接器和一个用于提供 TPC-H 基准数据的TPCH连接器。 许多第三方开发人员都提供了连接器，以便 Presto 可以访问各种数据源中的数据。每个 Catalog 都与特定 Connector 相关联，如果检查 Catalog 配置文件，您将看到每个都包含强制属性 connector.name， Catalog 使用该属性为给定的 catalog 创建 Connector。可以让多个 Catalog 使用相同的 Connector 来访问类似数据库的两个不同实例。例如，如果您有两个Hive集群，则可以在单个 Presto 集群中配置两个 Catalog， 这两个 Catalog 都使用 Hive Connector，允许您查询来自两个 Hive 集群的数据（即使在同一SQL查询中）。

#### 11.Catalog

Presto Catalog 包含 Schemas，并通过 Connector 引用数据源。例如，您可以配置 JMX catalog 以通过 JMX Connector 来提供对 JMX 信息的访问。 在 Presto 中运行 SQL 语句时，您将针对一个或多个 Catalog 运行它。Catalog 的其它示例包括用于连接到 Hive 数据源的 Hive Catalog。 当在 Presto 中寻址一个表时，全限定的表名始终以 Catalog 为根。例如，一个全限定的表名称hive.test_data.test 将引用 Hive catalog中 test_data库中 test表。 Catalog的定义存储在 Presto 配置目录中的属性文件中。

#### 12.Schema

Schema 是组织表的一种方式。Schema 和 Catalog 一起定义了一组可以查询的表。使用 Presto 访问 Hive 或 MySQL 等关系数据库时，Schema 会转换为目标数据库中的相同概念。 其他类型的 Connector 可以选择以对底层数据源有意义的方式将表组织成 Schema。

#### 13.Table

表是一组被组织成具有类型的列名的无序行。这与任一关系数据库中的定义相同。从源数据到表的映射由 Connector 定义。

#### 14.Statement

Presto 执行与 ANSI 兼容的 SQL 语句。当 Presto 文档引用一个语句时，它指的是 ANSI SQL 标准中定义的语句，该语句(Statment)由子句(Clauses)、表达式(Expression)和断言(Predicate)组成。 Presto 为什么将语句和查询(Query)概念分开呢？这是必要的，因为在 Presto 中，Statment 只是引用 SQL 语句的文本表示。执行语句时，Presto 会创建一个查询（Query）以及一个查询计划，然后该查询计划将分布在一系列 Presto Worker 进程中。


#### 15.Query

当 Presto 解析语句时，它将转换为查询（Query）并创建一个分布式查询计划，然后将其实现为在 Presto Worker 进程上运行的一系列相互关联的阶段。在 Presto 中检索有关查询的信息时，您会收到生成结果集以响应语句所涉及的每个组件的快照。 语句和查询之间的区别很简单。语句可以被认为是传递给 Presto 的 SQL 文本，而查询是指为实现该语句而实例化的配置和组件。查询包含 stages、tasks、splits、connectors以及其它组件和数据源，它们协同工作以生成结果。

#### 16.Stage
当Presto 执行查询时，它通过将执行分解为具有层级关系的多个 Stage。例如，如果 Presto 需要从Hive 中存储的10亿行数据进行聚合，则可以通过创建根State来聚合其它几个State的输出，所有这些State都旨在实现分布式查询计划的不同部分。包含查询的层级关系结构类似于树，每个查询都有一个根Stage，负责聚合其它 Stage 的输出。State是 Coordinator 用于分布式查询计划建模的工具，但State本身并不在 Presto Worker 进程上运行。
#### 17.Task
如Stage部分所述，Stage 对分布式查询计划的特定部分进行建模，但 Stage 本身不在 Presto Worker 进程上执行。要了解 Stage 的执行方式，您需要了解 Stage 是作为一系列任务分散在Presto Worker 网络上实施的。 Task是 Presto 架构中的“役马”(work horse)，因为分布式查询计划被分解为一系列 Stage，然后将这些 Stage 转换为Task对其进行处理或者split。Presto Task具有输入和输出，正如一个 Stage 可以通过一系列Task并行执行一样，Task与一系列驱动程序并行执行。

#### 18.Split
Task对Split进行操作，Split是较大数据集的一部分。分布式查询计划的最低级别的 Stage 通过 Connector 的拆分检索数据，而分布式查询计划的更高级别的中间Stage从其它Stage检索数据。 当 Presto 计划查询时，Coordinator 将查询 Connector 以获取表中所有可用于Split的列表。Coordinator 跟踪哪些机器正在运行哪些Task以及哪些Task正在处理哪些Split。

#### 19.Driver
Task包含一个或多个并行 Driver。Driver 对数据进行操作并组合运算符以产生输出，然后将输出交由一个Task聚合，然后在另一个 Stage 中传递给另外一个Task。 Driver 是一系列操作符实例，或者您可以将 Driver 视为内存中的一组物理运算符。它是 Presto 架构中最低级别的并行度。Driver 具有一个输入和一个输出。

#### 20. Operator
Operator消费、转换和生成数据。例如，表扫描从 Connector 获取数据并生成可由其它Operator使用的数据，并且过滤器Operator通过在输入数据上应用Predicate 来消费数据并生成子集。

#### 21. Exchange
Presto 的Stage通过Exchange来连接另一个Stage，Exchange用于完成有上下游关系的Stage之间的数据交换。Task将数据产生到输出缓冲区中，并使用Exchange客户端消费其它Task的数据。

#### 参考链接

https://blog.csdn.net/zhou12314/article/details/102886851

https://blog.csdn.net/github_39577257/article/details/90349919