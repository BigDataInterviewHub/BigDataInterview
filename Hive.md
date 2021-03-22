* [1.Hive与传统数据库的区别](#1hive与传统数据库的区别)
* [2.Hive内部表和外部表的区别](#2hive内部表和外部表的区别)
* [3.Hive中order by，sort by，distribute by和cluster by的区别](#3hive中order-bysort-bydistribute-by和cluster-by的区别)
* [4.row_number()，rank()和dense_rank()的区别](#4row_numberrank和dense_rank的区别)
* [5.Hive中常用的系统函数有哪些](#5hive中常用的系统函数有哪些)
* [6.Hive如何实现分区](#6hive如何实现分区)
* [7.Hive导入数据的五种方式](#7hive导入数据的五种方式)
* [8.Hive导出数据的五种方式](#8hive导出数据的五种方式)
* [9.Hive 表关联查询，如何解决数据倾斜的问题？](#9hive-表关联查询如何解决数据倾斜的问题)
* [10.写出hive 中split、coalesce 及collect_list 函数的用法（可举例）？](#10写出hive-中splitcoalesce-及collect_list-函数的用法可举例)
* [11.Hive 有哪些方式保存元数据，各有哪些特点？](#11hive-有哪些方式保存元数据各有哪些特点)
* [12.Hive 的HSQL 转换为MapReduce 的过程？](#12hive-的hsql-转换为mapreduce-的过程)
* [13.Hive join 过程中大表小表的放置顺序？](#13hive-join-过程中大表小表的放置顺序)
* [14.Hive 的两张表关联，使用MapReduce 怎么实现？](#14hive-的两张表关联使用mapreduce-怎么实现)
* [15.所有的Hive 任务都会有MapReduce 的执行吗？](#15所有的hive-任务都会有mapreduce-的执行吗)
* [16.Hive 的函数：UDF、UDAF、UDTF 的区别？](#16hive-的函数udfudafudtf-的区别)
* [17.说说对Hive 桶表的理解？](#17说说对hive-桶表的理解)
* [18.Hive 自定义UDF 函数的流程?](#18hive-自定义udf-函数的流程)
* [19.说下Hive的基本架构](#19说下hive的基本架构)
* [20.hive分区和分桶的区别](#20hive分区和分桶的区别)
* [21.hive的执行流程](#21hive的执行流程)
* [参考资料](#参考资料)

## Hive

#### 1.Hive与传统数据库的区别

Hive和数据库除了拥有类型的查询语言外，无其他相似

存储位置：Hive数据存储在HDFS上。数据库保存在块设备或本地文件系统
数据更新：Hive不建议对数据改写。数据库通常需要经常修改
执行引擎：Hive通过MapReduce来实现。数据库用自己的执行引擎
执行速度：Hive执行延迟高，但它数据规模远超过数据库处理能力时，Hive的并行计算能力就体现优势了。数据库执行延迟较低
数据规模：hive大规模的数据计算。数据库能支持的数据规模较小
扩展性：Hive建立在Hadoop上，随Hadoop的扩展性。数据库由于ACID语义[wh1] 的严格限制，扩展有限

#### 2.Hive内部表和外部表的区别

存储：外部表数据由HDFS管理；内部表数据由hive自身管理
存储：外部表数据存储位置由自己指定（没有指定location则在默认地址下新建）；内部表数据存储在hive.metastore.warehouse.dir（默认在/uer/hive/warehouse）
创建：被external修饰的就是外部表；没被修饰是内部表
删除：删除外部表仅仅删除元数据；删除内部表会删除元数据和存储数据

#### 3.Hive中order by，sort by，distribute by和cluster by的区别

order by：对数据进行全局排序，只有一个reduce工作
sort by：每个mapreduce中进行排序，一般和distribute by使用，且distribute by写在sort by前面。当mapred.reduce.tasks=1时，效果和order by一样
distribute by：类似MR的Partition，对key进行分区，结合sort by实现分区排序
cluster by：当distribute by和sort by的字段相同时，可以使用cluster by代替，但cluster by只能是升序，不能指定排序规则

在生产环境中order by使用的少，容易造成内存溢出(OOM)
生产环境中distribute by和sort by用的多

#### 4.row_number()，rank()和dense_rank()的区别

都有对数据进行排序的功能

row_number()：根据查询结果的顺序计算排序，多用于分页查询

rank()：排序相同时序号重复，总序数不变

dense_rank()：排序相同时序号重复时，总序数减少

#### 5.Hive中常用的系统函数有哪些

date_add(str,n)、date_sub(str,n) 加减时间
next_day(to_date(str),’MO’) 周指标相关,获取str下周一日期
date_format(str,’yyyy’) 根据格式整理日期
last_day(to_date(str)) 求当月最后一天日期
collect_set(col) 收集数据返回一个以逗号分割的字符串数组
get_json_object(jsondata,’$.object’) 解析json，使用'$. object’获取对象值
NVL(str,replace) 空字段赋值，str为空返回replace值；两个都为空则返回null

#### 6.Hive如何实现分区

建表：create table tablename(col1 string) partitioned by(col2 string);
添加分区：alter table tablename add partition(col2=’202101’);
删除分区：alter table tablename drop partition(col2=’202101’);

#### 7.Hive导入数据的五种方式

1. Load方式，可以从本地或HDFS上导入，本地是copy，HDFS是移动

本地：load data local inpath ‘/root/student.txt’ into table student;
HDFS：load data inpath ‘/user/hive/data/student.txt’ into table student;

2. Insert方式，往表里插入

insert into table student values(1,’zhanshan’);

3. As select方式，根据查询结果创建表并插入数据

create table if not exists stu1 as select id,name from student;

4. Location方式，创建表并指定数据的路径

create external if not exists stu2 like student location '/user/hive/warehouse/student/student.txt';

5. Import方式，先从hive上使用export导出在导入

import table stu3 from ‘/user/export/student’;

#### 8.Hive导出数据的五种方式

1. Insert方式，查询结果导出到本地或HDFS

Insert overwrite local directory ‘/root/insert/student’ select id,name from student;
Insert overwrite directory ‘/user/ insert /student’ select id,name from student;

2. Hadoop命令导出本地

hive>dfs -get /user/hive/warehouse/student/ 000000_0  /root/hadoop/student.txt

3. hive Shell命令导出

]$ bin/hive -e ‘select id,name from student;’  > /root/hadoop/student.txt

4. Export导出到HDFS

hive> export table student to ‘/user/export/student’;

5. Sqoop导出

#### 9.Hive 表关联查询，如何解决数据倾斜的问题？

1） 倾斜原因：
map 输出数据按key Hash 的分配到reduce 中，由于key 分布不均匀、业务数据本身的特、建表时考虑不周、等原因造成的reduce 上的数据量差异过大。
（1） key 分布不均匀;
（2） 业务数据本身的特性;
（3） 建表时考虑不周;
（4） 某些SQL 语句本身就有数据倾斜;
如何避免：对于key 为空产生的数据倾斜，可以对其赋予一个随机值。
2） 解决方案
（1） 参数调节：
hive.map.aggr = true
hive.groupby.skewindata=true
有数据倾斜的时候进行负载均衡，当选项设定位true,生成的查询计划会有两个MR Job。第一个MR Job 中，Map 的输出结果集合会随机分布到Reduce 中，每个Reduce 做部分聚合操作，并输出结果，这样处理的结果是相同的Group By Key 有可能被分发到不同的Reduce 中，从而达到负载均衡的目的；第二个MR Job 再根据预处理的数据结果按照Group By Key 分布到Reduce 中（这个过程可以保证相同的Group By Key 被分布到同一个Reduce 中）， 最后完成最终的聚合操作。
（2） SQL 语句调节：
① 选用join key 分布最均匀的表作为驱动表。做好列裁剪和filter 操作，以达到两表做join 的时候，数据量相对变小的效果。
② 大小表Join：使用map join 让小的维度表（1000 条以下的记录条数）先进内存。在map 端完成reduce.
③ 大表Join 大表：把空值的key 变成一个字符串加上随机数，把倾斜的数据分到不同的reduce 上，由于null 值关联不上，处理后并不影响最终结果。
④ count distinct 大量相同特殊值:count distinct 时，将值为空的情况单独处理，如果是计算count distinct，可以不用处理， 直接过滤，在最后结果中加1。如果还有其他计算，需要进行group by，可以先将值为空的记录单独处理，再和其他计算结果进行union。

#### 10.写出hive 中split、coalesce 及collect_list 函数的用法（可举例）？

split 将字符串转化为数组，即：split(‘a,b,c,d’ , ‘,’) ==> [“a”,“b”,“c”,“d”]。
coalesce(T v1, T v2, …) 返回参数中的第一个非空值；如果所有值都为NULL，那么返回NULL。
collect_list 列出该字段所有的值，不去重select collect_list(id) from table。

#### 11.Hive 有哪些方式保存元数据，各有哪些特点？

Hive 支持三种不同的元存储服务器，分别为：内嵌式元存储服务器、本地元存储服务器、远程元存储服务器，每种存储方式使用不同的配置参数。
内嵌式元存储主要用于单元测试，在该模式下每次只有一个进程可以连接到元存储，Derby 是内嵌式元存储的默认数据库。
在本地模式下，每个Hive 客户端都会打开到数据存储的连接并在该连接上请求SQL 查询。
在远程模式下，所有的Hive 客户端都将打开一个到元数据服务器的连接，该服务器依次查询元数据，元数据服务器和客户端之间使用Thrift 协议通信。

#### 12.Hive 的HSQL 转换为MapReduce 的过程？

HiveSQL ->AST( 抽象语法树) -> QB( 查询块) ->OperatorTree （ 操作树） -> 优化后的操作树->mapreduce 任务树->优化后的mapreduce 任务树

过程描述如下：
SQL Parser：Antlr 定义SQL 的语法规则，完成SQL 词法，语法解析，将SQL 转化为抽象语法树AST Tree；
Semantic Analyzer：遍历AST Tree，抽象出查询的基本组成单元QueryBlock；
Logical plan：遍历QueryBlock，翻译为执行操作树OperatorTree；
Logical plan optimizer: 逻辑层优化器进行OperatorTree 变换， 合并不必要的ReduceSinkOperator，减少shuffle 数据量；
Physical plan：遍历OperatorTree，翻译为MapReduce 任务；
Logical plan optimizer：物理层优化器进行MapReduce 任务的变换，生成最终的执行计划；

#### 13.Hive join 过程中大表小表的放置顺序？

将最大的表放置在JOIN 语句的最右边，或者直接使用/*+ streamtable(table_name) */指出。
在编写带有join 操作的代码语句时，应该将条目少的表/子查询放在Join 操作符的左边。因为在Reduce 阶段，位于Join 操作符左边的表的内容会被加载进内存，载入条目较少的表可以有效减少OOM（out of memory）即内存溢出。所以对于同一个key 来说，对应的value 值小的放前，大的放后，这便是“小表放前”原则。若一条语句中有多个Join， 依据Join 的条件相同与否，有不同的处理方法。

#### 14.Hive 的两张表关联，使用MapReduce 怎么实现？

如果其中有一张表为小表，直接使用map 端join 的方式（map 端加载小表）进行聚合。
如果两张都是大表，那么采用联合key，联合key 的第一个组成部分是join on 中的公共字段，第二部分是一个flag，0 代表表A，1 代表表B，由此让Reduce 区分客户信息和订单信息；在Mapper 中同时处理两张表的信息，将join on 公共字段相同的数据划分到同一个分区中，进而传递到一个Reduce 中，然后在Reduce 中实现聚合。

#### 15.所有的Hive 任务都会有MapReduce 的执行吗？

不是，从Hive0.10.0 版本开始，对于简单的不需要聚合的类似 SELECT from t1 LIMIT n 语句，不需要起MapReduce job，直接通过Fetch task 获取数据。

#### 16.Hive 的函数：UDF、UDAF、UDTF 的区别？

UDF: 单行进入，单行输出
UDAF: 多行进入，单行输出
UDTF: 单行输入，多行输出

#### 17.说说对Hive 桶表的理解？

桶表是对数据进行哈希取值，然后放到不同文件中存储。
数据加载到桶表时，会对字段取hash 值，然后与桶的数量取模。把数据放到对应的文件中。物理上，每个桶就是表(或分区）目录里的一个文件，一个作业产生的桶(输出文件)和reduce 任务个数相同。
桶表专门用于抽样查询，是很专业性的，不是日常用来存储数据的表，需要抽样查询时， 才创建和使用桶表。

#### 18.Hive 自定义UDF 函数的流程?

1） 写一个类继承（org.apache.hadoop.hive.ql.）UDF 类；
2） 覆盖方法evaluate()；
3） 打JAR 包；
4） 通过hive 命令将JAR 添加到Hive 的类路径：
hive> addjar /home/ubuntu/ToDate.jar;
5） 注册函数：
hive> create temporary function xxx as ‘XXX’;
6） 使用函数；
7）[可选] drop 临时函数；

#### 19.说下Hive的基本架构

Hive的体系结构可以分为以下几部分
（1） 用户接口主要有三个：CLI，Client 和 WUI。其中最常用的是CLI，Cli启动的时候，会同时启动一个Hive副本。Client是Hive的客户端，用户连接至Hive Server。在启动 Client模式的时候，需要指出Hive Server所在节点，并且在该节点启动Hive Server。 WUI是通过浏览器访问Hive。

（2）Hive将元数据存储在数据库中，如mysql、derby。Hive中的元数据包括表的名字，表的列和分区及其属性，表的属性（是否为外部表等），表的数据所在目录等。

（3）解释器、编译器、优化器完成HQL查询语句从词法分析、语法分析、编译、优化以及查询计划的生成。生成的查询计划存储在HDFS中，并在随后有MapReduce调用执行。

（4）Hive的数据存储在HDFS中，大部分的查询、计算由MapReduce完成（包含*的查询，比如select * from tbl不会生成MapRedcue任务）。

#### 20.hive分区和分桶的区别

分区partition
划分数据集, 通过分区减少每次扫描的总数据量.
分区使用hdfs的子目录功能实现, 每个子目录都包含了分区对应的列名和每一列的值, 但是hdfs并不支持大量的子目录, 所以分区的数量是有限制的, 要先对表中分区数量进行预估, 从而避免分区数量过大带来的问题.
分区的划分是非随机的.

分桶bucket
在分区数量过于庞大以至于可能导致文件系统崩溃时使用分桶来解决问题.
分桶是通过对指定列进行哈希计算来实现, 使用列的哈希值对数据打散, 然后分发到不同的桶中从而完成数据的分桶.
在数据量够大的情况下, 分桶比分区更有查询效率.
分桶的划分是随机的.


#### 21.hive的执行流程

用户提交查询等任务给Driver。
Driver为查询操作创建一个session handler，接着dirver会发送查询操作到compiler去生成一个execute plan
Compiler根据用户任务去MetaStore中获取需要的Hive的元数据信息。这些元数据在后续stage中用作抽象语法树的类型检测和修剪。
Compiler得到元数据信息，对task进行编译，先将HiveQL转换为抽象语法树，然后将抽象语法树转换成查询块，将查询块转化为逻辑的查询plan，重写逻辑查询plan，将逻辑plan转化为物理的plan（MapReduce）, 最后选择最佳策略。
将最终的plan提交给Driver。
Driver将plan转交给ExecutionEngine去执行，将获取到的元数据信息，提交到JobTracker或者RsourceManager执行该task，任务会直接读取到HDFS中进行相应的操作。
获取执行的结果。
取得并返回执行结果。

#### 参考资料

https://blog.csdn.net/wushuoyouting/article/details/113120237

https://blog.csdn.net/wzc8961661/article/details/104509518/

https://blog.csdn.net/Dota_Data/article/details/94993195