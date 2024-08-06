---
layout: post
title: Mysql_basic
date: 2024-07-26
tags: [mysql]
author: chizzk
---
## sql-transaction
### 一.事务四大特性
ACID：原子性 一致性 隔离性 持久性<br>
**隔离级别：**
>读未提交
>>一个事务读取到了另外一个事务没有提交的数据（脏读）
>
>读已提交
>>一个事务要等到另外一个事务提交之后才能读取数据（不可重复读）
>
>可重复读
>>同一事务下，事务在执行期间，多次读取同一数据时，能够保证读取的数据是一致的。（幻读）
>
>串行化
>>一个事务读取到了另一个事务新增的数据

查询mysql全局事务隔离级别：select @@global.tx_isolation;

### 二.MVCC(Multi-Version Concurrency Control)
MVCC原理底层就是通过read view 以及 undo log来实现<br>

**1. 为什么会有MVCC?**<br>

频繁加锁会导致数据库性能低下，引入了MVCC多版本控制来实现读写不阻塞，提高数据库性能，<br>
在多版本并发控制中，为了保证数据操作在多线程过程中，保证事务隔离的机制，降低锁竞争的压力，保证较高的并发量。<br>
在每开启一个事务时，会生成一个事务的版本号，被操作的数据会生成一条新的数据行（临时），但是在提交前对其他事务是不可见的，<br>
对于数据的更新（包括增删改）操作成功，会将这个版本号更新到数据的行中，<br>
事务提交成功，将新的版本号更新到此数据行中，这样保证了每个事务操作的数据，都是互不影响的，也不存在锁的问题。<br>

**2. MVCC在哪个隔离级别下会失效**<br>

MVCC只在 READ COMMITTED 和 REPEATABLE READ 两个隔离级别下工作。<br>
其他两个隔离级别和MVCC不兼容，因为 READ UNCOMMITTED 总是读取最新的数据行，而不是符合当前事务版本的数据行。而 SERIALIZABLE 则会对所有读取的行都加锁。<br>

**3. InnoDB行数据默认隐藏列**<br>

InnoDB在每行数据都增加三个隐藏字段：一个唯一行号，一个记录创建的版本号，一个记录回滚的版本号。<br>

**4. ReadView是什么**
在我们平时执行一个事务的时候，就会生成一个ReadView，ReadView的组成结构如下：<br>
- creator_trx_id
- m_ids
- min_trx_id
- max_trx_id

**5. ReadView参数解释**
1. creator_trx_id:当前事务id
2. m_ids:所有活跃事务的事务id 当前所有未提交的事务id组成的集合
  
3. min_trx_id:m_ids中最小的事务id  当前所有未提交的事务id构成的集合中的最小的事务id
4. max_trx_id:最大事务id  下一个即将创建的事务id

**6. ReadView快照生成的时机**

>repeatable read级别 ：事务级的快照

>read commited级别 ：语句级的快照

**7. 版本链比对规则**
1. 如果被访问版本的trx_id属性值与rv中的creator_trx_id值相同 可见
2. 如果被访问版本的trx_id属性值小于rv中的min_trx_id值 可见
3. 如果被访问版本的trx_id属性值大于或等于rv中的max_trx_id值 不可见
4. 如果被访问版本的trx_id属性值在rv的min_trx_id和max_trx_id之间<br>
    4.1 trx_id在m_ids中 不可见<br>
    4.2 trx_id不在m_ids中 可见<br>