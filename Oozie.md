## oozie

* [1.oozie 是什么](#1oozie-是什么)
* [2.三个主要概念](#2三个主要概念)
* [3.Workflow](#3workflow)
* [4.Coordinator](#4coordinator)
* [5.Bundle](#5bundle)
* [6.oozie各个组件之间的关系](#6oozie各个组件之间的关系)
* [7.节点类型](#7节点类型)
* [8.  流程控制节点](#8--流程控制节点)
* [9.动作节点](#9动作节点)
* [10.Oozie Cli命令 启动任务](#10oozie-cli命令-启动任务)
* [12.Oozie Cli命令 停止任务](#12oozie-cli命令-停止任务)
* [13.Oozie Cli命令 提交任务](#13oozie-cli命令-提交任务)
* [14.Oozie Cli命令 开始任务](#14oozie-cli命令-开始任务)
* [15.Oozie Cli命令 查看任务执行情况](#15oozie-cli命令-查看任务执行情况)
* [参考链接](#参考链接)


#### 1.oozie 是什么

oozie 是一个定时调度工具

oozie本质就是一个作业协调工具（底层原理是通过将xml语言转换成mapreduce程序来做，但只是在集中map端做处理，避免shuffle的过程。），所以我对它的暂时的定位就是会用，能解决问题就行，暂时没有进行深入研究。

#### 2.三个主要概念

分别是workflow，coordinator，bundle。

#### 3.Workflow

​    工作流，由我们需要处理的每个工作组成，进行需求的流式处理。

#### 4.Coordinator

​    协调器，可以理解为工作流的协调器，可以将多个工作流协调成一个工作流来进行处理。

#### 5.Bundle

​    捆，束。将一堆的coordinator进行汇总处理。

#### 6.oozie各个组件之间的关系

![img](https://img-blog.csdn.net/20180301151049612?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VpeGluXzM5MTk4Nzc0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#### 7.节点类型

     Oozie的节点分成两种，流程控制节点和动作节点。所谓的节点实际就是一组标签。两种节点分别如下：

#### 8.  流程控制节点

u  <start />——定义workflow的开始

u  <end />——定义workflow的结束

u  <decision />——实现switch功能

<switch><case /><default /></switch>标签连用

u  <sub-workflow>——调用子workflow

u  <kill />——程序出错后跳转到这个节点执行相关操作

u  <fork />——并发执行workflow

u  <join />——并发执行结束（与fork一起使用）


#### 9.动作节点

u  <shell />——表示运行的是shell操作

u  <java />——表示运行的java程序

u  <fs />——表示是对hdfs进行操作

u  <MR />——表示进行的是MR操作

u  <hive />——表示进程的是hive操作

u  <sqoop />——表示进行的是sqoop的相关操作


#### 10.Oozie Cli命令 启动任务

```
oozie job -oozie oozie_url -config job.properties_address-run
```



#### 12.Oozie Cli命令 停止任务

```
oozie job -oozie oozie_url -kill jobId -oozie-oozi-W


```

#### 13.Oozie Cli命令 提交任务

```
oozie job -oozie oozie_url -config job.properties_address -submit

```

#### 14.Oozie Cli命令 开始任务

```
oozie job -oozie oozie_url -config job.properties_address -startJobId -oozie-oozi-W
```

#### 15.Oozie Cli命令 查看任务执行情况

```
oozie job -oozieoozie_url -config job.properties_address -info jobId -oozie-oozi-W


```



#### 参考链接

https://www.jianshu.com/p/877bbcae7dad

https://blog.csdn.net/weixin_39198774/article/details/79412726