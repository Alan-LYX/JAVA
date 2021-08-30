# JAVA

Java from beginner to master.



## 一、编程基础

- **Java语言**
  - 语言基础
    - [x] 基础语法、分支结构if/switch、循环结构for/while/do while
    - [x] 面向对象、封装、继承、多态、构造器、包、super、this
    - [x] Object类、抽象类、异常、接口、内部类、枚举、注解
    - [x] 集合List/Set/Map、泛型、线程的创建和启动、反射、注解、I/O
    - [x] Lambda表达式、方法引用、构造器引用、StreamAPI
    - [ ] 图形化（Swing）
  - 并发/多线程
    - [x] 并发基础、线程池、锁
    - [ ] 并发容器、原子类、JUC并发工具类
  - JVM
    - [x] 类加载机制、JVM内存模型
    - [ ] 字节码执行机制、GC垃圾回收、JVM性能监控与故障定位、JVM调优
- **数据结构和算法**
  - 数据结构
    - [x] 字符串、数组、队列、栈、链表、树、堆、图、哈希
  - 算法
    - [x] 顺序查找、折半查找、分块查找、KMP、广度/深度优先搜索
    - [x] 插入排序、交换排序、选择排序、归并排序、最短路径、拓扑排序、关键路径
    - [ ] 贪心、分治、动态规划、回溯
- **计算机网络**
  - [x] TCP/UDP协议、Cookie/Session
  - [ ] ARP协议、IP/ICMP协议、DNS/HTTP/HTTPS协议
- **数据库/SQL**
  - [x] DML语言、DDL语言、DCL语言
  - [x] 分组查询、Join查询、子查询、Union查询、函数
  - [x] 流程控制语句、事务的特点、事务以及隔离级别
  - [ ] 索引和优化、存储引擎、锁机制、高可用设计、集群
  - [ ] 分库分表、主从复制、视图、存储过程、触发器、自定义函数
  - [x] JDBC增删改查、批处理、连接池的原理及应用
  - [x] 常见数据库连接池C3P0、DBCP、Druid等
- **操作系统**
  - [ ] 进程/线程、并发/锁、内存管理和调度、IO原理
- **设计模式**
  - [x] 单例、工厂、代理
  - [ ] 策略、模板方法、观察者、适配器、责任链、建造者



## 二、研发工具

- **集成开发环境**
  - [x] Eclipse、Intellij IDEA、VS code
- **Linux系统**
  - [ ] Linux系统基础、Linux网络基础、在Vmware下的安装、Linux下Java环境的搭建
  - [ ] linux常用命令、基本Shell脚本
- **代码管理工具**
  - [ ] Git、SVN
- **项目管理/构建工具**
  - [ ] Maven、Gradle



## 三、应用框架

- **前端**
  - **基础套餐**
    - 三大件
      - [x] HTML、CSS、JavaScript
    - 基础库
      - [x] jQuery、Ajax、Json
  - **模板框架**
    - [x] Servlet、JSP、JSTL、EL、Cookie、Session
    - [x] Filter&Listener、文件的上传下载
    - [ ] Thymeleaf、FreeMarker
  - **组件化框架**
    - [ ] Node、Vue、Reack、Angular
  
- **后端**
  - **数据库**
    - ORM层框架
      - [x] MyBatis
      - [ ] MyBatis-plus、Hibernate、JPA
    - 连接池
      - [x] C3P0、Druid、DBCP
      - [ ] HikaiCP
    - 分库分表
      - [ ] MyCat、Sharding-JDBC、Sharding-Sphere
  - **Spring家族**
    - [x] Spring、Spring MVC
    - [ ] Spring Boot
  - 服务器软件
    - Web服务器
      - [ ] Nginx
    - 应用服务器
      - [x] Tomcat
      - [ ] Jetty、Undertow
  - **中间件**
    - 缓存
      - [ ] Redis、memcache
    - 消息队列
      - [ ] RocketMQ、RabbitMQ、Kafka
    - RPC框架
      - [ ] Spring Cloud、Dubbo、gRPC、Thrift、Netty
  - 搜索引擎
    - [ ] ElasticSearch、Solr
  - **分布式/微服务**
    - 服务发现/注册
      - [ ] Eureka、Consul、Zookeeper、Nacos
    - 网关
      - [ ] Zuul、Gateway
    - 服务调用（负载均衡）
      - [ ] Ribbon、Feign
    - 熔断/降级
      - [ ] Hystrix
    - 配置中心
      - [ ] Config、Apollo、Nacos
    - 认证和鉴权
      - [ ] Spring Security、Shiro、OAuth12、SSO
    - 分布式事务
      - [ ] JTA接口——Atomikos组件
      - [ ] 2PC、3PC
      - [ ] XA模式
      - [ ] TCC模式——tcc-transaction、ByteTCC、EasyTransaction、Seata
      - [ ] SAGA模式——SerivceComb、Seata
      - [ ] LCN模式——tx-lcn
    - 任务调度
      - [ ] Quartz、Elastic-Job
    - 链路追踪与监控
      - [ ] Zipjin、Sleuth、Skywalking
    - 日志分析与监控
      - [ ] ELK（ElasticSearch、Logstash、Kibana）
  - **虚拟化/容器化**
    - 容器技术
      - [ ] Docker
    - 容器编排技术
      - [ ] Kubernetes、Swarm



## 四、运维知识

- Web服务器——Nginx
- 应用服务器——Tomcat/Jetty/Undertow
- CDN加速
- 持续集成/持续发布——Jenkins
- 代码质量检查——sonar
- 日志收集/分析——ELK



## 五、手撕源码

