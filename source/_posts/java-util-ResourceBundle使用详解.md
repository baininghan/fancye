title: "java.util.ResourceBundle使用详解"
categories: [Java]
tags: [ResourceBundle实现国际化]
date: 2015-01-15 10:57:00
---
使用`java.util.ResourceBundle`实现国际化。
<!-- more -->
### 1. 认识国际化资源文件
```
轻松地本地化或翻译成不同的语言
一次处理多个语言环境
以后可以轻松地进行修改，支持更多的语言环境
```

说的简单点，这个类的作用就是读取资源属性文件（`properties`），然后根据`.properties`文件的名称信息（本地化信息），匹配当前系统的国别语言信息（也可以程序指定），然后获取相应的`properties`文件的内容。

使用这个类，要注意的一点是，这个properties文件的名字是有规范的：一般的命名规范是： `自定义名_语言代码_国别代码.properties`，
如果是默认的，直接写为：`自定义名.properties`
比如：
```
myres_en_US.properties
myres_zh_CN.properties
myres.properties
```

当在中文操作系统下，如果`myres_zh_CN.properties`、`myres.properties`两个文件都存在，则优先会使用`myres_zh_CN.properties`，当`myres_zh_CN.properties`不存在时候，会使用默认的`myres.properties`。

没有提供语言和地区的资源文件是系统默认的资源文件。
资源文件都必须是`ISO-8859-1`编码，因此，对于所有非西方语系的处理，都必须先将之转换为`Java Unicode Escape`格式。转换方法是通过JDK自带的工具`native2ascii`.

### 2. 实例

定义三个资源文件，放到src的根目录下面（必须这样，或者你放到自己配置的calsspath下面。
ResourceBundleTest.properties
```
user=admin
password=admin
```
ResourceBundleTest_en_US.properties
```
user=admin
password=admin
```
ResourceBundleTest_zh_CN.properties
```
user=管理员
password=管理员
```
ResourceBundleTest.java
```java
package com.fancy.app.resource;

import java.util.Locale;
import java.util.ResourceBundle;

public class ResourceBundleTest {

	public static void main(String[] args) {
		Locale locale1 = new Locale("zh", "CN");
		ResourceBundle rb1 = ResourceBundle.getBundle("com.fancy.app.resource.ResourceBundleTest", locale1);
		System.out.println(rb1.getString("user"));
		
		ResourceBundle rb2 = ResourceBundle.getBundle("com.fancy.app.resource.ResourceBundleTest", Locale.getDefault());
		System.out.println(rb2.getString("user"));
		
		Locale locale3 = new Locale("en", "US");
		ResourceBundle rb3 = ResourceBundle.getBundle("com.fancy.app.resource.ResourceBundleTest", locale3);
		System.out.println(rb3.getString("user"));
	}
}
```

```java
管理员
管理员
admin
```

如果你碰到类似如下的错误，是因为你没有将properties文件放到src下或者指向classpath里面.方便的做法是你可以在`getBundle()`方法的第一个参数中书写完整的报名，如上面给出的例子。
```java
Exception in thread "main" java.util.MissingResourceException: Can't find bundle for base name ResourceBundleTest, locale zh_CN
	at java.util.ResourceBundle.throwMissingResourceException(ResourceBundle.java:1499)
	at java.util.ResourceBundle.getBundleImpl(ResourceBundle.java:1322)
	at java.util.ResourceBundle.getBundle(ResourceBundle.java:796)
	at com.fancy.app.resource.ResourceBundleTest.main(ResourceBundleTest.java:10)
```

### 3. 认识Locale

`Locale` 对象表示了特定的地理、政治和文化地区。需要 `Locale` 来执行其任务的操作称为语言环境敏感的 操作，它使用 `Locale` 为用户量身定制信息。例如，显示一个数值就是语言环境敏感的操作，应该根据用户的国家、地区或文化的风俗/传统来格式化该数值。
 
使用此类中的构造方法来创建 `Locale`：
```
Locale(String language)
Locale(String language, String country)
Locale(String language, String country, String variant)
``` 
创建完 `Locale` 后，就可以查询有关其自身的信息。使用 `getCountry` 可获取 ISO 国家代码，使用 `getLanguage` 则获取 ISO 语言代码。可用使用 `getDisplayCountry` 来获取适合向用户显示的国家名。同样，可用使用 `getDisplayLanguage` 来获取适合向用户显示的语言名。有趣的是，`getDisplayXXX` 方法本身是语言环境敏感的，它有两个版本：一个使用默认的语言环境作为参数，另一个则使用指定的语言环境作为参数。

语言参数是一个有效的 ISO 语言代码。这些代码是由 ISO-639 定义的小写两字母代码。在许多网站上都可以找到这些代码的完整列表，如：
<http://www.loc.gov/standards/iso639-2/englangn.html>
国家参数是一个有效的 ISO 国家代码。这些代码是由 ISO-3166 定义的大写两字母代码。在许多网站上都可以找到这些代码的完整列表，如：
<http://www.iso.ch/iso/en/prods-services/iso3166ma/02iso-3166-code-lists/list-en1.html>

### 4. 中文资源文件的转码 native2ascii

在jdk的bin目录下有这样一个`native2ascii.exe`文件，使用如下：
native2ascii -encoding GBK ResourceBundleTest_zh.properties ResourceBundleTest_zh1.properties

或者双击`native2ascii.exe`文件，在打开的命令窗口，输入中文回车之后即可得到：


---

原文参考：<http://lavasoft.blog.51cto.com/62575/184605/>