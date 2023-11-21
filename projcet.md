1.自研规则引擎  
2.架构升级：单机->集群模式->负载均衡->服务拆分。  
  读写分离, 分库分表
  异步处理 
3.流程性能优化，原来处理流程35S，优化到10S内 
4.服务端进件流程优化。

异步编程提升性能

日志链路追加
配置中心引入

你遇到什么生产事故，如何排查解决的。

- 慢SQL，导致CPU升高
- 数据库连接池配置问题 导致吞吐量上不去
- 线程池问题？
- MQ



todo



本周

- [ ] Java编程之美  
- [ ] 源码解析
  - [ ] 并发
  - [ ] Spring
  - [ ] Kafka
- [ ] 性能优化

结合具体的项目经验

https://articles.zsxq.com/id_izrmpuk43owr.html

https://wx.zsxq.com/dweb2/index/topic_detail/185425252544152





排查工具

JVM(java 、Arthas、MAT、JDK)

- OOM

MySQL

- 慢SQL
- 连接池配置

并发

- 线程池

Redis

Kafka

- 消息延迟和堆积

网络
