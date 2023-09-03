# skill-tree

本质是通过将简要知识串联起来，形成自己的一个技能树、融会贯通。

[TOC]

# **一.基础学科**
## 操作系统
进程管理

- 进程&线程&协程
- 进程/线程通信方式
- PCB
- 进程状态
- 调度算法


内存管理

-  内存碎片

- 内存管理方式

- 虚拟内存

- 分段机制

- 分页机制

- 段页机制

- 局部性原理

  局部性原理分为时间局部性和空间局部性，本质是为了硬件成本与文件读写速度之间的平衡，将高频热点数据存储在高速缓存中。以此提升整体的性能。但是本身也引入了缓存，需要注意缓存命中率与淘汰策略。

- 用户态与内核态、系统调用、实模式与保护模式

文件系统

- 硬连接、软链接
- 磁盘调度算法

网络通信

- 套接字 socket

死锁

- 死锁
- 产生条件
- 解决方案、模拟死锁

性能优化

- 零拷贝 / pageCache / 异步IO、直接IO

  零拷贝是通过减少用户态到内核态数据的拷贝次数，以及提升数据的复制速度，文件从磁盘到内核缓冲区到网络socket缓冲区。

  而pageCache是操作系统为了提升文件到读写，会先从pageCache中查询数据，如果有直接返回，没有再从磁盘读取数据，而写的过程也是一样的，写入pageCache，然后在同步回磁盘，以及提升读写性能。

  异步IO、直接IO是为了解决大文件在零拷贝下的瓶颈，而推荐使用零拷贝。
  
- COW (Cop On Write)

- IO多路复用

## 网络协议

网络基础

- 网络分层结构
- 常见网络协议

物理层链路层

- MAC
- 交换机与VlAN
- ICMP与PING
- 网关
- 路由协议

传输层

- TCP
- UDP
- 套接字Socket

应用层

- HTTP
- HTTPS
- 流媒体
- P2P

数据中心

- DNS
- HTTPDNS
- CDN
- 数据中心
- VPN
- 移动网络

云计算网络

容器网络

## 数据库

ACID、三范式

索引

锁

事物和隔离级别

存储引擎

日志

SQL优化

读写分离（主从复制）

分库分表

高可用

​    高可用依赖的是数据复制，数据复制复制就是从一个库备份数据，然后恢复到另一个库中

- 数据备份：

  全量备份 mysqldump -u root -p test > test.sql

  增量备份 binlog

- MySQL HA

  | 方案                              | 高可用 | 可能丢数据 | 性能 |
  | --------------------------------- | ------ | ---------- | ---- |
  | 一主一从<br /> 异步复制、手动切换 | 否     | 可控       | 好   |
  | 一主一从<br />异步复制、自动切换  | 是     | 是         | 好   |
  | 一主二从 <br />同步复制、自动切换 | 是     | 否         | 差   |

## 数据结构与算法
### 数据结构

数组

链表

- 跳表

栈

- 单调栈

队列

树

- BST
- Trie
- 红黑树
- B树
- B+树

哈希表

字符串

图

布隆过滤器

堆

### 算法

排序

- 快排

二分

搜索

- BSF
- DFS
- 回遡

贪心算法

动态规划

位运算

双指针

## 软件工程
## Linux 
## 程序员素养/数学
## 密码学
# 二.编程语言
## Java

集合类

字符串

OOP

关键字

异常

IO

范型

反射

序列化

java8

注解

枚举

## Go
## python
## JVM

类加载器/流程

字节码技术

JVM对象分配回收策略

运行时数据区

垃圾回收算法

垃圾收集器

JVM调优

## 并发编程
并发基础

- 进程&线程
- 线程状态 
- 线程创建

互斥同步

- synchronized
  - 锁升级
- ReentrantLock

线程协作/通信

- join
- wait/notify/notifyAll
- awit/singal/singalAll
- LockSupport

AQS

- AQS源码解析整体流程

  大概的整体流程是这样的，首先，我们创建三个线程A,B,C。线程A先拿到锁，执行任务。 而B、C一直获取不到锁，不能执行任务。被park。等A指向完毕之后，unpark(B);

  lock.lock() 线程A通过CAS 设置上锁。而等线程B去获取锁的时候，CAS获取不到锁。于是进入acquire(1)进入nonfairTryAcquire 再次尝试获取锁，获取不到、直接返回 false。进入addWaiter()将当前节点添加到队列中enq(node)，因为t==null ，所以先创建一个哨兵结点。然后第二次自旋，将当前节点Node(ThreadB)，添加到队列中。调用 acquireQueued() ，拿到当前节点的前置节点。第三次获取，获取不到。进入 park() 等待。等待线程释放锁， unpark() 操作。而在此时，线程A执行任务完毕，进行 lock.unlock() 操作。执行 release(1) ,通过head节点将下一个节点进行 unpark() 操作。而因为线程B被park()了，所以下一次就可以获取到锁，将队列中的哨兵结点进行修改。

工具类

- CountDownLatch
- CyclicBarrier
- Semaphore

JMM

线程安全

锁

线程池原理

threadlocal原理，应用场景

## 网络编程

# 三.开发框架
## web框架
## ORM
## Spring

Spring三级缓存

Bean生命周期

bean初始化流程

AOP原理

IOC原理

Spring MVC原理

Spring中设计模式

## Spring-cloud

Nacos

OpenFeign

Gateway

sentinel

# 四.中间件
## RPC/注册中心

Zookeeper

## 网关/代理

Nginx

## NoSQL
## 缓存
### Redis

![](https://img-blog.csdnimg.cn/a0b5b20bd1224d06814f1410019461c8.png)

Redis基本命令

底层数据结构

Redis高性能IO模型

IO多路复用

持久化

- AOF
- RDB

双写一致性

雪崩、击穿、穿透、预热

布隆过滤器

分布式锁

高可用

Redis数据同步、复制

Redis哨兵机制

Redis哨兵集群

Redis切片集群

## 消息队列

消息队列作用

- 异步、削峰、解耦、提升写性能

带来的问题

- 消息延迟、系统的复杂度、数据不一致（消息顺序、消息丢失、重复消费、消息挤压、高可用、高性能）

### Kafka

Kafka基础架构

- Broker->Topic->Partition->Partition Leader-> Partition Follower 
- 部署架构：单机模式/集群模式

分区机制

- 作用提升负载均衡、可伸缩能力、提升系统处理写性能
- 策略：随机、轮询、按消息键保序、其他策略

消息挤压如何处理

- 优化性能避免消息挤压
  - 发送端性能优化
    - 准备数据、序列化数据、之前的耗时
    - 发送消息和返回响应网络传输中的耗时
    - Broker处理消息的时延
  - 消费端性能优化
    - 优化消费端程序业务逻辑性能
    - 水平扩容，增加消费端并发数据。加机器，分区。
- 发送快了，消费变慢了
  - 监控、紧急消费端扩容、关闭上游系统功能、错误日志、同一条消息反复消费，拖垮整个系统

消息重复消费

- 生产者幂等、消费者幂等

消息可靠传输

- 发送、存储、消费三个阶段

生产者消费流程

Kafka如何实现高性能

- 批量发送、顺序读写提升磁盘IO性能、PageCache加速消息读写、零拷贝技术

**Kafka消费者分区分配和重平衡 ⭐️**

消费者位移

副本机制

Kafka多线程消费

Kafka高水位和Leader Epoch原理

### RabbitMQ

## 搜索引擎 
 ### ES

## 配置中心

### Apollo
### XDmond
## 定时

### XXL-JOB

## 监控/报警/日志

## 链路

### FalCon

# 五.软件设计

![img](https://static001.geekbang.org/resource/image/f3/d3/f3262ef8152517d3b11bfc3f2d2b12d3.png)

## 编程范式

面向过程

面向对象

- 封装、抽象、继承、多态
- 接口 VS 抽象类

## 规范

编码规范

## 重构

- 单元测试
- 可测试性
- 大重构
- 小重构

## 设计原则
单一职责原则-SRP

开闭原则-OCP

里氏替换原则-LSP

接口隔离原则-ISP

依赖倒置原则-DIP

迪米特法则

DRP原则

KISS原则

YAGNI原则

LOD原则

## 设计模式

### 创建型

单例模式

工厂模式

建造者模式

原型模式

### 结构型

代理模式

桥接模式

装饰者模式

适配器模式

门面模式

组合模式

享元模式

### 行为型

观察者模式

模板模式

策略模式

职责链模式

迭代器模式

状态模式

访问模式

备忘录模式

命令模式

解释器模式

中介模式

## 源码解析

TomCat源码解析

## DDD&MVC
# 六.架构设计

架构设计三原则、简单、合适、演化。**面向复杂度设计**
![img](https://img-blog.csdnimg.cn/a2e7e82a740c49d79c0d5f1d50cae3d4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAcXhseGk=,size_20,color_FFFFFF,t_70,g_se,x_16)
![img](https://img-blog.csdnimg.cn/2ab8c7fae2894781899ceac595beec6e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAcXhseGk=,size_20,color_FFFFFF,t_70,g_se,x_16)
- ![image](https://github.com/qxlx/skill-tree/assets/36980092/df11e9cd-d3e5-4e49-93d8-4b72de63e0d2)
![image](https://github.com/qxlx/skill-tree/assets/36980092/b86f37cf-5a9f-4d37-a4e7-cab9bd595395)

## 微服务

## 分布式理论

BASE

CAP

ACID

FLP

两将军

拜占庭将军

数据一致性

![img](https://learn.lianglianglee.com/%E4%B8%93%E6%A0%8F/%E5%B7%A6%E8%80%B3%E5%90%AC%E9%A3%8E/assets/8958a432f32dd742b6503b60f97cc3f2.png)

## 共识算法

一致性与共识

Paxos

Raft

ZAB

Gossip

## 分布式计算

## 分布式存储

分布式ID

分片

复制

事务

- 2PC
- TCC
- 3PC
- XA

分布式锁

- Redis
- ZK
- MySQL

## 高性能架构

![img](https://learn.lianglianglee.com/%E4%B8%93%E6%A0%8F/%E5%B7%A6%E8%80%B3%E5%90%AC%E9%A3%8E/assets/a9edeae125a80f381003d8d9d0056317.png)

## 高可用架构

![img](https://learn.lianglianglee.com/%E4%B8%93%E6%A0%8F/%E5%B7%A6%E8%80%B3%E5%90%AC%E9%A3%8E/assets/befd21e1b41a257c5028f8c1bc7fa279.png)

接口级别：限流、降级、排队、熔断、

超时重试&幂等

异地多活：同城多机房、跨城多机房、跨国数据中心

存储高可用：复制（主从、主备、主主）

计算高可用：负载均衡、任务分配、任务分解

## 可拓展架构

分层

SOA

微服务

微内核

## 性能调优

性能指标

- TPS、QPS、吞吐量

架构设计

- 缓存、异步、集群、分片、复制
- 网络层面
- 服务器硬件层面
- 操作系统层面
- JVM性能优化
- 基础组件优化（MySQL、Kafka、Redis）
- 软件架构层面优化
- 软件代码优化

如何优化

- 三要三不要
  - 三要
    - 查询系统最大性能瓶颈
    - 确诊问题根本原因
    - 考虑多种情况
  - 三不要
    - 过度反常优化
    - 过早不成熟优化
    - 表面的肤浅优化
- 十大策略
  - 时空转换
    - 空间换时间
    - 时间换空间
  - 并行/异步
    - 并行操作
    - 异步操作
  - 预先/延后处理
    - 预先/提前处理
    - 延后/惰性处理
  - 缓存/批量合并
    - 缓存数据和结果
    - 合并和批处理
  - 算法设计和数据结构
    - 更快的算法设计
    - 更优化的数据结构

## 系统设计

# 七.编程工具

## git

## idea

## Maven

## gradle

# 八.云原生

## CI/CD

软件生命周期

CI/CD

Jenkins

## DevOps

## docker

虚拟化/容器化

基础命令

镜像

compose

container

network

image

volume

swarm

## k8s



# 九.大数据
  实时计算
  离线计算

## Hadoop

## Spark\Filnk

# 十.其他领域

## 区块链              

     操作系统
       https://segmentfault.com/a/1190000039774784 从根上理解用户态与内核态


​     
