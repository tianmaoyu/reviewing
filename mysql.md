## 一，常见问题

1. Mysql 如何找到慢sql，如何优化，那些手段
2. Mysql 数据库中修改如何对它进行记录修改前，修改后 （多个角度，binlog）
3. Mysql 中的某mvcc 原理 ==难==



1. **解释 InnoDB 与 MyISAM 的区别。**
2. **如何优化一个复杂的 SQL 查询？**
3. **什么是死锁？如何避免死锁？**
4. **如何通过 EXPLAIN 来优化查询？**
5. **如果你有一个包含上百万条记录的表，如何优化查询性能？**
6. **如何在 MySQL 中实现主从复制？**

7. **什么是事务的隔离级别？不同级别的区别是什么？**

8. **MySQL 中的数据类型有哪些？**

9. **如何做数据库备份与恢复？**
10. **MySQL 的存储引擎有哪些，如何选择合适的存储引擎？**

#### 知识点

1. 锁，行锁，表锁，mvvm

2. mvvm 解决什么问题？ 和锁的关系

3. 底层的数据结构

4. 索引，什么时候走索引

5. 常考点，

6. 优化的一些方法

7. 为什么事 b+tree，为什么不是红黑树

8. explan 优化-常用的哪些标签

9. Read View（一致性视图）

10. redo-log ： buff-pool；  顺序写入

11. Undo-log: 原子性

12. Bingo. :  记录了语句；  redo log 就变成了两阶段提交；

13. 生产环境 buffer-pool 设置多大； 一般设置 系统==内存的 60-80%==

14. ```shell
    [mysqld]
    innodb_buffer_pool_size = 8G
    #每个 独立的 建设锁的竞争
    innodb_buffer_pool_instances = 8
    
    #计算命中率率
    Innodb_buffer_pool_reads: 从磁盘读取数据页的次数。
    Innodb_buffer_pool_read_requests: 从 buffer pool 中读取数据页的请求次数。
    Innodb_buffer_pool_hit_rate: 命中率，可以通过 (Innodb_buffer_pool_read_requests - Innodb_buffer_pool_reads) / Innodb_buffer_pool_read_requests 来计算。
    ```

15. 聚集索引（主键索引）： 存放数据的索引-叶子索引- 才存了完整的数据行

16. 二级索引： 叶子节点存储的是-主键值--- 回表：扫描两次； 没有主键-默认添加一个  rawid

17. 离散度低不键索引

18. 不是越多越好

19. 联合索引最左匹配原则- 经常用的放左边

20. 索引尽量有顺序，否中 数据 的存储的变动 变动

21. 索引条件下推： 把本来应该在server 过滤的直接交给 存储层

22. 覆盖索引：select 的字段 正好是 索引字段

23. 频繁更新的： b+tree 频繁的 结构的调整

24. 字段上使用函数 ：sum，cont，arg， 提前计算好 放一个冗余字段

25. 字符串，加引号，不加的 --- 隐式转换

26. like 前面 带%

27. 过长的字段；前缀索引

28. 小表驱动大表

29. 在哪些上建立索引： where， order by ， join on ,group by 上；

30. explain，extra 的几个 ：

    1. all： 全表扫描
    2. const： 主键索引
    3. ref：非唯一 ，
    4. rang：范围的索引
    5. index ：扫描全表索引（）full index scall
    6. all： 全表扫描
    7. 多少个 select 就有多少个id。；大到小 -优先
    8. 如果 id 值是相同的，那么 从上往下 优先

31. 优化方向

    1. 表结构，存储引擎
    2. 字段冗余，sql 语句，索引
    3. 分库，分表，读写分离
    4. mysql 配置参数
    5. 硬件

32. 并发面临的问题： 读不一致 

    1. 脏读； 读到为提交的数据 （内存中）
    2. 不可重复读： 两次读不一致 （update，delet）
    3. 幻读： insert

33. 事务的隔离级别： 读未提交，读已经提交，可重复读，串型化

34. 事务的四个特性：原子性：要么成功，要么失败； 隔离性：两个事务相互透明； 持久性：提交不可以变；  一致性： 前三个的目的

35. 数据读取的两种方法：1:  lock ； 2: mvcc。为每个事务生成快照

#### mcvv：

1. 应该看到：能看见之前的修改的数据
2. 应该看到：本事务修改的数据
3. 不应该看到：本事务后面的事务的修改
4. 不应该看到：活动的事务修改的数据

#### innerdb 的三隐藏的字段：

1. db_row_id; 
2. db_tra_id;   事务的创建版本号
3. Db_roll_prt： 事务的删除版本号





## 二，答案

### 2.1 Mysql 如何找到慢sql，如何优化，那些手段

#### **一、如何找到慢 SQL？**

1. **开启慢查询日志**

   ```sql
   -- 查看配置
   SHOW VARIABLES LIKE 'slow_query_log%';
   SHOW VARIABLES LIKE 'long_query_time';
   
   -- 动态开启（重启失效）
   SET GLOBAL slow_query_log = 'ON';
   SET GLOBAL long_query_time = 2;  -- 阈值（秒）
   ```

   - 日志路径：`slow_query_log_file` 参数指定。
   - 分析工具：`mysqldumpslow` 或 `pt-query-digest`（Percona Toolkit）。

2. **使用 `EXPLAIN` 分析执行计划**

   ```sql
   EXPLAIN SELECT * FROM users WHERE age > 30;
   ```

   - 关键字段：
     - **type**：访问类型（`ALL` 全表扫描，`index` 索引扫描，`range` 范围扫描）。
     - **key**：实际使用的索引。
     - **rows**：预估扫描行数。
     - **Extra**：`Using filesort`（需优化排序），`Using temporary`（需优化临时表）。

3. **监控工具**

   - `SHOW FULL PROCESSLIST`：实时查看活跃查询。
   - **Performance Schema**：记录 SQL 执行统计信息。
   - **Prometheus + Grafana**：可视化监控数据库指标。

#### **二、慢 SQL 的常见原因**

|   **原因分类**    |                         **具体场景**                         |
| :---------------: | :----------------------------------------------------------: |
|   **索引问题**    | 未建索引、索引失效（如对字段运算）、索引区分度低（如性别字段） |
|   **SQL 写法**    | 全表扫描（`SELECT *`）、复杂子查询、`LIKE '%前缀%'`、`OR` 条件未优化 |
|  **数据量过大**   |           单表数据量超千万级、未分页或分页深度过大           |
|    **锁竞争**     |   行锁等待、事务未提交导致长时间锁、表锁（如 MyISAM 引擎）   |
| **硬件/配置瓶颈** | 内存不足（`innodb_buffer_pool_size` 过小）、磁盘 IO 高、CPU 过载 |



#### **三、优化手段**

##### **1. 索引优化**

- 添加缺失索引

  ```sql
  CREATE INDEX idx_age ON users(age);
  ```

- 避免索引失效

  - 禁止对索引字段进行运算（`WHERE age + 1 > 30` → 改为 `WHERE age > 29`）。
  - 字符串查询使用前缀匹配（`LIKE 'prefix%'`）。

- 联合索引优化

  ```sql
  -- 遵循最左前缀原则
  CREATE INDEX idx_name_age ON users(name, age);
  ```

##### **2. SQL 重写**

- 分页优化

  ```sql
  -- 原语句（深度分页慢）
  SELECT * FROM users LIMIT 1000000, 10;
  
  -- 优化后（利用主键延迟关联）
  SELECT * FROM users 
  WHERE id >= (SELECT id FROM users ORDER BY id LIMIT 1000000, 1)
  LIMIT 10;
  ```

- **减少 `SELECT \*`**：仅查询必要字段。

- **分解复杂查询**：将大查询拆分为多个小查询，减少锁竞争。

##### **3. 表结构优化**

- **垂直拆分**：将大字段（如 TEXT）分离到独立表。
- **水平拆分**：按时间或哈希分表（如 `users_2023`）。
- **数据类型优化**：用 `INT` 代替 `VARCHAR` 存储 ID。

##### **4. 服务器调优**

- 调整 InnoDB 缓冲池

  ```ini
  # my.cnf 配置
  innodb_buffer_pool_size = 系统内存的 70%~80%
  ```

- 优化事务提交

  ```ini
  innodb_flush_log_at_trx_commit = 2  -- 非严格 ACID 场景提升性能
  ```

##### **5. 架构扩展**

- **读写分离**：用主库处理写操作，从库处理读操作。
- **缓存层**：使用 Redis 缓存热点数据。

------

#### **四、示例优化流程**

1. 通过慢查询日志定位耗时超过 2 秒的 SQL。
2. 使用 `EXPLAIN` 检查执行计划，发现全表扫描（`type=ALL`）。
3. 为条件字段添加索引，将查询时间从 2 秒降至 0.01 秒。
4. 监控优化后性能，确认无锁竞争或 IO 瓶颈。

------

通过以上方法，可系统性解决 MySQL 慢 SQL 问题，提升数据库吞吐量和响应速度。







### 2.2 Mysql 数据库中修改如何对它进行记录修改前，修改后 （多个角度，binlog）



### 2.3  Mysql 中的MVCC 核心原理

1. **版本链与隐藏字段**：
   - InnoDB每行数据包含三个隐藏字段：
     - `DB_TRX_ID`（6字节）：记录最后修改该行的事务ID。
     - `DB_ROLL_PTR`（7字节）：指向Undo Log中旧版本数据的指针，形成版本链。
     - `DB_ROW_ID`（6字节）：行标识（若未显式定义主键时自动生成）。
   - 每次更新/删除操作时，旧数据会被存入Undo Log，并通过`DB_ROLL_PTR`链接为新版本的“历史记录”。
2. **Read View（一致性视图）**：
   - 事务在启动时生成Read View，用于判断数据版本的可见性，包含：
     - `trx_ids`：当前活跃（未提交）的事务ID列表。
     - `low_limit_id`：生成Read View时系统尚未分配的下一个事务ID（即当前最大事务ID+1）。
     - `up_limit_id`：活跃事务中最小的事务ID。
     - `creator_trx_id`：创建该Read View的事务ID。
   - 通过比较数据版本的`DB_TRX_ID`与Read View的规则，决定版本是否可见。
3. **可见性判断规则**：
   - 若`DB_TRX_ID < up_limit_id`：该版本在Read View创建前已提交，**可见**。
   - 若`DB_TRX_ID >= low_limit_id`：该版本在Read View创建后生成，**不可见**。
   - 若up_limit_id ≤ DB_TRX_ID < low_limit_id：
     - `DB_TRX_ID`在`trx_ids`列表中：生成该版本的事务仍活跃，**不可见**。
     - 否则：事务已提交，**可见**。
   - 若`DB_TRX_ID = creator_trx_id`：该版本由当前事务修改，**可见**。
4. **Undo Log与版本链遍历**：
   - 当某行数据被多次修改，所有版本通过`DB_ROLL_PTR`形成链表。
   - 事务读取数据时，从最新版本开始遍历版本链，找到第一个符合可见性规则的版本。

------

#### **MVCC 实现细节**

1. **事务隔离级别的支持**：
   - **读已提交（RC）**：每次执行SELECT时生成新Read View，总能读取最新提交的数据。
   - **可重复读（RR，默认级别）**：仅在第一次SELECT时生成Read View，后续读取沿用该视图，保证多次读取结果一致。
   - 通过调整Read View生成时机，实现不同隔离级别的语义。
2. **快照读与当前读**：
   - **快照读**：普通SELECT语句基于MVCC读取历史版本，无需加锁。
   - **当前读**：UPDATE/DELETE/SELECT FOR UPDATE等操作读取最新版本并加锁（如记录锁、间隙锁），确保数据一致性。
3. **Purge机制**：
   - Undo Log中的旧版本数据在无活跃事务依赖时，由后台Purge线程清理，释放空间。
   - 删除操作仅标记记录为“删除”，待Purge时物理删除。

------

#### **示例流程**

1. 事务A（ID=100）更新一行：
   - 原数据存入Undo Log，生成新版本，`DB_TRX_ID=100`，`DB_ROLL_PTR`指向旧版本。
2. 事务B（ID=200）读取该行：
   - 生成Read View，假设此时活跃事务为[150, 180]。
   - 遍历版本链，发现最新版本`DB_TRX_ID=100`（小于`up_limit_id=150`且已提交），故读取该版本。

------

#### **总结**

MVCC通过版本链、Read View和Undo Log的协作，实现了高效的读写并发控制。其核心优势在于：

- **读不阻塞写，写不阻塞读**：通过历史版本避免锁竞争。
- **隔离级别灵活适配**：通过调整Read View生成策略支持不同一致性需求。
- **数据恢复与回滚**：Undo Log保留旧版本，支持事务回滚和崩溃恢复。

理解MVCC机制有助于优化事务设计，平衡性能与一致性需求。







## 二，Mysql 架构图

https://dev.mysql.com/doc/refman/8.4/en/sys-host-summary.html



## 三，B+Tree



==源码==
