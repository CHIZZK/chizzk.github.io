---
layout: post
title: java-multithreading&locking
date: 2024-07-26
tags: [java]
author: chizzk
---
## 多线程和锁

> &emsp;&emsp;**volatile**:缓存一致性问题：对于某个共享变量，每个操作单元都缓存一个该变量的副本，当一个操作单元更新其副本时，
>其他的操作单元可能没有及时发现，进而产生缓存一致性问题。<br>
>&emsp;&emsp;在JMM(Java内存模型)的访问规则中，关于volatile有这样一条规则：对于一个volatile变量的写操作happen-before对这个变量之后的每一个读操作。<br>
>&emsp;&emsp;happen-before含义：如果指令甲happen-before指令乙，那么指令甲必须排序在指令乙前面，并且指令甲的执行结果对指令乙可见。<br>
>&emsp;&emsp;从规则的定义可知，如果多个线程同时操作volatile变量，那么对该变量的写操作必须在读操作之前执行（禁止重排序),并且写操作的结果对读操作可见（强缓存一致性）。<br>
>&emsp;&emsp;synchronized:作用：修饰实例方法(作用于当前实例加锁) ；修饰静态方法(作用于当前类对象加锁)； 修饰代码块(指定加锁对象，对给定对象加锁)<br>
>


### 对象锁
```text
   public class SynchronizedTest {
        private static ExecutorService 丽春院 = Executors.newFixedThreadLocal(10);
        private volatile boolean 老鸨 = false;
        public static class 客官 implements Runnable {
            private String 姓名;
            public 客官(String 姓名) {
                this.姓名 = 姓名;
            }
            @Override
            public void run() {
                try {
                    清倌(姓名);
                }catch(InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
        
        public static synchronized void 清倌(String 姓名) throws InterruptedException {
            while(true) {
                System.out.println("韦春花与" + 姓名 + "喝茶、吟诗、作对、聊风月";
                if(老鸨) {
                    System.out.println("老鸨敲门：时间到了 \r\n");
                    老鸨 = false;
                    break;
                }
                Thread.sleep(1000);
            }
        }
        
        private static List<String> list = Arrays.asList("鳌大人","陈近南","海大富");
        
        public static void main(String[] args) throws InterruptedException {
            for (int i = 0; i < 3; i++) {
                丽春院.execute(new 客官(list.get(i)));
                Thread.sleep(3000);
                老鸨 = true;
            }
        }
   }
```
### 门栓
```text

```
### 可重入锁
```text

```
### 读写锁
```text

```
### 信号量锁
```text

```