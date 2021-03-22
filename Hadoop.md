## Hadoop

- [1、简要描述如何安装配置一个 apache 开源版 hadoop，描述即可，列出步骤更好](#1简要描述如何安装配置一个-apache-开源版-hadoop描述即可列出步骤更好)

* [2、请列出正常工作的 hadoop 集群中 hadoop 都需要启动哪些进程，他们的作用分别是什么？](#2请列出正常工作的-hadoop-集群中-hadoop-都需要启动哪些进程他们的作用分别是什么)
* [3、启动 hadoop 报如下错误，该如何解决？](#3启动-hadoop-报如下错误该如何解决)
* [4、请列出你所知道的 hadoop 调度器，并简要说明其工作方法？](#4请列出你所知道的-hadoop-调度器并简要说明其工作方法)
* [5、当前日志采样格式为如下，请编写 MapReduce 计算第四列每个元素出现的个数](#5当前日志采样格式为如下请编写-mapreduce-计算第四列每个元素出现的个数)
* [6、hive 有哪些方式保存元数据，各有哪些特点？](#6hive-有哪些方式保存元数据各有哪些特点)
* [7、请简述 hadoop 怎么样实现二级排序？](#7请简述-hadoop-怎么样实现二级排序)
* [8、用非递归方法实现二分查找](#8用非递归方法实现二分查找)
* [9、请简述 mapreduce 中，combiner，partition 作用？](#9请简述-mapreduce-中combinerpartition-作用)
* [10、HDFS 数据写入实现机制](#10hdfs-数据写入实现机制)
* [11、hadoop 节点的动态上线下线的大概操作](#11hadoop-节点的动态上线下线的大概操作)
* [12.MapTask 并行机制是由什么决定的？](#12maptask-并行机制是由什么决定的)
* [13.MR 是干什么的？](#13mr-是干什么的)
* [14.combiner 和 partition 的作用：](#14combiner-和-partition-的作用)
* [15.什么是 shuffle](#15什么是-shuffle)
* [16.列举几个 hadoop 生态圈的组件并做简要描述](#16列举几个-hadoop-生态圈的组件并做简要描述)
* [17.NameNode 的 Safemode 是怎么回事? 如何才能退出 safemode？](#17namenode-的-safemode-是怎么回事-如何才能退出-safemode)
* [18.SecondaryNameNode 的主要职责是什么？简述其工作机制](#18secondarynamenode-的主要职责是什么简述其工作机制)
* [19.一个 datanode 宕机，怎么恢复，简单说一下恢复流程？(运维)](#19一个-datanode-宕机怎么恢复简单说一下恢复流程运维)
* [20.hadoop 的 namenode 宕机，怎么解决？（运维）](#20hadoop-的-namenode-宕机怎么解决运维)
* [21.简述 hadoop 安装？（运维）](#21简述-hadoop-安装运维)
* [22.Hadoop 中需要哪些配置文件，其作用是什么？（运维）](#22hadoop-中需要哪些配置文件其作用是什么运维)
* [23. 请列出 hadoop 正常工作时要启动哪些进程，并写出各自的作用](#23-请列出-hadoop-正常工作时要启动哪些进程并写出各自的作用)
* [参考链接](#参考链接)

### Hadoop

#### 1、简要描述如何安装配置一个 apache 开源版 hadoop，描述即可，列出步骤更好



​    -- 解压 hadoop 包，到指定安装文件夹



​    -- 配置 linux 基本网络环境、jdk 环境、防火墙环境



​    -- 修改主机名，方便后面 UI 的访问



​    -- 修改 hadoop/etc/hadoop/conf 下的配置文件，根据部署的模式和需要进行配置



​    -- 格式化 namenode，对数据缓存的的路径进行格式化



​    -- 启动 hadoop 进程



#### 2、请列出正常工作的 hadoop 集群中 hadoop 都需要启动哪些进程，他们的作用分别是什么？

![img](https://img-blog.csdn.net/20180623164802867?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21ha2V0dWJ1Nw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

​    --namenode =>HDFS 的守护进程，负责维护整个文件系统，存储着整个文件系统的元数据信息，有 image+edit log namenode 不会持久化存储这些数据，而是在启动时重建这些数据。

​    --datanode => 是具体文件系统的工作节点，当我们需要某个数据，namenode 告诉我们去哪里找，就直接和那个 DataNode 对应的服务器的后台进程进行通信，由 DataNode 进行数据的检索，然后进行具体的读 / 写操作

​    --secondarynamenode => 一个冗余的守护进程，相当于一个 namenode 的元数据的备份机制，定期的更新，和 namenode 进行通信，将 namenode 上的 image 和 edits 进行合并，可以作为 namenode 的备份使用

​    --resourcemanager => 是 yarn 平台的守护进程，负责所有资源的分配与调度，client 的请求由此负责，监控 nodemanager

​    --nodeanager => 是单个节点的资源管理，执行来自 resourcemanager 的具体任务和命令

#### 3、启动 hadoop 报如下错误，该如何解决？

  --1.error org.apache.hadoop.hdfs.server.namenode.NameNode 

​    -- 找不到主类，应该是配置文件的 hadoop 的安装位置配置错误，对 hadoop-env.sh 文件进行检查修改

  --2.org.apache.hadoop.hdfs.server.common.inconsistentFSStateException

​    -- 这个是存储目录不存在，或者被删除，对 namenode 进行格式化，或重新格式化，对 tmp.dir 进行自己的设置

  --3.Diretory /tmp/hadoop-root/dfs/name is in an inconsistent 

​    -- 这个和上面一样的，重新设置 core-site.xml 中 hadoop.tmp.dir 的值，对 namenode 进行格式化，

  --4.state storage direction does not exist or is not accessible?

​    -- 之前是默认的 tmp 目录，每次重启都会清除这个数据，所以找不到整个文件系统的信息，重新设置 core-site.xml 中 hadoop.tmp.dir 的值，对 namenode 进行格式化，

#### 4、请列出你所知道的 hadoop 调度器，并简要说明其工作方法？

  --1. 先进先出调度器（FIFO）

  --Hadoop 中默认的调度器，也是一种批处理调度器。它先按照作业的优先级高低，再按照到达时间的先后选择被执行的作业

 --2. 容量调度器（Capacity Scheduler)

  -- 支持多个队列，每个队列可配置一定的资源量，每个队列采用 FIFO 调度策略，为了防止同一个用户的作业独占队列中的资源，该调度器会对同一用户提交的作业所占资源量进行限定。调度时，首先按以下策略选择一个合适队列：计算每个队列中正在运行的任务数与其应该分得的计算资源之间的比值，选择一个该比值最小的队列；然后按以下策略选择该队列中一个作业：按照作业优先级和提交时间顺序选择，同时考虑用户资源量限制和内存限制

 --3. 公平调度器（Fair Scheduler）

  -- 公平调度是一种赋予作业（job）资源的方法，它的目的是让所有的作业随着时间的推移，都能平均的获取等同的共享资源。所有的 job 具有相同的资源, 当单独一个作业在运行时，它将使用整个集群。当有其它作业被提交上来时，系统会将任务（task）空闲资源（container）赋给这些新的作业，以使得每一个作业都大概获取到等量的 CPU 时间。与 Hadoop 默认调度器维护一个作业队列不同，这个特性让小作业在合理的时间内完成的同时又不 "饿" 到消耗较长时间的大作业。公平调度可以和作业优先权搭配使用——优先权像权重一样用作为决定每个作业所能获取的整体计算时间的比例。同计算能力调度器类似，支持多队列多用户，每个队列中的资源量可以配置， 同一队列中的作业公平共享队列中所有资源。

#### 5、当前日志采样格式为如下，请编写 MapReduce 计算第四列每个元素出现的个数



**a,b,c,d**

**a,s,d,f**

**d,f,g,c    就如此格式，**



代码如下，比 wordcount 还要简单一点，代码差不多的



```
package make.hadoop.com.four_column;
 
import java.io.IOException;
 
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.conf.Configured;
import org.apache.hadoop.fs.FileSystem;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.util.Tool;
import org.apache.hadoop.util.ToolRunner;
 
public class four_column extends Configured implements Tool {
	// 1、自己的map类
	// 2、继承mapper类，<LongWritable, Text, Text,
	// IntWritable>输入的key,输入的value，输出的key,输出的value
	public static class MyMapper extends
			Mapper<LongWritable, Text, Text, IntWritable> {
		private IntWritable MapOutputkey = new IntWritable(1);
		private Text MapOutputValue = new Text();
 
		@Override
		protected void map(LongWritable key, Text value, Context context)
				throws IOException, InterruptedException {
 
			String strs = value.toString();
			// 分割数据
			String str_four = strs.split(",")[3];
 
			MapOutputValue.set(str_four);
			System.out.println(str_four);
			context.write(MapOutputValue, MapOutputkey);
 
		}
	}
	// 2、自己的reduce类，这里的输入就是map方法的输出
	public static class MyReduce extends
			Reducer<Text, IntWritable, Text, IntWritable> {
 
		IntWritable countvalue = new IntWritable(1);
 
		@Override
		// map类的map方法的数据输入到reduce类的group方法中，得到<text,it(1,1)>,再将这个数据输入到reduce方法中
		protected void reduce(Text inputkey, Iterable<IntWritable> inputvalue,
				Context context) throws IOException, InterruptedException {
 
			int sum = 0;
 
			for (IntWritable i : inputvalue) {
				System.out.println(i.get());
				sum = sum + i.get();
			}
			// System.out.println("key: "+inputkey + "...."+sum);
			countvalue.set(sum);
			context.write(inputkey, countvalue);
		}
	}
	// 3运行类，run方法，在测试的时候使用main函数，调用这个类的run方法来运行
 
	/**
	 * param args 参数是接受main方得到的参数，在run中使用
	 */
	public int run(String[] args) throws Exception {
 
		Configuration conf = new Configuration();
 
		Job job = Job.getInstance(this.getConf(), "four_column");
 
		// set mainclass
		job.setJarByClass(four_column.class);
 
		// set mapper
		job.setMapperClass(MyMapper.class);
		job.setMapOutputKeyClass(Text.class);
		job.setMapOutputValueClass(IntWritable.class);
 
		// set reducer
		job.setReducerClass(MyReduce.class);
		job.setOutputKeyClass(Text.class);
		job.setOutputValueClass(IntWritable.class);
 
		// set path
		Path inpath = new Path(args[0]);
		FileInputFormat.setInputPaths(job, inpath);
		Path outpath = new Path(args[1]);
		FileOutputFormat.setOutputPath(job, outpath);
		FileSystem fs = FileSystem.get(conf);
		// 存在路径就删除
		if (fs.exists(outpath)) {
			fs.delete(outpath, true);
		}
		job.setNumReduceTasks(1);
 
		boolean status = job.waitForCompletion(true);
 
		if (!status) {
			System.err.println("the job is error!!");
		}
 
		return status ? 0 : 1;
 
	}
	public static void main(String[] args) throws IOException,
			ClassNotFoundException, InterruptedException {
 
		Configuration conf = new Configuration();
 
		int atatus;
		try {
			atatus = ToolRunner.run(conf, new four_column(), args);
			System.exit(atatus);
		} catch (Exception e) {
			e.printStackTrace();
		}
 
	}
}
```

#### 6、hive 有哪些方式保存元数据，各有哪些特点？

  --1. 内嵌 Derby 数据库存储

​    -- 这个是 hive 默认自带的内嵌数据库，用来储存元数据，但这个在配置了 hiveserver2 和 metastore 服务后，不支持多个用户同时登录，不方便对数据库的安全访问

  --2.multi user mode

​    -- 在自己本地配一个，mysql 的数据库用作，hive 的元数据的存储数据库，这个需要要自己本地搭建一个 mysql 数据库，通过配置文件创建一个，hive 自己的元数据库，也是我们学习一般会用的方式，配置一般如下

```
<property>
 jasbdaksbdaskbdoajsbdasbu
  <name>javax.jdo.option.ConnectionURL</name>
  <value>jdbc:mysql://hostname:3306/hive?createDatabaseIfNotExist=true</value>
  <description>JDBC connect string for a JDBC metastore</description>
</property>
<property>
  <name>hive.metastore.uris</name>
    <value>thrift://hostname:9083</value>
      <description>IP address (or fully-qualified domain name) and port of the metastore host</description>
</property>
<property>
  <name>javax.jdo.option.ConnectionDriverName</name>
  <value>com.mysql.jdbc.Driver</value>
  <description>Driver class name for a JDBC metastore</description>
</property>
 
<property>
  <name>javax.jdo.option.ConnectionUserName</name>
  <value>xxxx</value>
  <description>username to use against metastore database</description>
</property>
 
<property>
  <name>javax.jdo.option.ConnectionPassword</name>
  <value>xxxx</value>
  <description>password to use against metastore database</description>
</property>
```

  --3.remote server mode

​    -- 一种在远端配置数据库服务的方式，这个需要配置 metastore 服务，通过客户端的 metastore 服务访问服务器上的元数据库达到访问数据的目的

#### 7、请简述 hadoop 怎么样实现二级排序？

​    -- 在 MapReduce 中本身就会对我们 key 进行排序，所以我们要对 value 进行排序，主要思想为将 key 和部分 value 拼接成一个组合 key（实现 WritableComparable 接口或者调用 setSortComparatorClass 函数），这样 reduce 获取的结果便是先按 key 排序，后按 value 排序的结果，在这个方法中，用户需 要自己实现 Paritioner，继承 Partitioner<>, 以便只按照 key 进行数据划分。Hadoop 显式的支持二次排序，在 Configuration 类中有个 setGroupingComparatorClass() 方法，可用于设置排序 group 的 key 值。

####  8、用非递归方法实现二分查找

​    -- 代码如下，二分查找只适用于有序数列，对其进行查找，效率非常高，不适用于无序数列

```
public static int binSearch(int srcArray[], int key) {
		int mid;
		int start = 0;
		int end = srcArray.length - 1;
		while (start <= end) {
			mid = (end - start) / 2 + start;
			if (key < srcArray[mid]) {
				end = mid - 1;
			} else if (key > srcArray[mid]) {
				start = mid + 1;
			} else {
				return mid;
			}
		}
		return -1;
	}
```

递归的二分查找

```
public static int binSearch_di(int srcArray[], int start, int end, int key) {
		int mid = (end - start) / 2 + start;
		if (srcArray[mid] == key) {
			return mid;
		}
		if (start >= end) {
			return -1;
		} else if (key > srcArray[mid]) {
			return binSearch_di(srcArray, mid + 1, end, key);
		} else if (key < srcArray[mid]) {
			return binSearch_di(srcArray, start, mid - 1, key);
		}
		return -1;
	}
```

####  9、请简述 mapreduce 中，combiner，partition 作用？

​    -- 在 MapReduce 整个过程中，combiner 是可有可无的，需要是自己的情况而定，如果只是单纯的对 map 输出的 key-value 进行一个统计，则不需要进行 combiner，combiner 相当于提前做了一个 reduce 的工作，减轻了 reduce 端的压力，



Combiner 只应该适用于那种 Reduce 的输入（key：value 与输出（key：value）类型完全一致，且不影响最终结果的场景。比如累加，最大值等，也可以用于过滤数据，在 map 端将无效的数据过滤掉。



在这些需求场景下，输出的数据是可以根据 key 值来作合并的，合并的目的是减少输出的数据量，减少 IO 的读写，减少网络传输, 以提高 MR 的作业效率。



  1.combiner 的作用就是在 map 端对输出先做一次合并, 以减少传输到 reducer 的数据量.



  2.combiner 最基本是实现本地 key 的归并, 具有类似本地 reduce, 那么所有的结果都是 reduce 完成, 效率会相对降低。



  \3. 使用 combiner, 先完成的 map 会在本地聚合, 提升速度.



​    --partition 意思为分开，分区。它分割 map 每个节点的结果，按照 key 分别映射给不同的 reduce，也是可以自定义的。其实可以理解归类。也可以理解为根据 key 或 value 及 reduce 的数量来决定当前的这对输出数据最终应该交由哪个 reduce task 处理 
partition 的作用就是把这些数据归类。每个 map 任务会针对输出进行分区，及对每一个 reduce 任务建立一个分区。划分分区由用户定义的 partition 函数控制，默认使用哈希函数来划分分区。 
HashPartitioner 是 mapreduce 的默认 partitioner。计算方法是 



which reducer=(key.hashCode() & Integer.MAX_VALUE) % numReduceTasks，得到当前的目的 reducer。

####  10、HDFS 数据写入实现机制

​    -- 写入 HDFS 过程：

​      1、根 namenode 通信请求上传文件，namenode 检查目标文件是否已存在，父目录是否存在 
​      2、namenode 返回是否可以上传 
​      3、client 会先对文件进行切分，比如一个 blok 块 128m，文件有 300m 就会被切分成 3 个块，一个 128M、一个 128M、一个 44M 请求第一个 block 该传输到哪些 datanode 服务器上 
​      4、namenode 返回 datanode 的服务器 
​      5、client 请求一台 datanode 上传数据（本质上是一个 RPC 调用，建立 pipeline），第一个 datanode 收到请求会继续调用第二个 datanode，然后第二个调用第三个 datanode，将整个 pipeline 建立完成，逐级返回客户端 
​      6、client 开始往 A 上传第一个 block（先从磁盘读取数据放到一个本地内存缓存），以 packet 为单位（一个 packet 为 64kb），当然在写入的时候 datanode 会进行数据校验，它并不是通过一个 packet 进行一次校验而是以 chunk 为单位进行校验（512byte），第一台 datanode 收到一个 packet 就会传给第二台，第二台传给第三台；第一台每传一个 packet 会放入一个应答队列等待应答 
​      7、当一个 block 传输完成之后，client 再次请求 namenode 上传第二个 block 的服务器。

​    -- 读取文件过程：

​      使用 HDFS 提供的客户端开发库 Client，向远程的 Namenode 发起 RPC 请求；Namenode 会视情况返回文件的部分或全部 block 列表，对于每个 block，Namenode 都会返回有该 block 拷贝的 DataNode 地址；客户端开发库 Client 会选取离客户端最接近的 DataNode 来读取 block；如果客户端本身就是 DataNode, 那么将从本地直接获取数据. 读取完当前 block 的数据后，关闭与当前的 DataNode 连接，并为读取下一个 block 寻找最佳的 DataNode；当读完列表的 block 后，且文件读取还没有结束，客户端开发库会继续向 Namenode 获取下一批的 block 列表。读取完一个 block 都会进行 checksum 验证，如果读取 datanode 时出现错误，客户端会通知 Namenode，然后再从下一个拥有该 block 拷贝的 datanode 继续读。

####  11、hadoop 节点的动态上线下线的大概操作

​    -- 节点上线


​      \1. 关闭新增节点的防火墙
​      \2. 在 NameNode 节点的 hosts 文件中加入新增数据节点的 hostname
​      \3. 在每个新增数据节点的 hosts 文件中加入 NameNode 的 hostname
​      \4. 在 NameNode 节点上增加新增节点的 SSH 免密码登录的操作
​      \5. 在 NameNode 节点上的 dfs.hosts 中追加上新增节点的 hostname,
​      \6. 在其他节点上执行刷新操作：hdfs dfsadmin -refreshNodes
​      \7. 在 NameNode 节点上，更改 slaves 文件，将要上线的数据节点 hostname 追加
​      到 slaves 文件中
​      \8. 启动 DataNode 节点

​      \9. 查看 NameNode 的监控页面看是否有新增加的节点   

​    -- 节点下线

​      \1. 修改 / conf/hdfs-site.xml 文件
​      \2. 确定需要下线的机器，dfs.osts.exclude 文件中配置好需要下架的机器，这个是阻
​      止下架的机器去连接 NameNode
​      \3. 配置完成之后进行配置的刷新操作./bin/hadoop dfsadmin -refreshNodes, 这个
​      操作的作用是在后台进行 block 块的移动
​      \4. 当执行三的命令完成之后，需要下架的机器就可以关闭了，可以查看现在集
​      群上连接的节点，正在执行 Decommission，会显示：
​      Decommission Status : Decommission in progress 执行完毕后，会显示：
​      Decommission Status : Decommissioned

​      \5. 机器下线完毕，将他们从 excludes 文件中移除。

#### 12.MapTask 并行机制是由什么决定的？
由切片数量决定，每一个 split 分配一个 mapTask 并行实例处理。

#### 13.MR 是干什么的？
hadoop mapreduce 采用分而治之的思想，大问题分解成小问题同时并发处理小问题然后再合并结果。
分解（map） 合并（reduce）

#### 14.combiner 和 partition 的作用：
`combiner`的意义是对每一个 map 任务的输出进行局部汇总，以减小网络传输量。

```
（combiner阶段：也可以称为local reduce）combiner阶段是程序员可以选择的，Combiner是一个本地化的reduce操作，它是map运算的后续操作，主要是在map计算出中间文件前做一个简单的合并重复key值的操作，因此在reduce计算前对相同的key做一个合并操作，那么文件会变小，这样就提高了宽带的传输效率，毕竟hadoop计算力宽带资源往往是计算的瓶颈也是最为宝贵的资源，但是combiner操作是有风险的，使用它的原则是combiner的输入不会影响到reduce计算的最终输出，（combiner需要满足结合律和交换律）

结合律：map把任务分配给combine，如果combine上有key值相同的，combine进行结合value值相加
交换律：combine输出后经过shuffer重新洗牌，不同map的相同key会分到一个map中，然后输出给reduce
```

[shuffle 博客](https://blog.csdn.net/zpf336/article/details/80931629)

![img](https://img.kancloud.cn/f3/1b/f31ba1ec5dc65bca94191a53d93dad07_431x415.png)



`Partition` 默认使用的是 HashPartitioner
获取 key 的哈希值，使用 key 的哈希值对 Reduce 任务数求模，进行分桶， 按照 key 分别映射给不同的 reduce， 这样做的目的是可以把 (key,value) 对均匀的分发到各个对应编号的 reduce 节点上，达到 reduce task 节点的负载均衡



```
在进行MapReduce计算时，有时候需要把最终的输出数据分到不同的文件中，比如按照省份划分的话，需要把同一省份的数据放到一个文件中；按照性别划分的话，需要把同一性别的数据放到一个文件中。我们知道最终的输出数据是来自于Reducer任务。那么，如果要得到多个文件，意味着有同样数量的Reducer任务在运行。Reducer任务的数据来自于Mapper任务，也就说Mapper任务要划分数据，对于不同的数据分配给不同的Reducer任务运行。Mapper任务划分数据的过程就称作Partition。负责实现划分数据的类称作Partitioner。
```



#### 15.什么是 shuffle

```
map阶段处理的数据如何传递给reduce阶段，是mapreduce框架中最关键的一个流程，这个流程就叫shuffle； shuffle: 洗牌（核心机制：数据分区，排序，缓存溢写，合并，分组）。
具体来说：就是将maptask输出的处理结果数据，分发给reducetask，并在分发的过程中，对数据按key进行了分区和排序，分组。
```



#### 16.列举几个 hadoop 生态圈的组件并做简要描述

```
1.Zookeeper:是一个开源的分布式应用程序协调服务,基于zookeeper 可以实现同步服务， 配置维护，命名服务。
2.Flume:一个高可用的，高可靠的，分布式的海量日志采集、聚合和传输的系统。
3.Hbase:是一个分布式的、面向列的开源数据库, 利用Hadoop HDFS 作为其存储系统。
4.Hive:基于Hadoop 的一个数据仓库工具，可以将结构化的数据文件映射为数据库表， 并提供简单的sql 查询功能，可以将sql 语句转换为MapReduce 任务进行运行。
```



####  17.NameNode 的 Safemode 是怎么回事? 如何才能退出 safemode？

```
namenode在刚启动的时候元数据只有文件块信息，没有文件所在datanode的信息，需要datanode自己向namenode汇报。如果namenode发现datanode汇报的文件块信息没有达到namenode内存中所有文件块的总阈值的一个百分比，namenode就会处于safemode。 只有达到这个阈值，namenode才会推出safemode。也可手动强制退出。
```

[Hadoop 中 namenode 的安全模式](https://blog.csdn.net/andyguan01_2/article/details/89711714)

####  18.SecondaryNameNode 的主要职责是什么？简述其工作机制

```
sn的主要职责是执行checkpoint操作 每隔一段时间，会由secondary namenode将namenode上的所有edits日志文件合并最新的fsimage持久化到磁盘上，并加载到内存进行merge（这个过程称为checkpoint）
```

#### 19.一个 datanode 宕机，怎么恢复，简单说一下恢复流程？(运维)

```
Datanode宕机了后，如果是短暂的宕机，可以实现写好脚本监控，将它启动起来。如果是长时间宕机了，那么datanode上的数据应该已经被备份到其他机器了， 那这台datanode就是一台新的datanode了，删除他的所有数据文件和状态文件，重新启动 。
```

####  20.hadoop 的 namenode 宕机，怎么解决？（运维）

```
先分析宕机后的损失，宕机后直接导致client无法访问，内存中的元数据丢失，但是硬盘中的元数据应该还存在，如果只是节点挂了， 重启即可，如果是机器挂了，重启机器后看节点是否能重启，不能重启就要找到原因修复了。 但是最终的解决方案应该是在设计集群的初期就考虑到这个问题，做namenode的HA。
```

####  21.简述 hadoop 安装？（运维）

```
1）使用 root 账户登录
2）修改 IP
3）修改 host 主机名
4）配置 SSH 免密码登录
5）关闭防火墙
6）安装 JDK
7）解压 hadoop 安装包
8）配置 hadoop 的核心文件 hadoop-env.sh，core-site.xml , mapred-site.xml ，hdfs-site.xml
9）配置 hadoop 环境变量
10）格式化 hadoop namenode-format
11）启动节点 start-all.sh
```

#### 22.Hadoop 中需要哪些配置文件，其作用是什么？（运维）

```
1.core-site.xml：
    fs.defaultFS:hdfs://cluster1(主机名)，这里的值指的是默认的HDFS 路径。
    hadoop.tmp.dir:/export/data/hadoop_tmp,这里的路径默认是NameNode、DataNode存储临时文件的（edits等信息）
    secondaryNamenode 等存放数据的公共目录。用户也可以自己单独指定这三类节点的目录。
    ha.zookeeper.quorum:hadoop101:2181,hadoop102:2181,hadoop103:2181,这里是
    ZooKeeper 集群的地址和端口。注意，数量一定是奇数，且不少于三个节点。
2.hadoop-env.sh:
    只需设置jdk 的安装路径，如：export JAVA_HOME=/usr/local/jdk。
3.hdfs-site.xml：
    dfs.replication:他决定着系统里面的文件块的数据备份个数，默认为3 个。
    dfs.data.dir:datanode 节点存储在文件系统的目录。
    dfs.name.dir:是namenode 节点存储hadoop 文件系统信息的本地系统路径。
4.mapred-site.xml：
    mapreduce.framework.name: yarn 指定mr 运行在yarn 上。
```



####  23. 请列出 hadoop 正常工作时要启动哪些进程，并写出各自的作用

```
namenode：管理集群并记录datanode的元数据，相应客户端的请求。
secondery namenode：对namenode一定范围内的数据做一份快照性备份。它不是namenode 的冗余守护进程，而是提供周期检查点和清理任务。帮 助NN 合并editslog，减少NN 启动时间。
datanode：存储数据。它负责管理连接到节点的存储（一个集群中可以有多个节点）。每个存储数据的节点运 行一个datanode 守护进程。
jobTracker：管理客户端提交的任务，并将任务分配给TaskTracker。 
ResourceManager（JobTracker）JobTracker 负责调度DataNode 上的工作。每个DataNode 有一个TaskTracker，它们执行实际工作。
TaskTracker：执行各个Task。
DFSZKFailoverController 高可用时它负责监控NN 的状态，并及时的把状态信息写入ZK 。它通过一个独立线程周期性的调用NN 上的一个特定接口来获取NN 的健康状态。FC 也有选择谁作为Active NN 的权利，因为最多只有两个节点，目前选择策略还比较简单（先到先得，轮换）。JournalNode 高可用情况下存放namenode 的editlog 文件。
```





#### 参考链接

https://blog.csdn.net/maketubu7/article/details/80784680

https://www.kancloud.cn/happycode-together/bigdata6/1681846