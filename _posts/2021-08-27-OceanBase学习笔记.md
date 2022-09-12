---
title: OceanBase学习笔记
description: 本章是我因需学习OB时记录的内容，其主要工作就是将OBCA考试的PPT记录下来，将其中可能的重点标注，从而达到强化记忆的效果。OBCA 认证主要讲解 OceanBase 的发展历程、应用案例、产品架构、核心功能、部署安装等知识。帮助您理解多副本一致性协议、数据可靠及高可用、在线水平扩展、分布式事务等 OceanBase 的重要特性。OBCA 认证主要面向具备 IT 通用基础能力的学员，了解至少一门关系型数据库（MySQL 或者 Oracle），对分布式系统或分布式事务有基本了解，适合初级数据库管理员，初级应用开发人员，合作伙伴驻场服务人员等。如果对各位有所帮助，可以点赞打赏一下哟~
categories:
 - book
tags:
 - oceanbase
keywords:
  - oceanbase
  - 数据库
  - 学习
  - OCBA
  - OCBP
  - 考试
  - 笔记
  - 重点
updated: 2022-09-12 15:17:00
---


## 关于本书

本章是我因需学习OB时记录的内容，其主要工作就是将OBCA考试的PPT记录下来，将其中可能的重点标注，从而达到强化记忆的效果。
如果对各位有所帮助，可以点赞打赏一下哟~
> 本章部分内容可能摘自其他博客，已标明出处，凡未声明标注的图片均来自于OceanBase培训视频截图。

## 简介

OBCA 认证主要讲解 OceanBase 的发展历程、应用案例、产品架构、核心功能、部署安装等知识。帮助您理解多副本一致性协议、数据可靠及高可用、在线水平扩展、分布式事务等 OceanBase 的重要特性。OBCA 认证主要面向具备 IT 通用基础能力的学员，了解至少一门关系型数据库（MySQL 或者 Oracle），对分布式系统或分布式事务有基本了解，适合初级数据库管理员，初级应用开发人员，合作伙伴驻场服务人员等。

## 成果

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121527367.jpeg)

## 1.1 传统数据库的挑战

### 1.1.1 数据库是核心的IT基础设施

数据库应用场景：

- 金融
  - 互联网业务增长，带动核心系统升级
  - 核心系统引入数据库分布式与云化改造，支撑横向平滑扩展
- 运营商
  - 5G规模推广，带动IT系统升级
  - 5G具备大带宽和超低延时能力，需要数据库系统提升响应速度和并发能力
- 公用事业
  - 打造智慧政府
  - 实现智慧政府为目标的“互联网+”业务构建，对于数据库的性能和拓展提出了更高的要求

### 1.1.2 传统集中式数据库面临的挑战

- 优势
  - **成熟稳定**：发展时间长
  - **行业适配性强**：适配不同行业的各种需求
  - **生态完善**：拥有大量的ISV应用开发商和技术开发者等
- 劣势
  - **成本高**：售价高，软件硬件成本高，**CAPEX**（资本性支出，资金或固定资产、无形资产、递延资产等资产的投入）和**OPEX**（企业的管理支出、办公室支出、员工工资支出和广告支出等日常开支。包括维护成本、营销费用、人工成本和折旧）成本高。
  - **无法横向扩展**：容易碰到单点上限。

### 1.1.3 使用数据库中间件的分库分表方案依然有短板

利用数据库中间件可实现数据库的**线性**扩容。
数据库间单点存在，无互相感知，依赖中间件实现跨库事务。

- 优势
  - **线性扩展**：利用分库分表可快速实现数据库水平扩展
  - **技术成本低**：对数据库引擎改动小
- 劣势
  - **跨库分布式事务**：中间件难以做到RPO=0，遇到异常时无法保证分布式事务的ACID能力。

> RPO (Recovery Point Objective，复原点目标)：[是指从系统和应用数据而言，要实现能够恢复至可以支持各部门业务运作，恢复得来的数据所对应时的间点](https://zhidao.baidu.com/question/1706463534212352700.html)，即**“发生错误到有效解决错误的时间”**
> ACID：[是指数据库管理系统（DBMS）在写入或更新资料的过程中，为保证事务（transaction）是正确可靠的，所必须具备的四个特性：原子性（atomicity，或称不可分割性）、一致性（consistency）、隔离性（isolation，又称独立性）、持久性（durability）](https://baike.baidu.com/item/ACID/10738)
> RTO (Recovery Time Objective，复原时间目标)：[是指灾难发生后，从IT系统当机导致业务停顿之时开始，到IT系统恢复至可以支持各部门运作、恢复运营之时，此两点之间的时间段称为RTO。比如说灾难发生后半天内便需要恢复，RTO值就是十二小时](https://zhidao.baidu.com/question/1706463534212352700.html)；

- **全局一致性**：因多数据库时间戳的不一致性导致的多个数据版本号的不一致性（无法保证全局一致性）
- **负载均衡**：扩容和缩容时，底层数据库引擎无法在线调整数据分布规则。
- **跨库复杂SQL**：跨库的复杂SQL只能在中间件完成，但会影响分布式并行计算能力，可能会产生业务入侵。

## 1.2 分布式数据库的基本特点及对比分析

### 1.2.1 原生的分布式关系数据库架构

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121529041.png)

- 优势
  - **数据高可靠+服务高可用**：多副本一致性协议Paxos的工业级实现，个别节点发生故障时保证数据零丢失（RPO=0)和服务快速恢复（RTO<30秒）

> [Paxos协议是一种基于消息传递且具有高度容错特性的一致性算法，是目前公认的解决分布式一致性问题最有效的算法之一，其解决的问题就是在分布式系统中如何就某个值（决议）达成一致 。](https://zhuanlan.zhihu.com/p/361108372)

- **线性扩容**：根据业务扩容缩绒
- **低成本**：普通x86服务器即可保证高可用，无需小型机和存储
- **全局一致性**：支持分布式事务，确保全局一致性，支持分布式复杂查询
- **灵活部署方式**：支持三中心、五中心、主备等多种部署模式
- **对业务透明**：业务系统可像单点数据库一样使用。业务改造成本低

### 1.2.2 OB和传统数据库的对比

|                                                                    | 传统集中式数据库                                | 分布式数据库         |
| ------------------------------------------------------------------ | ----------------------------------------------- | -------------------- |
| 产品架构                                                           | 单点集中式架构，全共享架构。                    |
| 构建于高端硬件上。                                                 | 采用Paxos分布式一致性协议，基于普通PC硬件的设计 |
| 数据可靠性和服务高可用性                                           | 利用高端硬件设备保证数据可靠性。                |
| 采用“主从复制”，主节点故障下会有数据损失（RPO>0)，RTO以小时计      | 利用Paxos保证数据可靠性。                       |
| 主节点故障情况下，保证数据无损（RPO=0)并自动选举恢复服务，RTO<30秒 |
| 扩展性                                                             | 单点纵向扩展，横向扩展范围有限                  | 横向扩展，受限于带宽 |
| 应用场景                                                           | 企业（金融、电信、政企等）的核心系统            |
| 无法应付互联网业务场景                                             | 支付宝、网上银行等                              |
| 使用成本                                                           | 昂贵                                            |
| 软硬件和日常维护                                                   | 相对较低                                        |

## 2.1 发展历程及产品简介

### 2.1.1 发展历程

略，自己找吧

### 2.1.2 OB核心特征

- 高扩展
  - 水平扩展
  - 按需在线扩缩容，不停服务
  - 单集群突破100台服务器
- 高性能
  - 准内存处理性能
  - 峰值6100万次/秒
  - 单表最大3200亿行
- 高可用
  - 给予Paxos协议，强一致性
  - RPO=0; RTO<30s
  - 少数副本故障，数据不丢，服务不停
- 高兼容
  - Oracle/Mysql两种模式，降低业务迁移改造成本
- 多租户
  - DBaaS架构
  - 资源隔离
  - 自动负载均衡
- 高透明
  - 全局一致性快照
  - 全局索引
  - 自动事务**两阶段提交**

> [DBaaS 是一种云模型，它允许用户通过使用自助服务供应框架从预定义的服务目录中进行选择来请求数据库环境。这些数据库云的主要优势是敏捷性以及加快部署数据库服务。在供应和取消供应数据库时，会占用和释放相关的计算资源。数据库资源可以被某个项目占用一段时间，项目结束后会被自动取消供应，返回到资源池中。可以针对用户跟踪计算成本并对其收费。](https://www.oracle.com/technetwork/cn/database/dbaas-guide-2301319-zhs.pdf)

OB是100%知识产权的产品，不涉及任何开源数据库项目。

## 2.2 TPC-C认证成果

略，不感兴趣

## 2.3 内外部应用案例

- 支付宝
  - 2019双十一6100万次/秒
- 淘宝
  - 日活用户3亿，月活用户8亿
- 网商银行
  - 交易和灾备
- paytm
  - 。

## 3.1 产品家族及安装部署

### 3.1.1 产品家族

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121530466.png)
> 源自[https://www.oceanbase.com/training/Course?courseUniqueId=k8yjnbx1&lessonId=6](https://www.oceanbase.com/training/Course?courseUniqueId=k8yjnbx1&lessonId=6)

- ODC 面向开发者工具平台
- OCP 面向运维者工具平台
- ODP 数据中间件
- OMS 数据库迁移平台
- OB数据库内核

### 3.1.2 部署形式

- 云部署
  - 公有云
  - 专有云
- 独立部署
| 服务器类型    | 数量 | 功能最低配置     | 性能最低配置              |
| ------------- | ---- | ---------------- | ------------------------- |
| OCP管控服务器 | 1    | 32C，128G，1.5TB | 32C，128G，1.5TBSSD，万兆 |
| OB服务器      | 3    | 32C，128G，1.2TB | 32C，256G，2TBSSD，万兆   |

### 3.1.3 支持CPU/OS

- CPU
  - X86
  - 海光
  - 海思
  - 飞腾
- OS
  - CentOS
  - Red Hat
  - SUSE
  - Debian/Ubuntu
  - AliOS
  - 中标麒麟
  - 银河麒麟

### 3.1.4 OB部署流程

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121534705.PNG)

## 3.2 基础概念

### 3.2.1 OB 基本概念介绍

#### 管理员视角

由大到小：集群，Zone，OBServer，租户
![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121535816.png)

> 引用自[https://www.oceanbase.com/training/Course?lessonId=7](https://www.oceanbase.com/training/Course?lessonId=7)

其中：

- 一个集群有多个zone，给一个集群的机器打上相同tag，就属于同一个zone；zone>=3（建议奇数）；每个zone有且只有一份完整副本；每个zone存在一台OBserver作为 RootService总控服务
- Zone可以理解成逻辑块，但在物理上无法直接定义。
- OBserver相对独立，有独立的计算和存储引擎
- 租户：类似 “实例”
- 每个租户有自己的资源池（CPU、内存）用户名、密码
- 租户之间的数据严格隔离；租户内可创建专属的数据库、表、执行DML操作
- 逻辑上类似于数据库实例，但物理上租户并没有自己的专属进程
- 使用OceanBase集群资源的第一个步骤
- 每个Zone有多台PC服务器构成。

> 上述引用自：[https://www.cnblogs.com/snail-z/p/14122013.html](https://www.cnblogs.com/snail-z/p/14122013.html)

### 3.2.2 RootService总控服务（RS）

- 每个Zone都有一个OBServer做总控服务。

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121536434.png)

- 总控服务
  - OB核心模块，管理整个集群
  - 集群内置服务，无需额外软硬件部署
  - 自带高可用能力，无单点故障风险
- 核心功能
  - 系统初始化(BootStrap)；系统元数据管理
  - 资源分配及调度：分区及副本管理、动态负载均衡、扩容/缩容等。
  - 全局DDL；集群数据合并

### 3.2.3 多租户机制，资源隔离，数据隔离

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121536242.png)
说明：

- 将数据库集群按指定规格（CPU、内存、存储、TPS、QPS）划分成多个资源池，分配给不同租户；
- 租户资源隔离策略：内存物理隔离；CPU逻辑隔离，数据隔离；
- 一般一个应用占用一个租户。

> [TPS：Transactions Per Second，意思是每秒事务数，具体事务的定义，都是人为的，可以一个接口、多个接口、一个业务流程等等。](https://www.cnblogs.com/uncleyong/p/11059556.html)
> [QPS：Queries Per Second，意思是每秒查询率，是一台服务器每秒能够响应的**查询**次数（数据库中的每秒执行查询sql的次数）](https://www.cnblogs.com/uncleyong/p/11059556.html)

- 租户类似数据库的实例，需要指定占用的资源

租户特性

- **可以创建自己的用户**（不同的用户名和密码）
- **可以创建数据库（db)、表**等所有客体对象
- **有自己独立的系统数据库**
- **有自己独立的系统变量**
- 数据库实例所具有的其他特性

### 3.2.4 每个租户拥有若干资源池（Resouce Pool）

OB可以为不同类型的应用分配不同类型和不同数量的Unit来满足不同的需求。资源并不是静态的，后续可根据业务的发展不断调整（升配或者降配）。

**Unit**
每个Unit描述了位于一个Server上的一组计算机和存储资源，每个Unit只能属于一个租户；
每个Unit可以理解为一个轻量级虚拟机，包括若干CPU资源、内存资源及磁盘资源等。

**租户资源池**
一个租户拥有若干个资源池。这些资源池的集合描述了这个租户所能使用的所有资源。
一个租户在同一个Server上最多有一个Unit。实际从概念上讲，副本是存储在Unit中，Unit是副本的容器。

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121536913.png)

## 3.3 试验

```python
# 创建租户步骤
# 1、创建“资源单元规格”，定义规格，不实际分配
create resource unit unit1
    max_cpu = 4,
    max_memory = 10737418240, -- 10GB
    min_memory = 10737418240, -- 10GB
    max_iops = 1000,
    min_iops = 128,
    max_session_num = 300,
    max_disk_size = 21474836480 -- 20GB
    ;
# 2、创建“资源池”
create resource pool pool1
    unit = 'unit1',
    unit_num = 1,
    zone_list = ('zone1', 'zone2', 'zone3')
    ;
    <font color='red'>每个OBserver只能有一个 unit</font>；如果 unit_num > 1，每个zone内必须有对应数目的机器；
    zone list一般与 zone个数保持一致；
    
# 3、创建租户
create tenant mysql_tenant
    resource_pool_list = ('pool1'),
    primary_zone = 'zone1, zone2, zone3'
set ob_tcp_invited_nodes = '%', ob_compatibility_mode = 'mysql', recyclebin = off, ob_timestamp_service = 'GTS'
   primary zone:指定主副本分配到zone的优先级，逗号两侧优先级相同，分号左侧优先级高于右侧
   需要指定租户类型是mysql还是oracle
```

```python
# 检查所有服务器状态
select * from __all_server \G
# 查看集群中整体资源分配的情况
__all_virtual_server_stat
# 查看unit
select  * from __all_unit_config;
# 查看已经分配的resource unit
select * from __all_unit;
# 将resource unit和具体的租户对应起来
select * from __all_resource_pool;
```

### 日志

- observer.log:observer运行时的日志文件
- rootservice.log:observer上rootserver的日志文件
- election.log:observer上选举模块的日志文件
- OB proxy：/opt/xx/install/obproxy-/log/obproxy..log

### 开启日志循环

```python
enable_syslog_recycle = True;
max_syslog_file_count = <count>; // count指的最终保存的日志数量
```

### 日志级别

默认 info，级别超过 warn，会自动生成files保留下来

```python
syslog_level = [debug, trace, info(默认), warn, user_err, error]
```

## 4.1 Paxos 协议与负载均衡

### 4.1.1 数据分区与分区副本

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121537288.png)

#### 分区

- 当一个表很大的时候，可以水平拆分为若干个分区，每个分区包含表的若干记录。根据行数据到分区的映射关系不同，分为hash分区,List分区（按列表），range分区（按范围）等。
- 每个分区还可以用不同的维度再分若干分区，称为二级分区
- 分区是OB数据框架的基本单元，是传统数据库的分区表在分布式系统上的实现。

#### 副本

- 为了数据安全和提供高可用的数据服务，每个分区的数据在物理上存储多份，每一份叫做分区的一个副本。
- 副本根据负载和特定的策略，由系统自动调度分散在多个Server上。副本支持迁移、复制、增删、类型转换等管理操作。

默认情况下，一个Zone只包含一个副本。

### 4.1.2 副本构成

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121537414.png)

-

### 4.1.3 副本类型

根据存储数据种类不同，副本有几种不同的类型，以支持不同业务在**数据安全**、**性能伸缩性**、**可用性**、**成本**等之间进行取舍。

- **全能型副本**：拥有事务日志，MemTable和SSTable等全部完整的数据和功能，它可以随时快速切换为leader对外提供服务。
- **日志型副本**：只包含日志的副本，没有MemTable和SSTable，参与日志投票并对外提供日志服务，可以参与其他副本的恢复，但不能变为主提供数据服务。其小号物力资源更少，能有效降低最后一副本机器的成本，从而降低整个集群的总体成本。
- **只读型副本**：包含完整日志，MemTable和SSTable等，但其日志特殊，不作为paxos成员参与日志投票，而是作为观察者实施追赶paxos成员的日志，并在本地回放。

性质：

- 一个分区在一个zone钟最多有一个全功能或日志型副本（可理解成只允许有一个带有可编辑日志的副本）
- 只读型副本在同一个zone可以有多个

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121537018.png)

### 4.1.4 多副本一致性协议

- 以分区为单位组建Paxos协议组：每个分区都有多份副本，自动建立Paxos组，在分区级用多副本保证数据可靠性和服务高可用，数据管理更加灵活方便；
- 自动选举主副本：OB自动生成多份副本，多副本自动选举主副本，主副本提供服务。

### 4.1.5 自动负载均衡与智能路由

- 自动负载均衡：主副本均匀打散到各个服务器中，使得各个服务器都能承载业务流量
- 每台OBServer相互独立：每台OBServer均可以独立执行SQL，如果应用需要访问的数据在不同机器上，OBServer自动将请求路由至数据所在机器，对业务完全透明。

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121538677.png)

### 4.1.6 通过多副本同步Redo Log确保数据持久化

- Paxos组成员通过Redo-Log的多数派强同步，确保数据的持久化
- Leader无需等待所有Follower的反馈，多数派完成同步即可向应用反馈成功

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121538364.png)

> 1. 应用写数据到P2分区，Zone2-OB Server1的P2为主副本（Leader），承接业务需求；
> 1. 将Redo-Log同步请求发送到Zone1-OB Server1和Zone3-OB Server1中的P2从副本（Follower）；
> 1. 任何一个Follower完成Redo-Log罗盘并将响应返回给Leader后，Leader即认为Redo-Log完成强同步，无需再等待其他Follower的反馈
> 1. Leader反馈应用操作完成

### 4.1.7 OB Proxy为应用提供智能路由服务，应用透明访问

#### 高效路由转发

- 对SQL做基本解析，确定对应Leader所在机器
- 反向代理，将请求路由至对应Leader；Leader位置无法确定时随机选择OBServer；
- 轻量SQL解析+快速转发，保证高性能，单OB Proxy单秒转发百万次请求

#### “非”计算节点，无状态

- 每个OB Proxy是一个“无状态”的服务进程，不做数据持久化，对部署位置无要求；
- OB Proxy不参与数据库引擎的计算任务，不参与事务（单机or分布式）处理；
- 不需要独立服务器，可以与OB Server共用一台服务器，如果应用对实时性要求高，可将OB Proxy部署到应用服务器中

### 4.1.8 通过设置Primary Zone，将业务汇聚到特定Zone

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121538921.png)

### 4.1.9 Primary Zone有租户、数据库和表不同的级别

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121539269.png)

- 如无特殊指定，自动继承上级对象的primary_zone：database继承租户，table继承database
- database和table可以指定各自的primary_zone

### 4.1.10 Table Group，将多个表的相同分区ID的主副本聚集在一个OBServer中，减少分布式事务引入的开销

避免Zone级别太大导致的不方便划分，可利用TableGroup将满足一定条件的表归属在同一个组中：

- 如果多个表的分区方式**完全相同**（分区类型、分区键个数、分区数量等），可以在**逻辑上**将这些表归属在同一个Table Group中，以影响动态负载均衡的策略。
- 同一个Table Group中的所有表，**分区ID（partition_id）**相同的所有分区，他们的leader在同一个observer上：在不影响全局负载均衡的前提下，可**有效减少分布式事务引入的跨机访问开销**。
- 如果负载均衡被打破（服务器故障、扩容缩绒等），Table Group中的所有表会作为一个整体来调整分区分布和Leader分布。
- 动态创建和删除，并且**对上层应用完全透明**。
- 如果**租户的unit_num=1且primary_zone只有一个zone**，则不需要table Group。

## 4.2 数据可靠及高可用

### 4.2.1 灾难恢复能力等级

系统发生故障（掉电、宕机、进程误杀等）时，业务如何考察系统的“高可用”能力

- **RTO(Recovery Time Objective)恢复时间目标**：在故障或灾难发生之后，数据库停止工作的最高可承受时间，在时限内恢复数据。**RTO=服务可用性**
- **RPO(Recovery Point Object)恢复点目标**：这是一个过去的时间点，当灾难或紧急事件发生时，数据可以恢复到的时间点，是业务系统所能容忍的数据丢失量。**RPO=数据可靠性**

OB的RPO=0，RTO<30秒。即6级

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121539101.png)

### 4.2.2 OB基于通用PC服务器提供高可用性

- OB运行在易损的PC服务器上，依靠自身软件保证高可用性；
- 硬件成本低
- 无需外挂独立存储，无需Raid

### 4.2.3 少数派故障时自动实现服务接管，保证数据高可用

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121539430.png)

#### 当Leader主副本故障

- 假设Zone3-OBS2故障，P7 Leader无法提供正常服务
- 位于ZONE1和Zone2的P7 Follower从副本重新选出新的Follower并接管业务；
- 整个过程无人工干预，自动完成
- Zone3-OBS2故障恢复后，P7副本首先追平数据，数据追平后继续参加Paxos组，**一般情况下，该P7副本还会是主副本（以实现负载均衡）**

#### 当Follower从副本故障

- 假设Zone3-OBS2故障，从副本（P5 P6 P8）本来就不承接业务，剩下的2个副本依然满足多数派，可正常提供业务，对业务无影响
- 待从副本所在数据库故障恢复后，追平数据后继续加入Paxos组后**依然是从副本**。

### 4.2.4 OBS进程异常后的处理策略

如果OBS进程异常终止，通过server_permanent_offline_time参数的值判定是否进行“临时下线”处理。
![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121539431.png)

### 4.2.5 OB容灾：同城三机房部署

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121539128.png)

- 同城3个机房组成一个集群（每个机房是一个Zone），机房间延迟一般在0.5-2ms之间；
- 机房级灾难时，剩余两份副本依然是多数派，依然可以同步Redo-Log，保证RPO=0；
- 但无法应对城市级的灾难；

### 4.2.6 OB容灾：三地五中心五副本

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121540583.png)

- 三个城市可以组成5副本集群
- 任何一个IDC活着城市故障，依然构成多数派，确保RPO=0
- 由于3份以上副本才能构成多数派，但每个城市最多只有2份副本，为降低时延，城市1和2应该距离近，以降低同步Redo-Log的时延。
- 为降低成本，城市3可以只部署日志型副本（只有日志）

### 4.2.7 OB容灾：同城两机房主备方案

同城三机房活三地五中心对基础设施要求太高，OB提供同城两机房和两地三中心两种方案
![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121540058.png)

- 每个城市都部署一个OB集群，一个为主，一个为备，每个集群都有自己单独的Paxos Group，多副本一致性
- “集群间”通过Redo-Log做数据同步，类似传统数据库的主从复制模式，有“异步同步”和“强同步”两种数据同步模式，类似ODD中的“最大性能”和“最大保护”两种模式。

### 4.2.8 OB容灾：两地三中心主备方案

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121541325.png)

- 主城市与备城市组成一个5分本的集群，任何IDC的故障最多损失2份副本，剩余的3份依旧满足多数派
- 备用城市建立一个独立的3副本集群，作为一个备集群，从主集群“异步同步”或“强同步”到备集群。
- 一旦主城市遭遇灾难，备城市可接管业务

### 4.2.9 小结

- OB可达到国际灾难恢复能力6级
- RTO、RPO显著优于传统主备数据库

### 4.2.10 动态水平扩展的场景

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121541827.png)

- 假设有一个3副本集群，每个Zone有3000个分区的副本，扩容后，OB自动将1500分区的副本迁移到新服务器中的资源Unit中。
- 借助阿里云的弹性资源，OB可以很好地帮助业务扩容和缩容，低成本的（短时间的公有云资源费用）应对大型促销等活动。

### 4.2.11 动态扩容和缩容技术实现

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121541264.png)
> 图中下面黄色底的OBS1可能应为OBS2

上图假设一个表有8个分区，扩容后，在每个Zone内，OB自动将4个分区的副本（包含主副本）迁移到新服务器的租户资源单元中，实现Zone内各个OBS的负载均衡。整个扩容是在线动态扩容，对业务无感知，可以降低运维难度。

步骤：

1. 购买资源：大促前，购买阿里云服务器等资源
1. 追平数据：新增服务器OBS2到集群，OB会自动选择一些副本，动态的由OBS1复制到OBS2中（存量复制+Redo-Log增量复制），这个过程对业务无影响。
1. 切换服务：数据追平后，OBS1的主副本停止服务，由OBS2中的新的主副本接管，删除OBS1中的旧副本，过程对业务无影响。
1. 缩绒：洪峰流量过后，做反向动作，将OBS2副本迁回OBS1，业务重新访问OBS1中的副本
1. 退回资源：释放阿里云资源

### 4.2.12 扩容基本步骤

1. 为每个Zone添加新的物理机器
1. 在每台新机器上正确启动OBS服务
1. 为每个新增加的OBS进程执行alter system add server;命令，将OBS服务添加到集群
1. 执行`alter resource pool <pool name> unit_number=<bigger number>`；命令，扩充资源池的unit个数
1. OB自动启动"**rebalance**"过程，将部分数据从旧的unit**在线**复制到新的unit上，花费一段时间。
1. 每个**分区**的数据复制完成后，OB自动将服务切换到新的unit上，并删除旧的unit分区中的数据
1. 查看__all_virtual_sys_task_status表，查看是否有comment like '%partition migration%'的服务来判断数据复制的过程是否已经完成

## 4.3 分布式事务、MVCC、事务隔离级别

### 4.3.1 分布式事务跨机执行时，OB通过多种机制保证ACID

#### A：原子性Atomocity

> 原子性是指事务是一个不可分割的工作单位，事务中的操作要么都发生，要么什么都不发生

- 依赖两阶段提交协议保证分布式事务的原子性

#### C：一致性Consistency

> 事务前后数据的完整性必须保持一致

- 保证主键唯一等一致性约束
- 全局快照-单租户GTS服务，1秒冲能够响应获取全局时间戳的调用次数超过200万次

#### I：隔离性Isolation

> 多个用户并发访问数据库时，数据库为每个用户开启的事务，不能被其他事物的操作干扰

- 采用MVCC进行并发控制，实现read-committed的隔离级别；
- 所有修改的行加互斥锁，实现写-写互斥；
- 读操作读取特定快照版本的数据，读写互不阻塞

#### D：持久性Durability

> 一个事务一旦被提交，它对数据库中数据的改变就是永久性的，接下来及时数据库发生故障也不应该对其有任何影响

- Redo-Log使用Paxos协议做多副本同步

### 4.3.2 两阶段提交，任何故障均可保障事务的原子性

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121541520.png)
OB两阶段提交协议特点

- 事务协调者和所有参与者都是高可用的
- 单机多分区事务，所有参与者都prepare成功即认为事务进入提交状态，立即返回客户端commit
- 全自动处理异常情况
- 存在协调者和参与者两个角色：
  - 协调者向多个参与者发送请求，确认该操作可进行，参与者返回可执行后，协调者再次请求执行，收到全部状态后，返回给应用。
  - 如果任何参与者返回失败或超时，则把整个事务回滚并返回给应用。
  - 其中，OBProxy不是协调者（因为OBProxy是无状态的）。一般一个OBS是协调者，其他的OBS是参与者，取决于OB Proxy请求到哪个OBS上。
- 所有参与事务的都是主副本，非从副本
- 在prepare阶段的时候，协调者和参与者是有Redo-Log并同步到从副本上（从副本知道主副本状态）

### 4.3.3 多版本并发控制(MVCC),解决读写互斥问题

MVCC可以理解成：当A事务对数据进行修改但未提交的时候，以系统时间戳为版本号记录两个版本数据，在成功提交前，其他事务读取时都获取的是未提交的旧版本数据，等提交完成后获取的才是新版本数据。

#### MVCC核心功能

利用锁机制控制读写冲突时，加锁后其他事物无法进行读，导致读写竞争，影响读的并发性。MVCC可以解决问题：

- 全局统一的数据版本号管理，取自全局唯一的时间戳服务（GTS）
- 读、写操作都要从GTS获取版本号；同一个租户内只有一个GTS服务，可以保证全局（跨机）一致性；
- 修改数据：事务未提交前，数据的新旧版本共存，拥有不同的版本号
- 读取数据：先读取版本号，再去查找小于等于当前版本号的已提交数据
- 写操作获取行锁，读操作不需要锁，有效避免读写锁竞争

### 4.3.4 事务隔离级别（Isolation Level）

保证全局数据一致性的隔离级别

#### 事务并发问题

- 脏读：读取了未提交的数据
- 不可重复读：指的是在同一事物内，不同的时刻读到的同一批数据可能是不一样的（期间被别的事务更新数据）
- 幻读：指的是在同一事物内，在操作过程中进行两次查询，第二次查询的结果包含了第一次查询中未出现的数据或缺少了第一次查询中出现的数据（期间被别的事务插入或者剔除了数据）

#### OB支持多种事务隔离

- 基于全局一致的数据版本号管理，以不同的版本号策略实现不同的隔离级别
- 支持以下两种隔离级别：
  - Read-Committed：避免脏读，存在不可重复读和幻读（默认）
  - Serializable：避免脏读、不可重复读、幻读
- 不支持脏读（Dirty-Read），只能获取已提交数据

## 5.1 SQL引擎

### 5.1.1 SQL引擎支持MySQL和Oracle兼容模式

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121543000.png)

- 同一个集群，同时支持Mysql和Oracle
- 租户创建时需要配置MySQL兼容模式或Oracle兼容模式
- DBA由原来维护“多个数据库产品”变为维护一个“统一的数据库产品”，DBA可以根据应用需求创建不同兼容模式的租户

#### MySQL兼容模式

- MySQL 5.6语法全兼容
- 兼容MySQL通信协议，MySQL应用可以直接迁移至OB

#### Oracle兼容模式

- 兼容Oracle 11g语法
- 兼容90%的Oracle数据类型和内置函数
- 支持分布式执行的存储过程（PL/SQL)

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121543590.png)

## 5.2 存储引擎与备份恢复

### 5.2.1 传统数据库有随机写、写放大等问题

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121543142.png)
大量随机写：buffer pool和表空间页面“一一对应”，数据更新时会在磁盘上产生频繁的随机写（check point）；
写放大：随机写导致SSD的写放大问题，影响性能及磁盘寿命

#### 传统数据库中的读写操作

##### 读数据

- 如果buffer pool中有，则直接从内存读，如果没有，则从硬盘中提取到buffer pool中
- 可以提升热数据的读取速度，减少时延

##### 写数据

- 修改数据时，现将数据写到buffer pool，再刷新到磁盘
- 通过check point将脏数据刷新到硬盘中，造成随机写和写放大：
  - 数据页离散分布，造成大量随机写，延迟大，影响性能；
  - SSD的随机写会导致严重的写放大，不仅影响写操作性能，而且显著降低SSD的寿命
  - 一般使用高端读写型的SSD

### 5.2.2 准“内存数据库”+LSMTree存储，避免随机写

#### 描述

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121544916.png)

- 增量数据直接写入内存，并将Redo-Log落盘及同步给从副本后，即可通知业务成功
- 内存占用率达到阈值后冻结MemTable，并执行转储/合并等操作以释放内存空间
- 内存增量数据批量合并到磁盘，以顺序读写代替随机写
- 读操作时，需要从热点缓存，MemTable以及转储SSTable中读取数据，确保数据一致性

#### 技术优势

- 读写分离：读内存和写内存分开
- 提升写速度：准内存处理，数据修改主要是内存操作，无频繁checkpoint操作，提高写性能
- 避免随机写：内存的脏数据批量合并之后，顺序写入SSD，提高写性能同时延长SSD寿命
- 数据持久性：为避免内存中数据丢失，redo-log以WAL机制实时落盘，保证数据持久性
- 降低成本：磁盘数据按主键有序排列，磁盘碎片少，并提供快速检索能力，使用普通读密集型SSD
- 底层存储会划分微块和宏块，有数据库内部管理

### 5.2.3 OB转储和合并简介

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121544010.png)

#### 转储操作（minor freeze）

- 目的是不断的把内存的MemTable写入磁盘以释放内存空间
- 转储过程首先会冻结MemTable（阻止当前的MemTable再有新的写入），并生成新的活跃的MemTable
- Partition副本可以独立决定冻结当前MemTable，并转储到磁盘上
- 转储出的数据只与相同大版本的增量数据做递归合并，不与全局静态数据合并

#### 合并操作（Major freeze）

- 试讲静态数据做归并，会比较费时，当转储产生的增量数据积累到一定程度时，通过Major freeze实现大版本的合并

- **磁盘数据按主键有序排列，提供快速检索能力。内存增量数据（MEMTable）分多级做批量归并（Minor-Major），最终整合到磁盘（SS Table），对整体性能影响较小。**

### 5.2.4 OB转储与合并区别

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121544531.png)

### 5.2.5 LSMTree存储高数据压缩率，降低存储要求

OB会对数据进行2次压缩，第一次压缩是encoding，第二次是通用压缩。

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121544340.png)

#### 第一次压缩-encoding

使用字典、RLE等算法对数据做瘦身

#### 第二次压缩-通用压缩

- 第二次压缩使用lz4等压缩算法再次压缩
- 支持snappy、lz4、lzo、zstd等压缩算法，由用户选择
- OB比MySQL平均节省一半空间
- 查询性能基本没有变化，写入（合并）性能有了较大提升

### 5.2.6 备份/恢复

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121545442.png)

- 支持全量备份和增量恢复：直接对存储层基线数据做全量备份，通过redo-log实现增量备份；在线备份对业务无影响；
- 最小粒度为租户：备份恢复目前支持的最小粒度是租户，备份灵活、节省空间；
- 全局范围内保证数据一致性：所有节点数据分别备份，统一管理；可恢复至全局统一的时间点，确保全局数据的一致性；
- 支持数据库上的任何操作：支持的数据包括用户权限、表定义、系统变量、用户信息、视图信息等逻辑数据以及所有的物理数据；
- 支持多种备份介质：可以备份至普通NFS，也可备份至阿里云对象存储（OSS）；备份恢复也可以恢复到任意时间节点；
- 性能：备份速度可以达到硬件处理能力上限；恢复速度取决于并发度，实测可达500MB/秒。

## 6.1 OCP及ODC简介

### 6.1.1 OCP（OceanBase Cloud Platform）是企业级数据库管理平台

OCP(OceanBase Cloud Platform)是一款以OceanBase为核心的企业级数据管理平台。

#### 高效的OB集群管理

- 支持高可用性
- 最多可以同时管理500台主机
- 响应速度可达到秒级

#### 可视化的体验

- 任务详情提供流程图和自然灵活的画布交互，管理任务进度更直观、高效
- 新增集群和租户的拓扑图，透出分布式的业务逻辑，展示运行状态、关键数据
- 依据DBA的工作场景，划分沉浸式导航的功能模块，功能入口更加清晰

#### 流畅高效的用户路径

- 对核心job的任务进行串连设计，流程清晰无断点
- 提供运维任务的快捷入口，在所有页面均可快速切换至任务
- 提供单个集群、单个租户的沉浸式管理视角，更符合用户心智

#### 专业化的功能

- 提供OB集群的全生命周期管理
- 针对OB特性设计的管理功能
- 从集群到会话的递进式管理功能
- 精心设计的拓扑图功能

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121545761.png)

### 6.1.2 OCP核心功能

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121546719.jpeg)

### 6.1.3 OB开发者中心（ODC）

ODC(OceanBase Developer Center)是为OceanBase数据库量身打造的企业级数据库开发平台，可通过B/S或C/S架构为开发者提供日常开发操作、WebSQL、SQL诊断、会话管理和数据导入导出等功能。

- 支持连接OB中MySQL和Oracle模式下的数据库，提供日常开发操作、WebSQL、SQL诊断等
- 可下载专门客户端，也可以直接浏览器登录

![替换图片](https://raw.githubusercontent.com/ZhengqiaoWang/blog_resources_1/main/202209121546206.png)

#### 数据导入导出

ODC提供数据导入导出和单表导出功能：

- 通过整库导入导出用户可以批量选择表以及对应的数据
- 通过单表导出，用户可以导出目标表中的数据
- 支持导入导出SQL文本和ODC格式的文件

### 6.1.4 实时同步工具(OMS)

其功能是不停的从源数据库读取数据，将数据导入到目标的OB数据库中去。

方便平滑数据迁移，避免服务下线过久。

- 支持多种数据源：支持Oracle、MYSQL、DB2、OB等全量和增量数据实时同步
- 兼容性评估和改造：
- 一站式交互：一个界面完成全部操作
- 多重数据校验：提供多种方式校验的保护，更加全面、省时、高效地保障数据质量；同时展示差异数据，提供快速订正途径
