---
layout: post
title: Java-Basic
date: 2024-07-26
tags: [java]
author: chizzk
---

### 0802-抽象类和接口

抽象类定义：如果一个类中没有足够的信息来描述一个具体的对象，这样的类就是抽象类<br>

抽象类语法：一个类如果被abstract修饰被称为抽象类，抽象类中被abstract修饰的方法被称为抽象方法，抽象方法不用给出具体的实现体。<br>

注意：抽象类也是类，内部可以包含普通方法和属性，甚至构造方法。<br>

特性：
- 抽象类不能实例化对象；
- 抽象方法不能是private的；
- 抽象方法不能被final和static修饰，因为抽象方法要被子类重写；
- 抽象类必须被继承，并且继承后子类要重写父类中的抽象方法，否则子类也是抽象类，必须要使用abstract修饰；
- 抽象类中不一定包含抽象方法，但是有抽象方法的类一定是抽象类；
- 抽象类中可以有构造方法，供子类构建对象时，初始化父类的成员变量；

---
接口定义：接口是一系列方法的声明，是一些方法特征的集合，一个接口只有方法的特征没有方法的实现，因此这些方法可以在不同的地方被不同的类实现，而这些实现可以具有不同的行为。
通俗的讲，接口可以理解为一种特殊的类，里面全部是由全局变量和公共的抽象方法所组成。接口是解决java无法使用多继承的一种手段，但是接口在实际中更多的作用是制定标准的，或者我们可以直接把接口理解为100%的抽象类，即接口中的方法必须全部是抽象方法。（1.8之前）<br>

接口语法：接口的定义格式和定义类的格式基本相同，将class关键字换成interface关键字，就定义了一个接口。<br>
特性：
- 接口类型是一种引用类型，但是不能直接new接口的对象；
- 接口中每一个方法都是public的抽象方法，即接口中的方法会被隐式的指定为public abstract（只能是public abstract）
- 接口中的方法是不能在接口中实现的，只能由实现接口的类来实现；
- 重写接口中方法时，不能使用默认的访问权限；
- 接口中可以含有变量，但是接口中的变量会被隐式的指定为public static final变量；
- 接口中不能有静态代码块和构造方法

---

### Class类：代表了运行时环境中的类和接口的信息
1. 获取类信息的方法
- getName():返回类的全限定名称
- getSimpleName():返回类的简单名称（没有包名的部分）
- getCanonicalName():返回类的规范名称，如果有则返回简单名称加上包名，否则返回null
- getPackage():返回类所在的包
- getModifiers():返回表示类修饰符的整数，例如：public，static，final等

2. 获取构造器和方法的方法
- getConstructors():返回所有公共构造器的数组
- getDeclaredConstructors():返回所有声明的构造器的数组
- getMethods():返回所有公共方法的数组
- getDeclaredMethods():返回所有声明的方法的数组
- getMethod(String name, Class<?>...parameterTypes):返回指定名称和参数类型的公共方法
- getDeclaredMethod(String name, Class<?>...parameterTypes):返回指定名称和参数类型的声明方法

3. 获取字段和注解的方法
- getFields(): 返回所有公共字段的数组
- getDeclaredFields(): 返回所有声明的字段的数组
- getField(String name): 返回指定名称的公共字段
- getDeclaredField(String name): 返回指定名称的声明字段
- getAnnotations(): 返回类上所有注解的数组

4. 实例化和调用方法
- newInstance(): 创建并返回这个类的一个新实例（需要有无参构造器）
- getMethod(String name, Class<?>... parameterType).invoke(Object obj, Object...args): 调用对象上的指定方法

5. 类型和继承结构
- getSuperclass():返回直接超类的Class对象
- getInterfaces():返回实现的所有接口的Class对象数组
- isAssignableFrom(Class<?> cls):测试指定的类是否可以被此类所赋值

6. 其他
- forName(String className): 根据类的字符串名称获取对应的Class对象
- isInstance(): 如果类是一个接口，则返回true
- isPrimitive(): 如果类是一个原始类型，则返回true
- isArray(): 如果类是一个数组，则返回true
- isEnum(): 如果类是一个枚举，则返回true

这些方法使得Java的```反射机制（Reflect）```非常强大，可以动态的创建和操作对象

---

### 0816-位运算符&逻辑运算符



