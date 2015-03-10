title: "java.lang.ThreadLocal类简单示例"
date: 2015-03-03 14:11:14
categories: [Java]
tags: [java.lang.ThreadLocal]
---

JDK 1.2的版本中就提供java.lang.ThreadLocal，ThreadLocal为解决多线程程序的并发问题提供了一种新的思路。

使用这个工具类可以很简洁地编写出优美的多线程程序，ThreadLocal并不是一个Thread，而是Thread的局部变量。
<!-- more -->

### 源码示例

```java
import java.util.Random;

/**
 * <code>java.lang.ThreadLocal</code>简单示例
 * <p>@Author baininghan
 * <p>@Date 2015年3月3日
 * <p>@version 1.0
 *
 */
public class ThreadLocalDemo implements Runnable{

	private static final ThreadLocal<Student> threadLocal = new ThreadLocal<Student>();
	
	public static void main(String[] args) {
		ThreadLocalDemo demo = new ThreadLocalDemo();
		Thread t1 = new Thread(demo, "t1");
		Thread t2 = new Thread(demo, "t2");
		t1.start();
		t2.start();
	}
	
	@Override
	public void run() {
		Student student = getStudent();
		student.setAge(new Random().nextInt(100));
		System.out.println("Thread[" + Thread.currentThread().getName() + "] is running...");
		System.out.println("First read Student's age : " + student.getAge());
		try {
			Thread.sleep(1000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		
		System.out.println("Second read Student's age : " + student.getAge());
		
		clearStudent();
	}
	
	public Student getStudent(){
		// 获取当前线程调用对象的一个局部变量（副本）
		Student student = threadLocal.get();
		// 第一次调用 get()，执行的是initialValue()，返回肯定是null，可以通过重写initialValue()方法，获得自己的返回值
		// 第二次调用就会得到set()设置进去的值
		if(student == null){
			student = new Student();
			threadLocal.set(student);
		}
		
		return student;
	}
	
	/** 回收Student */
	public void clearStudent(){
		Student student = threadLocal.get();
		threadLocal.set(null);
		if(student != null){
			student = null;
		}
	}
	
	class Student {
		private int age;
		
		public void setAge(int age){
			this.age = age;
		}
		public int getAge(){
			return age;
		}
	}
}
```

### 输出结果

```java
Thread[t1] is running...
Thread[t2] is running...
First read Student's age : 16
First read Student's age : 99
Second read Student's age : 99
Second read Student's age : 16
```


参考链接：[深入研究java.lang.ThreadLocal类](http://lavasoft.blog.51cto.com/62575/51926/)

---
2015-03-03 Fancye



