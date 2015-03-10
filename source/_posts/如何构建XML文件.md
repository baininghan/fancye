title: "如何构建XML文件"
categories: [Java]
tags: [java构建XML文件]
date: 2014-12-19 15:34:00
---

本文演示了如何构建一个xml格式的字符串。
<!-- more -->
开发语言：`JAVA`
引用包：`org.dom4j`

#### **构建下面一个*XML*格式的字符串**
```java
<?xml version="1.0" encoding="UTF-8"?>
<methodResponse>
	<fault>
		<value>
			<struct>
				<member>
					<name>faultCode</name>
					<value>
						<int>4</int>
					</value>
				</member>
				<member>
					<name>faultString</name>
					<value>
						<string>conferenceID: no such conference</string>
					</value>
				</member>
			</struct>
		</value>
	</fault>
</methodResponse>
```
这个示例是*Cisco TelePresence Server API* 中一个响应字符串。

#### **具体代码如下**
```java
public class Test {

	public static void main(String[] args) {
		/*构建XML头文件，默认编码为UTF-8*/
		Document doc = DocumentHelper.createDocument();
		
		/*构建XML主体部分*/
		Element root = doc.addElement("methodResponse");
		Element element = root.addElement("fault").addElement("value").addElement("struct");
		
		Element member1 = element.addElement("member");
		member1.addElement("name").addText("faultCode");
		member1.addElement("value").addElement("int").addText("4");
		
		Element member2 = element.addElement("member");
		member2.addElement("name").addText("faultString");
		member2.addElement("value").addElement("string").addText("conferenceID: no such conference");
		
		System.out.println(doc.asXML());
	}
}
```
打印的结构是一个没有经过格式化的*XML*字符串，你可以将此字符串保存进任何一个支持*XML*格式化的*IDE*中进行格式化，这样就可以得到上面开头的示例*XML*串。