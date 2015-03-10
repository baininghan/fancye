title: "关于List的几个方法"
categories: [Java]
tags: [java.util.List]
date: 2014-09-01 22:47:00
---

## 关于List的几个方法
<!-- more -->
### 1.retainAll(Collection<?> paramCollection);
例如：
```java
List list1 = new ArrayList();
List1.add("a");
list2.add("b");
List list2 = new ArrayList();
list1.add("a");
list2.add("c");	
list1.retainAll(list2);

System.out.println(list1);   
```

打印结果得到：
```java
[a]
```
### 2.indexOf(Object obj)方法和lastIndexOf(Object obj)方法的区别
在使用List集合时需要注意区分indexOf(Object obj)方法和lastIndexOf(Object obj)方法，前者是获得指定对象的最小的索引位置，而后者是获得指定对象的最大的索引位置，前提条件是指定的对象在List集合中具有重复的对象，否则如果在List集合中有且仅有一个指定的对象，则通过这两个方法获得的索引位置是相同的.
```java
List list = new ArrayList();
list.add("a");
list.add("b");
list.add("c");
list.add("a");
list.add("B");
list.add("C");
System.out.println(list.indexOf("a"));
System.out.println(list.lastIndexOf("a"));
```
打印结果得到：
```java 
0
3
```java
###3．subList(int fromIndex, int toIndex)方法
在使用subList(int fromIndex, int toIndex)方法截取现有List集合中的部分对象生成新的List集合时，需要注意的是，新生成的集合中包含起始索引位置代表的对象，但是不包含终止索引位置代表的对象，例如执行下面的代码：
```java
String a = "A", b = "B", c = "C", d = "D", e = "E";
List<String> list = new ArrayList<String>();
list.add(a); // 索引位置为 0
list.add(b); // 索引位置为 1
list.add(c); // 索引位置为 2
list.add(d); // 索引位置为 3
list.add(e); // 索引位置为 4
list = list.subList(1, 3);// 利用从索引位置 1 到 3 的对象重新生成一个List集合
for (int i = 0; i < list.size(); i++) {
System.out.println(list.get(i));
}
```
打印结果如下：
```java
B
C
```