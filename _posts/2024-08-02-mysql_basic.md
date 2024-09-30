---
layout: post
title: Mysql_basic
date: 2024-07-26
tags: [mysql]
author: chizzk
---
## MySQL
### innoDB Locking
- Shared and Exclusive Locks
- Intention Locks
- Record Locks
- Gap Locks
- Next-Key Locks
- Insert Intention Locks
- AUTO-INC Locks
- Predicate Locks for Spatial Indexes
#### shared and Exclusive Locks(共享锁和排他锁)
- 共享锁允许持有锁的事务读取某一行
- 排他锁允许持有锁的事务更新或删除某一行
如果事务T1在行r上持有一个共享锁，那么来自不同事务T2的对行r的锁定的要求将如下处理：<br>
1.T2对s锁的请求会立即被批准，结果,T1和T2都会对r持有s锁<br>
2.T2对x锁的请求不会被立即批准<br>
- 如果事务T1对行r持有独占x锁，那么来自某个不同事务T2的对行r的任何类型锁的请求都不能立即获得。相反，事务T2必须等待事务 T1 释放其对行r
的锁。
#### Intention Locks


#### Record Locks

#### Gap Locks

#### Next-Key Locks





