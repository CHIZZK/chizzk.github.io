---
layout: post
title: transaction
date: 2024-09-26
tags: [spring_transaction]
author: chizzk
---
## transaction
1. 传播行为<br>
1.1 REQUIRED：存在事务就支持，不存在就开启一个事务<br>
1.2 SUPPORTS：存在事务就支持，不存在就以非事务方式执行<br>
1.3 MANDATORY：不存在事务就报错<br>
