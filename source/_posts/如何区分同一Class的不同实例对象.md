title: "如何区分同一Class的不同实例对象"
categories: [Java]
tags: [同一Class的不同实例对象]
date: 2015-01-13 17:43:00
---
如何区分同一Class的不同实例对象
<!-- more -->
今天在一个Java群里有一个群友问了一个问题：
```java
public class AClass {

	public static void main(String[] args) {
		SubClass s1 = new SubClass();
		s1.print();
		SubClass s2 = new SubClass();
		s2.print();
	}
	
}

class SubClass{
	 void print(){
		System.out.println("s1 or s2 invoke thie method?");
	}
}
```
`SubClass`实例化了两个对象，分别是`s1`和`s2`，两个对象都调用了`print()`方法，在方法中我们如何得知到底是哪个对象调用了这个方法？

有人说使用`System.out.println(this.getClass().getName());`，这显然是不行的。这样只能得到`SubClass`这个类型的信息，不管是`s1`还是`s2`对象，他们都是属于`SubClass`类型。

我建议使用`System.out.println(this.hashCode());`，这样可以得到不同的对象的哈希码。更改`print()`方法之后，得到结果如下：
```java
1908246931
1385385019
```
这样，不同的对象拥有不同的哈希码，也就拥有了一个唯一标识。