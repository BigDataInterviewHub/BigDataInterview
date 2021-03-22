## Sqoop


* [1.Sqoop 在工作中的定位是会用就行](#1sqoop-在工作中的定位是会用就行)
* [2.Sqoop导入hive时的参数](#2sqoop导入hive时的参数)
* [3.Rdbms中的增量数据如何导入？](#3rdbms中的增量数据如何导入)
* [4.Sqoop导入导出Null存储一致性问题](#4sqoop导入导出null存储一致性问题)
* [5.Sqoop数据导出一致性问题](#5sqoop数据导出一致性问题)
* [6.Sqoop底层运行的任务是什么](#6sqoop底层运行的任务是什么)
* [7.Map task并行度设置大于1的问题](#7map-task并行度设置大于1的问题)
* [8.Sqoop数据导出的时候一次执行多长时间](#8sqoop数据导出的时候一次执行多长时间)
* [9.sqoop 导入数据到HDFS注意事项](#9sqoop-导入数据到hdfs注意事项)
* [10.Sqoop1和sqoop2优缺点：](#10sqoop1和sqoop2优缺点)
* [参考链接](#参考链接)


#### 1.Sqoop 在工作中的定位是会用就行
1.1.1 Sqoop导入数据到hdfs中的参数

```
/opt/module/sqoop/bin/sqoop import \
--connect \ # 特殊的jdbc连接的字符串
--username \
--password \
--target-dir \  # hdfs目标的目录
--delete-target-dir \ # 导入的目标目录如果存在则删除那个目录
--num-mappers \   #相当于 -m ,并行导入时map task的个数
--fields-terminated-by   \
--query "$2"  ' and $CONDITIONS;' # 指定满足sql和条件的数据导入
```


#### 2.Sqoop导入hive时的参数

```一步将表结构和数据都导入到hive中
bin/sqoop import \
--connect jdbc的url字符串 \
--table mysql中的表名\
--username 账号 \
--password 密码\
--hive-import \
--m mapTask的个数\
--hive-database hive中的数据库名;
```


#### 3.Rdbms中的增量数据如何导入？
--check-column 字段名 \  #指定判断检查的依据字段
--incremental  导入模式\  # 用来指定增量导入的模式（Mode），append和lastmodified
--last-value 上一次导入结束的时间\
--m mapTask的个数 \
--merge-key 主键 

补充：
·如果使用merge-key合并模式 如果是新增的数据则增加，因为incremental是lastmodified模式，那么当有数据更新了，而主键没有变，则会进行合并。
·--check-column字段当数据更新和修改这个字段的时间也要随之变化，mysql中建表时该字段修饰符，字段名timestamp default current_timestamp on update current_timestamp


#### 4.Sqoop导入导出Null存储一致性问题
Hive中的Null在底层是以“\N”来存储，而MySQL中的Null在底层就是Null，为了保证数据两端的一致性,转化的过程中遇到null-string,null-non-string数据都转化成指定的类型，通常指定成"\N"。在导出数据时采用–input-null-string “\N” --input-null-non-string “\N” 两个参数。导入数据时采用–null-string “\N” --null-non-string “\N”。

Import导入和export导出的关系如下图所示。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191031175722107.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FpQmlnRGF0YQ==,size_16,color_FFFFFF,t_70)

#### 5.Sqoop数据导出一致性问题
1）场景1：如Sqoop在导出到Mysql时，使用4个Map任务，过程中有2个任务失败，那此时MySQL中存储了另外两个Map任务导入的数据，此时老板正好看到了这个报表数据。而开发工程师发现任务失败后，会调试问题并最终将全部数据正确的导入MySQL，那后面老板再次看报表数据，发现本次看到的数据与之前的不一致，这在生产环境是不允许的。

Sqoop官网中的用户指南

使用—staging-table选项，将hdfs中的数据先导入到辅助表中，当hdfs中的数据导出成功后，辅助表中的数据在一个事务中导出到目标表中（也就是说这个过程要不完全成功，要不完全失败）。

为了能够使用staging这个选项，staging表在运行任务前或者是空的，要不就使用—clear-staging-table配置，如果staging表中有数据，并且使用了—clear-staging-table选项,sqoop执行导出任务前会删除staging表中所有的数据。

注意：–direct导入时staging方式是不可用的，使用了—update-key选项时staging方式也不能用。

sqoop export \
--connect url \
--username root \
--password 123456 \
--table app_cource_study_report \
--columns watch_video_cnt,complete_video_cnt,dt \
--fields-terminated-by "\t" \
--export-dir "/user/hive/warehouse/tmp.db/app_cource_study_analysi_${day}" \
--staging-table app_cource_study_report_tmp \
--clear-staging-table \
--input-null-string '\\N' \
--null-non-string "\\N"


2）场景2：设置map数量为1个（不推荐，面试官想要的答案不只这个）

多个Map任务时，采用–staging-table方式，仍然可以解决数据一致性问题。

#### 6.Sqoop底层运行的任务是什么
只有Map阶段，没有Reduce阶段的任务。

#### 7.Map task并行度设置大于1的问题
并行度导入数据的 时候 需要指定根据哪个字段进行切分 该字段通常是主键或者是自增长不重复的数值类型字段，否则会报下面的错误。

Import failed: No primary key could be found for table. Please specify one with --split-by or perform a sequential import with ‘-m 1’.

那么就是说当map task并行度大于1时，下面两个参数要同时使用

–split-by id 指定根据id字段进行切分

–m n 指定map并行度n个

#### 8.Sqoop数据导出的时候一次执行多长时间
Sqoop任务5分钟-2个小时的都有。取决于数据量。

#### 9.sqoop 导入数据到HDFS注意事项

**分割符的方向问题**
首先sqoop的参数要小心, 从数据库导出数据，写到HDFS的文件中的时候，字段分割符号和行分割符号必须要用

1. --fields-terminated-by 


而不能是

1. --input-fields-terminated-by 


--input前缀的使用于读文件的分割符号，便于解析文件，所以用于从HDFS文件导出到某个数据库的场景。
两个方向不一样。



**参数必须用单引号括起来**
官方文档的例子是错的：

```apl
The octal representation of a UTF-8 character’s code point. This should be of the form \0ooo, where ooo is the octal value. For example, --fields-terminated-by \001 would yield the ^A character. 
```


应该写成

1. --fields-terminated-by '\001' 

复制代码

**创建Hive表**

```
1. CREATE EXTERNAL TABLE my_table( 
2.  id int, 
3. ... 
4. ) 
5. PARTITIONED BY ( 
6.  dt string) 
7. ROW FORMAT DELIMITED 
8.  FIELDS TERMINATED BY '\001' 
9.  LINES TERMINATED BY '\n' 
10. STORED AS textfile; 


```


要小心hive的bug，如果用\001, hive会友好的转换成\u0001
但是如果直接写\u0001, hive某些版本会变成u0001



STORED AS textfile 可以不用。

#### 10.Sqoop1和sqoop2优缺点：

sqoop1优点：架构部署简单

sqoop1缺点：命令行方式容易出错，格式紧耦合，无法支持所有数据类型，安全机制不够完善，例如密码暴漏，安装需要root权限，connector必须符合JDBC模型

sqoop2优点：多种交互方式，命令行，web UI，rest API，conncetor集中化管理，所有的链接安装在sqoop server上，完善权限管理机制，connector规范化，仅仅负责数据的读写

sqoop2缺点：sqoop2的缺点，架构稍复杂，配置部署更繁琐



#### 参考链接

https://blog.csdn.net/AiBigData/article/details/102842796

https://blog.csdn.net/robbyo/article/details/50523998

https://www.jianshu.com/p/ec9003d8918c