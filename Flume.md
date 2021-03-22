## Flume

- [1、什么是 Flume？](#1什么是-flume)

* [2、Flume 特点？](#2flume-特点)
* [3、flume 组成，Put 事物，Task 事务？](#3flume-组成put-事物task-事务)
* [4、Flume 拦截器？](#4flume-拦截器)
* [5.flume 和 kafka 采集日志区别，采集日志时中间停了，怎么记录之前的日志？](#5flume-和-kafka-采集日志区别采集日志时中间停了怎么记录之前的日志)
* [6、Flume 采集数据会丢失吗？(防止丢失机制)](#6flume-采集数据会丢失吗防止丢失机制)
* [7、Flume 内存？](#7flume-内存)
* [8、FlumeChannel 优化？](#8flumechannel-优化)
* [9.Flume数据传输的监控的](#9flume数据传输的监控的)
* [10.描述Flume拦截器开发过程中的核心方法有哪几个以及各自作用是什么？拦截器带来的优缺点各是什么](#10描述flume拦截器开发过程中的核心方法有哪几个以及各自作用是什么拦截器带来的优缺点各是什么)
* [11、flume 管道内存，flume 宕机了数据丢失怎么解决？](#11flume-管道内存flume-宕机了数据丢失怎么解决)
* [12、flume 和 kafka 采集日志区别，采集日志时中间停了，怎么记录之前的日志？](#12flume-和-kafka-采集日志区别采集日志时中间停了怎么记录之前的日志)
* [13、flume 有哪些组件，flume 的 source、channel、sink 具体是做什么的？](#13flume-有哪些组件flume-的-sourcechannelsink-具体是做什么的)
* [14.Channel Selector中的replicating和multiplexxing各是什么含义](#14channel-selector中的replicating和multiplexxing各是什么含义)
* [15.自定义开发实现TailDirSource支持递归文件夹数据的实时收集](#15自定义开发实现taildirsource支持递归文件夹数据的实时收集)
* [16. Flume 的 Channel](#16-flume-的-channel)
* [17.了解 Flume 的负载均衡和故障转移吗](#17了解-flume-的负载均衡和故障转移吗)
* [18.Flume参数调优](#18flume参数调优)
* [19.Flume的事务机制](#19flume的事务机制)
* [20.Flume Event 是数据流的基本单元](#20flume-event-是数据流的基本单元)
* [参考链接](#参考链接)

### 

#### 1、什么是 Flume？

答：Flume 是 Cloudera 公司的一款高性能、高可用的分布式日志收集系统，现在已经被 Apache 收购。

####  2、Flume 特点？

答：可靠性、可扩展性、可管理性、功能可扩展性、、

####  3、flume 组成，Put 事物，Task 事务？

答：Flume 组成，Put 事物，Task 事务
a. Taildir Source：断点续传、多目录
b. File Channel：数据存储在磁盘中，宕机数据可以保存。传输速率慢，适合对数据传输可靠性要求高的场景，例如：金融行业
c. Memory Channel：数据存储在内存中，宕机数据容易丢失。传输效率快，适合对数据传输可靠性要求不高的场景，例如：日志数据
d. Kafka Channel：减少 Flume 的 Sink 阶段，提高了传输效率
Source 到 Channel 是 Put 事务
Channel 到 Sink 是 Task 事务

####  4、Flume 拦截器？

答：1. 拦截器注意事项：
项目中定义了 ETL 拦截器和区分类型拦截器
采用两个拦截器的优缺点：
优点：模块化开发和可移植性
缺点：性能会低一些
\2. 自定义拦截器：
a. 实现 Interceptor
b. 重写四个方法：
initialize 初始化
public Event intercept(Event event) 处理单个 Event
public Listintercept(Listevents) 处理多个 event 方法，在这个方法中调用 Event intercept(Event event)
close 方法
c. 静态内部类，实现 interceptor.Builder
3.Flume Channel 选择器：

![img](https://www.pianshen.com/images/838/bf8af1962ed107f41d440e9b23d05eae.png)



#### 5.flume 和 kafka 采集日志区别，采集日志时中间停了，怎么记录之前的日志？

 Flume 采集日志是通过流的方式直接将日志收集到存储层，而 kafka 是将缓存在 kafka集群，待后期可以采集到存储层。
Flume 采集中间停了，可以采用文件的方式记录之前的日志，而 kafka 是采用 offset 的方式记录之前的日志。

#### 6、Flume 采集数据会丢失吗？(防止丢失机制)

答：不会，Channel 存储可以存储在 File 中，数据传输自身有事物

#### 7、Flume 内存？

答：开发中在 flume-env.sh 中设置 JVM heap 为 4G 或更高，部署在单独的服务器上 (4 核 8 线程 16G 内存)
—Xmx 与—Xms 最好设置一致，减少内存抖动带来的性能影响，如果不一致容易导致频繁 fullgc。

#### 8、FlumeChannel 优化？

答：通过配置多个 dataDirs 指向多个路径，每个路径对应不同的硬盘，增大 Flume 吞吐量
checkpointDir 和 backupCheckpointDir 也尽量配置在不同的硬盘对应的目录中，保证 checkpoint 坏掉后，可以快速使用 backupCheckpointDir 恢复数据

#### 9.Flume数据传输的监控的

使用第三方框架Ganglia实时监控Flume。

#### 10.描述Flume拦截器开发过程中的核心方法有哪几个以及各自作用是什么？拦截器带来的优缺点各是什么

initialize()：初始化，处理 Event
intercept(Event event)：处理单个事件
intercept(List events)：处理多个事件
close()：资源释放

缺点：
Flume中拦截器的作用就是对于event中header的部分可以按需塞入一些属性，当然你如果想要处理event的body内容，也是可以的，但是event的body内容是系统下游阶段真正处理的内容，如果让Flume来修饰body的内容的话，那就是强耦合了，这就违背了当初使用Flume来解耦的初衷

#### 11、flume 管道内存，flume 宕机了数据丢失怎么解决？

答：(1）Flume 的 channel 分为很多种，可以将数据写入到文件。
(2）防止非首个 agent 宕机的方法数可以做集群或者主备

####  12、flume 和 kafka 采集日志区别，采集日志时中间停了，怎么记录之前的日志？

答：(1)Flume 采集日志是通过流的方式直接将日志收集到存储层，而 kafka 是将缓存在 kafka 集群，待后期可以采集到存储层。
(2)Flume 采集中间停了，可以采用文件的方式记录之前的日志，而 kafka 是采用 offset 的方式记录之前的日志。

####  13、flume 有哪些组件，flume 的 source、channel、sink 具体是做什么的？

答：(1）source：用于采集数据，Source 是产生数据流的地方，同时 Source 会将产生的数据流传输到 Channel，这个有点类似于 Java IO 部分的 Channel。
(2）channel：用于桥接 Sources 和 Sinks，类似于一个队列。
(3）sink：从 Channel 收集数据，将数据写到目标源 (可以是下一个 Source，也可以是 HDFS 或者 HBase)。



#### 14.Channel Selector中的replicating和multiplexxing各是什么含义

一个是复制，一个是分流
复制：可以将最前端的数据源复制多份，分别传递到多个channel中，每个channel接收到的数据都是相同的
分流：selector可以根据header的值来确定数据传递到哪一个channel

#### 15.自定义开发实现TailDirSource支持递归文件夹数据的实时收集

修改org.apache.flume.source.taildir.ReliableTaildirEventReader类的getMatchFiles 方法

修改并新增两个方法：

```java
private List getMatchFiles(File parentDir, final Pattern fileNamePattern) {
List result = Lists.newArrayList();
for(File f: getAllFiles(parentDir)){
String fileName = f.getName();
if (fileNamePattern.matcher(fileName).matches()) {
result.add(f);
}
}
Collections.sort(result, new TailFile.CompareByLastModifiedTime());
return result;
}

private List getAllFiles(File parentDir){
List fileList = Lists.newArrayList();
getAllFiles(parentDir,fileList);
return fileList;
}

private void getAllFiles(File parentDir,List fileList){
File[] files = parentDir.listFiles();
if(null != files){
for(File file: parentDir.listFiles()){
if(file.isDirectory()){
getAllFiles(file,fileList);
}else{
fileList.add(file);
}
}
}
}
```



#### 16. Flume 的 Channel 

Channel 被设计为 Event 中转临时缓冲区，存储 Source 收集并且没有被 Sink 读取的 Event，为平衡 Source 收集和 Sink 读取的速度，可视为 Flume 内部的消息队列。

Channel **线程安全并且具有事务性**，支持 Source 写失败写，和 Sink 读失败重复读的操作。常见的类型包括 Memory Channel，File Channel，Kafka Channel。

#### 17.了解 Flume 的负载均衡和故障转移吗

目的是为了提高整个系统的容错能力和稳定性。简单配置就可以轻松实现，首先需要设置 Sink 组，同一个 Sink 组内有多个子 Sink，不同 Sink 之间可以配置成负载均衡或者故障转移。

#### 18.Flume参数调优

Source
增加Source个数（使用Tair Dir Source时可增加FileGroups个数）可以增大Source的读取数据的能力。例如：当某一个目录产生的文件过多时需要将这个文件目录拆分成多个文件目录，同时配置好多个Source 以保证Source有足够的能力获取到新产生的数据。
batchSize参数决定Source一次批量运输到Channel的event条数，适当调大这个参数可以提高Source搬运Event到Channel时的性能。
Channel
type 选择memory时Channel的性能最好，但是如果Flume进程意外挂掉可能会丢失数据。type选择file时Channel的容错性更好，但是性能上会比memory channel差。
使用file Channel时dataDirs配置多个不同盘下的目录可以提高性能。
Capacity 参数决定Channel可容纳最大的event条数。transactionCapacity 参数决定每次Source往channel里面写的最大event条数和每次Sink从channel里面读的最大event条数。transactionCapacity需要大于Source和Sink的batchSize参数。
Sink
增加Sink的个数可以增加Sink消费event的能力。Sink也不是越多越好够用就行，过多的Sink会占用系统资源，造成系统资源不必要的浪费。
batchSize参数决定Sink一次批量从Channel读取的event条数，适当调大这个参数可以提高Sink从Channel搬出event的性能。

#### 19.Flume的事务机制

Flume的事务机制（类似数据库的事务机制）：Flume使用两个独立的事务分别负责从Soucrce到Channel，以及从Channel到Sink的事件传递。比如spooling directory source 为文件的每一行创建一个事件，一旦事务中所有的事件全部传递到Channel且提交成功，那么Soucrce就将该文件标记为完成。同理，事务以类似的方式处理从Channel到Sink的传递过程，如果因为某种原因使得事件无法记录，那么事务将会回滚。且所有的事件都会保持到Channel中，等待重新传递。

#### 20.Flume Event 是数据流的基本单元 

它由一个装载数据的**字节数组(byte payload)**和一系列可选的字符串属性来组成(可选头部).

![img](https://img-blog.csdnimg.cn/20190121101228568.png)

#### 参考链接

https://www.pianshen.com/article/46741071230/

https://blog.csdn.net/shujuelin/article/details/89020156

https://www.kanzhun.com/jiaocheng/502650.html

https://coding.imooc.com/learn/questiondetail/208640.html

https://blog.csdn.net/wwwzydcom/article/details/102981594