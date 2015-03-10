title: "Java源代码阅读-Object.toString()"
categories: [Java]
tags: [Object.toString]
date: 2014-07-29 19:16:00
---
关于*Object*中的toString()
<!-- more -->
---

先来看一个简单的基类 *Parent*,它有一个`toString()`方法
```java
public class Parent{
	
	public String toString(){
		return "Parent";
	}
}
```
然后you一个派生类*Child*继承*Parent*,它重载了基类*Parent*的`toString()`.
```java
public class Child{

	public String toString(){
		reuturn "Child";
	}
	
	public void get(){
		System.out.println(this);
		System.out.println(new Child());
	}

	public static void main(String[] args){
		new Child().get();
	}
}
```
然后执行*Child*的main方法得到结果如下:
```
Child    
Child
```
*Child*通过 new 调用 `get()`方法分别打印 `this` 和 `new Child()`.它们都是调用了*Child*的`toString()`方法,所以输出结果相同.

如果注释掉*Child*派生类的`toString()`方法,再执行main方法之后,输入结果如下:
```
Parent   
Parent
```
此时*Child*通过 new 调用 `get()`方法分别打印 `this` 和 `new Child()`.它们都是调用了*Parent*的`toString()`方法.

再做进一步的测试,注释掉*Parent*基类的`toString()`方法.查看输出结果如下:
```
com.B@15db9742   
com.B@6d06d69c
```
这些又是从哪里来的呢?我们知道**Java**所有类都有一个总基类就是*Object*,查看源代码之后我们在*Object*中找到如下`toString()`方法.
```java
//注释省略
public String toString() {
	return getClass().getName() + "@" + Integer.toHexString(hashCode());
}
```
从源代码中我们可以得到打印结果的组成方式.由类型的完整相对路径加@符号再加上一段通过`Integer.toHexString()`处理哈希玛`hashCode()`得到的一组字符串组成.实际打印语句可以看成是:
`System.out.println(new B().getClass().getName() + "@" + Integer.toHexString(new B().hashCode()));`.

从以上简单的测试中可以看到,如果想直接打印一个类型的实例时,实际上这个实例调用的*Object*的`toString()`方法.

此时,我又在想为什么打印一个类型的实例时,会调用`toString()`方法呢?