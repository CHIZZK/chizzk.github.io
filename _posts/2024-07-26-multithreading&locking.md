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
public class CountDownLatchTest {

    public static void main(String[] args) throws InterruptedException {
        List<String> list = Arrays.asList("January","February","March","April","May","June","July","August","September","October");

        CountDownLatch countDownLatch = new CountDownLatch(10);
        ExecutorService exec = Executors.newFixedThreadPool(10);
        for (int i = 0; i < 10; i++) {
            int idx = i;
            exec.execute(() ->{
                try{
                    System.out.println(list.get(idx) + " is coming");
                    Thread.sleep(3000);
                }catch (Exception e) {
                }finally {
                    countDownLatch.countDown();
                }
            });
        }
        countDownLatch.await();
        System.out.println("no November and December");
        exec.shutdown();
    }
}

```
### 可重入锁
```text
public class ReentrantLockTest {

    private static ReentrantLock lock = new ReentrantLock(true);

    private static List<String> list = Arrays.asList("路人甲", "路人乙", "路人丙", "路人丁", "路人戊", "路人己", "路人庚", "路人壬", "路人癸", "韦小宝");

    public static void main(String[] args) {
        for (int i = 0; i < 10; i++) {
            int idx = i;

            new Thread(() -> {
                try {
                    招收杂役(list.get(idx));
                } catch (InterruptedException ignore) {
                }
            }).start();

            if (idx == 9) {
                new Thread(() -> {
                    招收太监(list.get(idx));
                }).start();
            }
        }
    }

    public static void 招收杂役(String name) throws InterruptedException {
        lock.lock();
        try {
            while (true) {
                System.out.println(name + "，排队等待进宫当杂役...");
                Thread.sleep(1000);
            }
        } finally {
            lock.unlock();
        }
    }

    public static void 招收太监(String name) {
        System.out.println(name + "，进宫当太监，不用排队！");
    }
}

```
### 读写锁
```text
public class ReentrantReadWriteLockTest {
    private static final ReentrantReadWriteLock reentrantReadWriteLock = new ReentrantReadWriteLock();
    private static final ReentrantReadWriteLock.ReadLock readLock = reentrantReadWriteLock.readLock();
    private static final ReentrantReadWriteLock.WriteLock writeLock = reentrantReadWriteLock.writeLock();
    private static Deque<String> deque = new ArrayDeque<>();
    
    
    
    
    public static String get() {
        readLock.lock();
        try {
           return deque.poll();
        } finally {
            readLock.unlock();
        }
    }
    
    public static void put(String str) {
        writeLock.lock();
        try {
            deque.add(str);
        } finally {
            writeLock.unlock();
        }
    }
    public static void main(String[] args) {
        new Thread(()->{
            while (true) {
                put("1");
                put("2");
                put("3");
                put("4");
                put("5");
                put("6");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                }
            }
        }).start();
        
        
        new Thread(()->{ while (true) {
            System.out.println("得到的数字是"+get());
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                
            }
            }
        }).start();
    }
}
```
