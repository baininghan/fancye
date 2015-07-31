title: "hasCode in Java"
categories: "Java"
tags: [HashCode]
date: 2015-01-13 18:07:00
---
关于`Java`中的`hasCode`。
<!-- more -->
接上文[如何区分同一Class的不同实例对象](http://localhost:4000/2015/01/13/%E5%A6%82%E4%BD%95%E5%8C%BA%E5%88%86%E5%90%8C%E4%B8%80Class%E7%9A%84%E4%B8%8D%E5%90%8C%E5%AE%9E%E4%BE%8B%E5%AF%B9%E8%B1%A1/)，继续深入研究`HashCode`。

一般我们新定义的一个Class类，都会有一个`hashCode()`方法，他是继承自`Object`根类。我们可以查看源码，翻译过来说明如下：
```java
hashcode方法返回该对象的哈希码值。支持该方法是为哈希表提供一些优点，例如，java.util.Hashtable 提供的哈希表。   
  
hashCode 的常规协定是: 
    在 Java 应用程序执行期间，在同一对象上多次调用 hashCode 方法时，必须一致地返回相同的整数，前提是对象上 equals 比较中所用的信息没有被修改。从某一应用程序的一次执行到同一应用程序的另一次执行，该整数无需保持一致。   
    如果根据 equals(Object) 方法，两个对象是相等的，那么在两个对象中的每个对象上调用 hashCode 方法都必须生成相同的整数结果。   
    以下情况不 是必需的：如果根据 equals(java.lang.Object) 方法，两个对象不相等，那么在两个对象中的任一对象上调用 hashCode 方法必定会生成不同的整数结果。但是，程序员应该知道，为不相等的对象生成不同整数结果可以提高哈希表的性能。   
    实际上，由 Object 类定义的 hashCode 方法确实会针对不同的对象返回不同的整数。（这一般是通过将该对象的内部地址转换成一个整数来实现的，但是 JavaTM 编程语言不需要这种实现技巧。）   
  
当equals方法被重写时，通常有必要重写 hashCode 方法，以维护 hashCode 方法的常规协定，该协定声明相等对象必须具有相等的哈希码。
```
以上这段官方文档的定义，我们可以抽出成以下几个关键点：
```
hashCode的存在主要是用于查找的快捷性，如Hashtable，HashMap等，hashCode是用来在散列存储结构中确定对象的存储地址的；
如果两个对象相同，就是适用于equals(java.lang.Object) 方法，那么这两个对象的hashCode一定要相同；
如果对象的equals方法被重写，那么对象的hashCode也尽量重写，并且产生hashCode使用的对象，一定要和equals方法中使用的一致，否则就会违反上面提到的第2点；
两个对象的hashCode相同，并不一定表示两个对象就相同，也就是不一定适用于equals(java.lang.Object) 方法，只能够说明这两个对象在散列存储结构中，如Hashtable，他们“存放在同一个篮子里”。
```