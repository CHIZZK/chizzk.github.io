---
layout: post
title: work-exception
date: 2024-06-25
tags: [work]
author: chizzk
---
## ***try-catch-fianlly使用***
### 1.不带return的执行顺序
```java
public class ExceptionTest {
    public static int rank;

    public static void main(String[] args) {
        rank = 1;
        solve2();
    }

    public static void solve1() throws Exception {
        try {
            System.out.println("solve 1 try, rank:" + rank++);
            throw new Exception("throw by solve 1");
        } catch (Exception e) {
            System.out.println("catch exception:" + e.getMessage() + ", rank:" + rank++);
        } finally {
            System.out.println("solve 1 finally, rank:" + rank++);
        }
    }

    public static void solve2() {
        try {
            System.out.println("solve 2 try, rank:" + rank++);
            solve1();
        } catch (Exception e) {
            System.out.println("catch exception:" + e.getMessage() + ", rank:" + rank++);
        } finally {
            System.out.println("solve 2 finally, rank:" + rank++);
        }
    }
}
```
````text
执行结果：
solve 2 try, rank:1
solve 1 try, rank:2
catch exception:throw by solve 1, rank:3
solve 1 finally, rank:4
solve 2 finally, rank:5
````
***结论：<br>1.不带return时，如果try中没有异常，执行顺序为try-finally； 如果try中有异常，执行顺序为try-catch-finally；
<br>2.如果方法有多层内嵌，若里层的方法通过catch进行```捕获```，就不会在外层catch中进行捕获，如果是```(throws)抛出```,则会在上层方法的catch中捕获。***
### 2.带return的执行顺序
```java
public class ExceptionTest {
    public static int rank;
    public static void main(String[] args) {
        System.out.println(testReturn());
        System.out.println("----------------------");
        System.out.println(testReturn2());
        System.out.println("----------------------");
        System.out.println(testReturn3());
        System.out.println("----------------------");
        System.out.println(testReturn4());

    }
   

    //    基本类型  try有return
    public static int testReturn() {
        int i = 1;
        try {
            i++;
            System.out.println("try: " + i);
            return i;
        } catch (Exception e) {
            i++;
            System.out.println("catch: " + i);
        } finally {
            i++;
            System.out.println("finally: " + i);
        }
        return i;
    }
    //引用类型  try中有return
    public static List<Integer> testReturn2() {
        List<Integer> list = new ArrayList<>();
        try {
            list.add(1);
            System.out.println("try: " + list);
            return list;
        } catch (Exception e) {
            list.add(2);
            System.out.println("catch: " + list);
        } finally {
            list.add(3);
            System.out.println("finally: " + list);
        }
        return list;
    }

    //    catch中有return
    public static List testReturn3() {
        List<Integer> list = new ArrayList<>();
        try {
            list.add(1);
            System.out.println("try: " + list);
            int x = 1 / 0;
        } catch (Exception e) {
            list.add(2);
            System.out.println("catch: " + list);
            return list;
        } finally {
            list.add(3);
            System.out.println("finally: " + list);
        }
        return list;
    }

    //    finally中有return
    public static int testReturn4() {
        int i = 1;
        try {
            i++;
            System.out.println("try: " + i);
            return i;
        } catch (Exception e) {
            i++;
            System.out.println("catch: " + i);
        } finally {
            i++;
            System.out.println("finally: " + i);
            return i;
        }
    }
}

```
```text
执行结果：
try: 2
finally: 3
2
----------------------
try: [1]
finally: [1, 3]
[1, 3]
----------------------
try: [1]
catch: [1, 2]
finally: [1, 2, 3]
[1, 2, 3]
----------------------
try: 2
finally: 3
3
```
***结论：<br>
1.try中有return，finally无return，且返回值为基础类型时，执行try,finally中的代码，返回try中return<br>
2.try中有return，finally无return，且返回值为引用类型时，执行try和finally中的代码，返回外层return<br>
3.catch中有return时，执行try，catch，finally中的代码，返回finally中的值；<br>
4.finally中有return时，执行try，catch，finally中的代码，返回finally中的值***



