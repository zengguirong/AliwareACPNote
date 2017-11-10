## 概念
分布式关系型数据库服务（Distributed Relational Database Service)


## 特点
+ 轻量(无状态)
+ 灵活
+ 稳定
+ 高效


## 应用场景
大规模**在线**数据操作：   

+ 高并发实时交易场景
> 电商、金融、O2O、零售等用户基数大、营销活动频繁的行业   
> DRDS提供线性水平扩展能力, 峰值**TPS达150万+**

+ 海量数据存储访问场景
> 工业制造、智能家居、车联网等超大规模数据存储访问场景   
> DRDS可线性扩展存储空间, 目前支持**200+** MySQL实例的单数据库集群, 提供PB级存储能力

+ 高性价比数据库解决方案
> 传统方案需要强依赖小型机和高端存储等高成本的商业解决方案

+ 低运维成本数据库
> 适合技术积累相对比较薄弱、资金投入有限的初创型企业初期发展

+ 数据备份
> 企业可以利用 DRDS 将自建数据库数据备份到云上，实现云上的数据备份容灾

## 解决的问题
+ 单机数据库**容量**瓶颈: 数据量增长带来的挑战
+ 单机数据库**扩展**困难: 数据扩展导致服务中断
+ 传统数据库使用**成本**高: 用高端存储和小型机设备应对数据、并发量增长, 成本高

## 功能特性
+ 高度兼容MySQL协议和语法
+ 支持分库分表, 自动化水平拆分
+ 在线数据储存平滑扩缩容
+ 服务升降配, 弹性扩展(改变资源数量)
+ 透明读写分离
+ 具备数据库全生命周期运维管控能力
+ 分布式运维指令集(SHOW SLOW、TRACE、SHOW NODE等)
+ 全局唯一数字序列(且有序递增)
+ 数据库账号权限体系(类单机MySQL账号权限体系)
+ 分布式事务(结合GTS套件实现)
+ 监控报警(核心**资源**指标和数据库**实例**指标实时监控，如如实例 CPU、网络 IO、活跃线程等)

## 产品优势
+ 分布式(集群化)
+ 弹性(横向扩展: 添加机器, 纵向扩展:给原有机器升级配置)
+ 高性能(操作聚焦更少数据, 特定SQL并行执行)
+ 安全(类单机MySQL账号体系, 并提供具备授权鉴权的Open API)
+ 简单易用(兼容MySQL协议和大部分MySQL SQL语法，读写分离无业务侵入)
+ 成熟度高

## 名词解释
-----

1. DRDS服务节点(DRDS Server)   
DRDS Server是DRDS核心组件, 提供SQL的解析、优化、路由和结果归并

2. DRDS实例   
DRDS实例是由一组DRDS Server节点组成的分布式数据库服务**集群**; 各服务节点**无状态**, 同时处理SQL请求

3. DRDS 实例规格   
DRDS实例规格是DRDS实例处理能力体现, 按照**CPU和内存**提供不同的规格的实例，规格越高处理能力越强; 如4Core8G和8Core16G, 在标准的DRDS测试场景下, 后者的处理能力是前者的**两倍**

4. 实例升降配   
DRDS可以通过改变实例规格来改变处理能力，提升实例规格称为升配，降低实例规格称为降配

-----

1. 垂直拆分(一般概念)   
把关系紧密（比如同一模块）的表切分出来放在一个服务器上, 适合数据表多的情况
DRDS中没有垂直拆分

2. 水平拆分(切分维度: 行)   
把表的数据按某种规则（比如按ID散列）切分到多个服务器上, 适合表数据量多的情况

3. 拆分规则   
水平拆分过程中将逻辑数据库表拆分为多个物理分表规则称为拆分规则

4. 拆分键   
水平拆分过程中, 生成拆分规则的数据库**字段**称为拆分键

5. 分库
DRDS水平拆分后, 逻辑数据库数据存储在多个物理存储实例上, 每个存储实例上的**物理库**称为分库

6. 分表
DRDS水平拆分后, 每一个分库上的**物理数据表**称为分表;

7. 非拆分模式   
DRDS支持不进行数据库水平拆分而仅通过DRDS提供的**透明读写分离**来扩展数据库的服务能力; 这种模式称为非拆分模式

-----

1. 逻辑 SQL   
由应用端发送到DRDS的SQL称为逻辑SQL

2. 物理 SQL   
由DRDS对逻辑SQL进行解析之后发送到RDS上执行的SQL称为物理SQL

3. 读写分离   
为了确保数据库产品的稳定性, 很多数据库拥有双机热备功能; 读写分离，基本的原理是让主数据库处理**事务性增、改、删**操作, 而从数据库处理**查询**操作; 数据库**复制**被用来把事务性操作导致的变更**同步**到集群中的从数据库

4. 透明读写分离   
DRDS的单个存储实例节点遇到访问瓶颈时, 可通过增加**只读实例**来分担主实例的压力; DRDS的读写分离功能**不需要修改任何应用代码**, 称为透明读写分离

5. 平滑扩容   
DRDS可通过增加存储**实例节点**完成数据库的扩容; 扩容**不影响原有数据的正常访问**, 称为平滑扩容

6. 小表广播   
DRDS将一些**数据量小且更新频度不高**的数据表存储为**单表**模式，这些数据表称为小表; 通过数据同步将小表**复制**到与之JOIN的分库上进而提升JOIN效率的解决方案称为“小表广播”或者“小表复制”

7. 全表扫描   
数据库拆分模式下, 如果SQL语句中没有指定拆分键, DRDS将在所有分表上执行SQL并归并结果返回, 这个过程称为全表扫描; 为避免影响性能, 用户应尽量**避免**全表扫描

8. 全局唯一数字序列(DRDS Sequence)   
DRDS全局唯一数字序列(是**64**位数字, 对应MySQL中**BIGINT**类型)的主要目标是为了保证所定义唯一字段中的数据的**全局唯一**（比如PRIMARY KEY, UNIQUE KEY)和**有序递增**

9. DRDS自定义注释(DRDS Hint)   
DRDS提供的自定义注释, 用于指定一些**特殊行为**, 通过相关的语法影响SQL的**执行方式**，从而对SQL进行**特殊的优化**

-----

## SQL兼容性
参见[链接](https://help.aliyun.com/document_detail/49249.html?spm=5176.doc49248.6.544.k0SFHY)

## 版本查看
在控制台中的基本信息中或命令执行`SELECT version();`可以查看DRDS版本

## 快速入门注意事项
1. 创建数据库**只能在DRDS控制台**进行, 不支持**建库DDL**
2. DRDS实例需要使用RDS资源, 但对RDS原有数据无影响
3. DRDS支持跨区域使用RDS节点, 带来最高3ms(一般1 ms左右)的网络延迟
4. 创建数据库后尊享版DRDS默认不开放外网连接, 需要手动申请外网地址
5. 创建数据库时, 需要指明密码, 用户名默认与DRDS数据库名相同
6. 拆分模式下, DRDS默认在每个RDS实例上创建**8个**物理库
7. 允许创建只读用户
8. DDL创建表需要加入表拆分信息


## DRDS分库分表
> DRDS在后端将数据量较大的数据表水平拆分到后端的每个RDS数据库中, 这些拆分到 RDS中的数据库被称为**分库**, 分库中的表称为**分表**;   
拆分键即分库/分表字段, 因此分为**分库键**和**分表键**; 拆分键暂时只支持**单个**字段   
>
+ 分库键: DRDS 根据分库键的值将数据水平拆分到后端的每一个RDS分库里; 键值相同的数据, 一定会位于同一个RDS数据库里   
+ 分表键：每一张逻辑表都可以定义自己的分表键, 键值相同的数据, 一定会位于同一个RDS数据表里

DRDS支持以下三种拆分方式, 均是基于水平拆分:  

1. 不拆分, 数据表存储在单一物理库中(表数量: 1)
2. 分库不分表, 根据规则(如hash id)将一个表拆分到不同的物理库中(数据表数量: DDL指定的分表数)
3. 分表不分库, 根据规则(如hash id)将一个表拆分到同一物理库的不同表当中(表数量: RDS物理库数量[默认RDS实例数量*8])
4. 分库分表, 结合2、3(表数量:  DDL指定的分表数量*RDS物理库数量[默认RDS实例数量*8])

## SQL 路由
1. 路由
DRDS可以解释SQL, 按照拆分策略找到数据表所在的RDS物理库和物理表位置, 并将SQL发到对应数据库上执行

2. 数据合并
同一SQL被分配到多个数据库上执行的时候, 最终结果会被DRDS进行合并返回

## DRSD 读写分离
1. 原理   
增加只读实例对主实例的**读流量进行分流**, 减轻主实例压力
注意：  
>
强读: 实时一致性强读, 从主实例读取的数据   
弱读: 非强一致性读, 从只读实例读取的数据   
只读实例上的数据是**异步复制**的, 存在**毫秒**级别的延迟   
**DRDS Hint**可强制到主实例上读取数据

2. 特点
 + 对应应用透明
 + DRDS控制台可配置读流量分流比例
 + 写流量不分流

3. 支持非拆分模式下的读写分离
4. 事务支持
显式**事务**、写请求 -> 主实例
查询请求 -> 读写分离
`SELECT、SHOW、EXPLAIN、DESCRIBE`均属于读请求   
`INSERT、REPLACE、UPDATE、DELETE、CALL`均属于写请求

5. 规则
 + 读写比例在容量管理页面是以RDS实例为单位设置的; 如果一个DRDS数据库含有多个RDS 实例, 则需要针对每个RDS实例设置读写比例
 + RDS只读实例过期或者需要释放时, 需要在DRDS控制台中把读权重置为0, 否则流量会继续走到原有只读实例, 导致执行失败
 + 读写分离以DRDS数据库为基本单位, 如果同一个RDS只读实例在多个DRDS 数据库上使用, 需要在每个DRDS数据库上都将其权重设置为0

6. 查看
执行`SHOW NODE`指令查看实际读流量分布

## DRDS平滑扩容
**步骤:**   

1. 添加RDS节点
 + 默认会将分库平均分配到新节点上, 也可以手动分配
2. 迁移
 + `迁移`任务不会变更原有数据库中数据, 不会影响业务, 只是把待迁移库中的数据同步到新增的RDS上
 + 在`切换`前, 可以通过回滚, 放弃本次平滑扩容操作
 + 扩容期间需要停止清理源RDS的Binlog文件, 可能会导致磁盘空间不, 务必在源RDS实例上预留充足的磁盘空间(一般百分之三十以上为宜), 不够的话提工单解决
 + 源RDS实例扩容过程中会有读压力, 请尽量在源RDS低负载时操作
 + 扩容期间请勿在控制台提交DDL任务或通过直接连接DRDS执行DDL语句, 否则会导致扩容任务失败
 + 扩容需要保证源库中所有表具有主键, 如果没有需要事先添加好

3. 切换
 + 历史数据和增量数据`迁移`完成后, `迁移`任务进度会达到100%, 此时可以进行`切换`或者`回滚`, 放弃本次扩容
 + `切换`任务会将读写流量切换到新增的RDS 实例上, 整个过程会在3~5分钟内完成; 在`切换`过程中, 除了会有一到两次闪断, 服务不受影响; **请在业务低谷期执行`切换`**

4. 清理
 + `切换`完成后, 单击`清理`按钮并确认; 此步骤将删除原RDS上被迁移的分库
 + `清理`任务完成后, 整个平滑扩容过程结束; 新增RDS实例会成为DRDS对应逻辑库新的存储节点
 +  `清理`操作对数据库有一定压力，**请在业务低谷期执行**
 + 有需要的话`清理`前进行数据备份

清理任务完成后，整个平滑扩容过程结束。新增 RDS 实例会成为 DRDS 对应逻辑库新的存储节点。

**注意: 平滑扩容资源消耗高, 执行扩容前请提工单寻求DRDS团队协助**

## DRDS实例管理
### 实例类型
1. 共享实例: 共享物理资源(CPU、内存等), 逻辑隔离
2. 专享实例: 独享DRDS的物理资源

共享实例不支持**账号和权限系统**、不支持变更配置、只有单一规格、不定期统一升级、且每个**地域**只有一个**可用区**

### 升降配
1. 注意事项
 + 按量付费实例,支持动态升降配; 包年包月实例支持升配, 不支持降配
 + 降配会导致应用与DRDS连接中断, 应用具备重连能力可以自动恢复
 + 使用长连接时, 升配后新的节点不能有效的接收流量, 尽量重启应用

### 版本升级
1. 注意事项
 + 升级过程中请不要进行其它操作, 如建库、平滑扩容等
 + 升级后24小时内可回滚, 超过24小时需提出工单申请回滚

### 外网地址
1. 注意事项
 + 外网地址需要手动申请
 + 安全起见, 最好为外网客户端IP设置白名单
 + 共享实例无法申请外网地址

### 数据库迁移
共享实例的数据库可迁移至专享实例   
迁移后连接串会发生变化, 数据库业务需要暂停

### 克隆实例
1. 克隆内容
 + DRDS的逻辑库以及实例地域、可用区、数据库类型、版本、网络类型、连接池信息
 + 底层RDS的备份设置、数据、网络类型以及地域、可用区、数据库类型、版本、网络类型、白名单、阈值报警设置、SQL审计设置

2. 克隆规则
 + 克隆的新DRDS实例规格统一为最小规格(4C4G)
 + 克隆的新RDS实例规格和元RDS实例保持一致
 + 新的DRDS和RDS均为按量计费
 + 如果RDS主实例下挂载了只读实例以及灾备实例, 克隆时只克隆主实例
 + 克隆对线上实例没有影响

3. 克隆要求
 (1). DRDS   
 + 实例必须为 5.1.28-1320920及以上版本
 + DRDS 实例必须为专享实例   

 (2). RDS   
 + RDS状态为运行中且没有被锁定
 + 当前没有迁移任务
 + 已开启数据备份和日志备份
 + RDS没有创建高权限账号
 + RDS实例不是 5.7 版本

## DRDS 账号和权限系统
> + 支持GRANT、REVOKE、SHOW GRANTS、CREATE USER、DROP USER、SET PASSWORD等语句, 且与mysql语法一致
> + 支持库级、表级权限、不支持列级、全局级别和子程序级别权限

1. 账号
 + DRDS与RDS账号**没有关系**
 + 创建完数据库后自动生成**管理员账号**和**只读账号**,不能删除、不能更改权限
 + 管理员帐号跟数据库名称一致,只读账号多了_RO后缀
 + 根据`username@'host'`确定一个账号,`host`不一样会被认为是不同账号
 + 只读用户的用户名是固定的, 不可更改

2. 账号规则
 + 管理员帐号具有所有权限
 + 其他账号**只能**由管理员账号**创建**以及**授权**
 + 管理员权限与数据库绑定, 不能跨数据库操作
 + 只读账户只有`SELECT`权限

3. 用户名规则
 + 大小写不敏感；
 + 4<= 长度 <=20 字符；
 + 必须以字母开头；
 + 字符可以包括大写字母、小写字母、数字

4. 密码规则
 + 6<= 长度 <=20 字符(如果有), 密码**可以为空**
 + 字符可以包括大写字母、小写字母、数字、特殊字符(**@#$%^&+=**)

5. HOST匹配规则
 + HOST 必须是**纯IP**地址，可以包含 `_`和`%`通配符(`_`代表一个字符, `%`代表0个或多个字符); 含有通配符的HOST需要加上**单引号**, 例如 `lily@'30.9.%.%'`, `david@'%'`
 + 假设系统中有两个用户都符合当前欲登录的用户, 则以**最长前缀匹配**(不包含通配符的最长IP段)的那个用户为准
 + 开启VPC时, 主机的IP 地址会发生变化。为避免账号和权限系统中的配置无效,请将 HOST配置为`'%'`来匹配任意I。

6. 权限项
 + CREATE
 + DROP
 + ALTER
 + INDEX
 + INSERT
 + DELETE
 + UPDATE
 + SELECT

7. 权限规则
 + 权限是绑定到账号(`username@'host'`), 不是绑定到用户名(`username`)
 + 授权的时候会判断表是否存在, 不存在则报错
 + 数据库权限级别从高到低依次是: 全局级别权限(暂不支持)、数据库级别权限、 表级别权限、列级别权限
 +  高级别权限的授予会覆盖低级别权限, 移除高级别权限的同时也会移除低级别权限
 + 不支持`USAGE`授权
 +

## 全表扫面
  + DRDS禁止执行不带where条件的update、deelte语句
  + 设置全表扫面你是为了执行sql中where条件不带拆分键或者拆分键太广的语句, sql将被分配到所有分库中执行并汇总
  + 全表扫描效率低, 应避免使用

## 聚合函数
 + 支持 COUNT、MAX、MIN、SUM， 另外还支持 LIKE、ORDER BY 与 LIMIT 语法

## IP白名单
 + DRDS提供了访问控制功能, 只有通过白名单规则的IP才可以访问
 + IP地址支持全IP、CIDR模式和 `*` 号占位符模式; 当配置为 `*.*.*.*` 或空时，为不设置 IP 访问限制

## 数据导入
支持从两种数据源导入:
 1. MySQL
 2. RDS
支持两种方式：
 1. 数据量较小时(<500万条): 可利用`mysqldump`、`mysql source`等命令
 2. 数据量较大时: 建议从DRDS控制台导入
注意:
 1. 通过DRDS控制套导入数据时使用的是DTS服务
 2. DTS导入数据不支持结构导入, 要先在DRDS库上建表


## 创建表
1. 支持create, alert, drop, 加索引和删索引五种类型的sql   
 + 方式1: DRDS控制台上传sql文件
 + 方式2: DRDS控制台输入ddl(目前版本貌似不支持)
 + 方式3: MySQL客户端连接输入ddl   

2. 建表DDL
  > 相比原生MySQL的DDL, 增加了拆分选项(drds_partition_options),包括`DBPARTITION BY`、`TBPARTITION BY`、`TBPARTITIONS`、`BROADCAST`

  + DBPARTITION BY: 指定分库键(如`DBPARTITION BY hash(id)`)
     + 可选的, 不指定时不分库
     + 不支持按照时间分库
     + 默认使用主键
  + TBPARTITION BY: 指定分表键(如(`TBPARTITION BY hash(bid)`))
     + 是可选的
     + 键默认与`DBPARTITION BY`相同
     + 支持按照日期拆分(如`tbpartition by WEEK(actionDate)`)
  + TBPARTITIONS: 指定在同一个分库上的分表数(如`TBPARTITIONS=3`)
     + 默认值为1
  + BROADCAST: (小表)广播表, 即各个分库上都具有此表的全表   
     + 通过同步机制实现数据一致, 有秒级延迟
     + 好处是避免了跨表JOIN

3. 查看拆分规则
`show rule from user_log;`

4. 拆分函数
 + 哈希`hash(key)`
 + 双字段哈希`range_hash(col1,col2,N)`
 + 数值右移`right_shift(key,N)`
 + 时间
   + `WEEK(day_of_week)`
   + `MM(month_of_year)`
   + `DD(day_of_month)`
   + `MMDD(day_of_year)`

## DDL常见问题(分布式DDL执行失败)处理步骤
1. 查看错误信息, 若错误信息太长可使用`SHOW WARNINGS`, 找出错误原因
2. 使用`SHOW TOPOLOGY FROM table_name `查看表的拓扑结构
3. `CHECK TABLE tablename`指令来查看逻辑表是否创建成功
4. 使用幂等的方式继续执行建表操作或删表操作，会创建或删除剩余的物理表。