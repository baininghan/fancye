title: "Java 中那些不常用的关键字"
categories: [Java]
tags: [Java关键字]
date: 2014-09-01 21:09:00
---
**instanceof**
一个二操作数的Java(TM)语言关键字，用来测试第一个参数的运行时类型是否和第二个参数兼容。
<!-- more -->
**final**
一个Java语言的关键字。你只能定义一个实体一次，以后不能改变它或继承它。更严格的讲：一个final修饰的类不能被子类化，一个final修饰的方法不能被重写，一个final修饰的变量不能改变其初始值。

**native** 本地
**strictfp** 严格,精准
**synchronized** 线程,同步

**transient** 
Java语言的关键字，用来表示一个域不是该对象串行化的一部分。当一个对象被串行化的时候，transient型变量的值不包括在串行化的表示中，然而非transient型的变量是被包括进去的。

**volatile**
Java语言的关键字，用在变量的声明中表示这个变量是被同时运行的几个线程异步修改的。