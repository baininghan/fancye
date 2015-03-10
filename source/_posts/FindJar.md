title: "FindJar"
categories: [Java]
tags: [FindJar]
date: 2015-02-05 13:49:00
---

此类用于遍历一个文件目录下可能存在的所有压缩包(.jar)中是否存在指定的Class文件。
<!-- more -->
查找在`D:\Java`目录下的`String.class`所在的`jar`包。
```java
String fileSrc = "D:\\Java";
String className = "String";
FindJar fj = new FindJar();
fj.iteratorDir(fileSrc, className);
List<File> files = fj.jarList;
for(File file : files){
	System.out.println("Your jar name is : " + file.getName() 
	                + ", from : " + file.toString());
}
```
查找指定后缀名的压缩包中想要查找的指定后缀名的文件。例如下面的示例，查找`D:\Java`目录下后缀名是`.zip`的压缩包中存在的`request.htmll`文件。
```
String fileSrc = "D:\\Java";
String className = "request";
FindJar fj = new FindJar("zip", "html");
fj.iteratorDir(fileSrc, className);
List<File> files = fj.jarList;
for(File file : files){
	System.out.println("Your jar name is : " + file.getName() 
	                + ", from : " + file.toString());
}
```
[FindJar源码](https://github.com/baininghan/findJar)