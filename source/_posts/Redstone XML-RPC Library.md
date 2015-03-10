title: "RedStone XML-RPC library"
date: 2015-03-03 14:11:37
categories: [Java]
tags: [XML-RPC]
---

译文：[RedStone](http://xmlrpc.sourceforge.net/)

XML-RPC的全称是XML Remote Procedure Call，即XML（标准通用标记语言下的一个子集）远程过程方法调用。

它是一套允许运行在不同操作系统、不同环境的程序实现基于Internet过程调用的规范和一系列的实现。这种远程过程调用使用http作为传输协议，XML作为传送信息的编码格式。

XML-RPC的定义尽可能的保持了简单，但同时能够传送、处理、返回复杂的数据结构。
<!-- more -->

---

### **说明**

Redstone 的 XML-RPC 库是一个虽小但却很通用的基于[XML-RPC规范](http://xmlrpc.scripting.com/spec.html)的实现。 它包含以下特点：

>* ***它是一个用于访问XML-RPC服务端的XML-RPC客户端，也是一个XML-RPC容器，用于在Web服务器中作为一个XML-RPC服务发送简单JAVA对象。客户端和服务端都可以通过配置，以套接字的方式直接发送消息流信息，而不需要在内存中构建。 理论上当与服务端和客户端交互时它允许几乎无限量的信息,不使用或依赖于内容长度，虽然这是不符合规范的内容长度(授权使用HTTP头)。***
* ***序列化框架允许用户自定义的序列化器去序列化值，而不需要知道这些值原本在哪个库中。序列化框架可以使用外部的XML - rpc库对Java对象进行编码形成XML流。这些用于序列化Java对象的类包括常见的Map，Collections，Lists，object数组，元数组以及反射序列化器等。***
* ***在JavaScript脚本语言中有一组自定义序列化器可以生成Json信息，它替代XML-RPC信息可以很方便的进行存取数据。这个库还包括一个小的xml - rpc客户端，可以在JavaScript中访问这个库发布的服务。***
* ***基于SAX的XML-RPC解析器，可以在库外使用解析与XML-RPC的信息流。***
* ***一个动态代理生成器允许通过使用常规的java接口来实现XML-RPC服务。这有助于提高生产力和类型安全,同时允许ide提供内容辅助功能。***
* ***调用拦截器,挂在服务器端截取调用服务尽管图书馆出版。拦截器拦截可能调用基于方法名称或参数从客户机发送。基于这些信息,拦截器可以防止调用完成或添加、修改或删除参数在派遣之前调用服务。拦截器也可以重定向调用和检查和修改之前调用的结果发送回客户端。***

所有的这些都被打包成一个39kb的JAR(服务器和客户端)或20kb的JAR(仅客户端)。

### **开发XML-RPC客户端**

当我们开发XML-RPC客户端去访问XML-RPC服务器时，有两种可选方案。我们可以使用`redstone.xmlrpc.XmlRpcClient`类或者`redstone.xmlrpc.XmlRpcProxy`。

通常建议使用`XmlRpcProxy`，你首先要定义一个想使用该服务的XML-RPC接口的普通Java接口。

#### 使用XML-RPC代理

下面的示例演示了如何使用XML-RPC代理，登录和注销在线Atlassian的JIRA服务。通过在命令行上提供登录凭证的方式(为简便起见，`main()`方法抛出所有异常):

##### 示例：

```java
import redstone.xmlrpc.XmlRpcProxy;
import redstone.xmlrpc.XmlRpcFault;

static interface Jira
{
    public String login( String username, String password ) throws XmlRpcFault;
    public void logout( String loginToken ) throws XmlRpcFault;
}

public static void main( String[] args ) throws Exception
{
    Jira jira = ( Jira ) XmlRpcProxy.createProxy( "http://jira.atlassion.com/RPC2", new Class[] { Jira.class } );
    String token = jira.login( args[ 0 ], args [ 1 ] );
    jira.logout( token );
}
```
首先，我们定义一个标准的JAVA接口。这个接口一般定义以`.java`结尾的文件中。我们创建一个动态代理实现Java接口,包括一个XmlRpcClient幕后访问xml - rpc服务在<http://jira.atlassion.com/RPC2>找到。通过这个代理来调用XML-PRC服务的方法。这可以使代码更容易阅读，也更加的类型安全。当创建一个标准的JAVA接口之后，我们可以继续通过IDE工具进行代码的补全工作以及一些其他的帮助工作。

#### 使用XML-RPC客户端

下面的示例演示了如何使用XML-RPC客户端，我们可以通过`XmlRpcClient.invoke()`方法来实现任何我们所做的调用。在请求中，我们可以手动插入一些参数数组Array（或者集合List）。
```java
import redstone.xmlrpc.XmlRpcClient;

public static void main( String[] args ) throws Exception
{
    XmlRpcClient client = new XmlRpcClient( "http://jira.atlassion.com/RPC2" );
    Object token = client.invoke( "jira1.login", new Object[] { args[ 0 ], args [ 1 ] } );
    client.invoke( "jira1.logout", new Object[] { token } );
}
```
与上面的使用代理的示例比较，这种方法并不是类型安全的。

#### **开发XML-RPC服务**

本节假定你比较熟悉开发Web应用程序。在这个例子中，我们需要建立一个servlet文件和一个web.xml，同时在所选择的容器中进行注册。如果你从来没有在一种servlet容器中部署过应用程序（如Jetty或Tomcat），建议你先查阅出的任何一种或任何其它servlet容器提供的文档。

相对于原来的Marqu锟絜XML-RPC的库，Redstone XML-RPC库不包括轻量级Web服务器。相反，该库包括一个可用于在Web应用程序承载XML-RPC服务的servlet容器。如果你有需要一个独立的web服务器的需求，我们可能会在以后添加。然而，已经具有如Jetty以及其他一些优秀的servlet容器，我们觉得没有理由再去实现另一个HTTP服务器。

实现XML-RPC服务的最简单的方式是新创建一个servlet容器，去扩展`redstone.xmlrpc.XmlRpcServlet`并注册Java对象，这些Java对象将用于XML-RPC客户端发送XML-RPC服务请求。这些Java对象被称为调用处理程序，可以是任何普通的Java对象。

#### **实现 XML-RPC servlet**

下面的示例演示如何实现一个XML-RPC服务。
```java
package example;

import java.util.Random;
import java.util.HashMap;
import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import redstone.xmlrpc.XmlRpcServlet;

public class ExampleService extends XmlRpcServlet
{
    public void init( ServletConfig servletConfig ) throws ServletException
    {
        super.init( servletConfig );

        getXmlRpcServer().addInvocationHandler( "SimpleDatabase", new HashMap() );
        getXmlRpcServer().addInvocationHandler( "RandomNumberGenerator", new Random() );
    }
}
```
我们继承`redstone.xmlrpc.XmlRpcServlet`，这样一个新的servelt被构建可以处理XML-RPC请求。我们重写`init()`方法注册我们提供给客户端的处理调用程序。这样，我们有两个处理器，`SimpleDatabase`和`RandomNumberGenerator`。正如你所看到的，你可以使用任何类型的Java对象。在这个例子中，我们注册了一个`HasmMap`对象和一个`Random`对象。客户端可以通过使用`SimpleDatabase.put`,`SimpleDatabase.get`和`RandomNumberGenerator.nextInt`来调用这些对象。

要发布这个服务，你需要有如Tomcat，Jetty，Resin或任何其他的servlet容器。有关如何配置servlet的信息，请参阅下面的部分。

##### **配置 XML-RPC servlet**

在你的项目web.xml文件中使用以下的servlet配置，用于配置xml-rpc servelt配置。
```java
<servlet>
    <servlet-name>xml-rpc</servlet-name>
    <servlet-class>example.ExampleService</servlet-class>
    <init-param>
        <param-name>streamMessages</param-name>
        <param-value>1</param-value>
    </init-param>
    <init-param> <!-- Optional! Defaults to text/xml and ISO-8859-1 -->
        <param-name>contentType</param-name>
        <param-value>text/xml; charset=ISO-8859-1</param-value>
    </init-param>
    <load-on-startup>2</load-on-startup>
</servlet>

<servlet-mapping>
    <servlet-name>xml-rpc</servlet-name>
    <url-pattern>/xml-rpc/*</url-pattern>
</servlet-mapping>
```
在你的web.xml中定义servlet的XmlRpcServlet响应JSON消息，并使用这个库提供的XML-RPC客户端用于JavaScript调用。
```java
<servlet>
    <servlet-name>ajax</servlet-name>
    <servlet-class>example.ExampleService</servlet-class>
    <init-param>
        <param-name>streamMessages</param-name>
        <param-value>1</param-value>
    </init-param>
    <init-param>
        <param-name>contentType</param-name>
        <param-value>text/javascript+json; charset=ISO-8859-1</param-value>
    </init-param>
    <load-on-startup>2</load-on-startup>
</servlet>

<servlet-mapping>
    <servlet-name>ajax</servlet-name>
    <url-pattern>/ajax/*</url-pattern>
</servlet-mapping>
```
最终发布的应用程序，应该看起来是如下的格式：
```java
/WEB-INF
    web.xml
    classes/
        example/ExampleService.class
    lib/
        xmlrpc.jar
```
注册您的Web应用程序（在这种情况下，包含了一个servlet）在servlet容器后，客户将能够访问的服务分别为：“http://yourhost/XML-RPC”或“HTTP://yourhost/AJAX“。

#### **序列化**

每当调用或调用的返回值的参数是要序列化到其XML-RPC对口XmlRpcSerializer类用于幕后。该XmlRpcSerializer知道如何像整数，双打，布尔型，日期，字节数组和字符串序列原始值。如果不是由串行认可的对象提供它要求加入到XmlRpcSerializer任何自定义序列化。
该库包含了自动添加到XmlRpcSerializer几个自定义序列化。这使得收藏，地图，基本数组和其他对象序列化。定制序列很容易实现，如果应用程序需要使用其他类型的对象，在XML-RPC接口。除非应用程序需要使用定制序列它很少访问XmlRpcSerializer类直接。

该XmlRpcSerializer和自定义序列化实施XmlRpcCustomSerializer编写XML-RPC值java.io.Writer中。序列化代码可以被为此库外使用，如果有必要应该存在;
```java
import java.io.Writer;
import java.io.OutputStreamWriter;
import redstone.xmlrpc.XmlRpcSerializer;

public static void main( String[] args ) throws Exception
{
    Writer writer = new OutputStreamWriter( System.out );
    XmlRpcSerializer.serialize( "Hello!", writer );
    XmlRpcSerializer.serialize( new Integer( 42 ), writer );
    writer.flush();
}

控制台输出：<value><string>Hello!</string></value><value><i4>42</i4></value>
```
注意：虽然序列化机制允许自定义序列化以来XML-RPC只支持通用的结构和数组序列化任何对象的原始类型信息未在XML-RPC消息传达的。一旦消息由任何执行用于在结束端反序列化，它会被转换为使用的数据类型来表示<结构>和<阵列>第 对于Redstone XML-RPC，这意味着调用处理程序将接受的`java.util.List`和`java.util.HashMap`中（除了由XML-RPC支持，像integers，doubles，Strings，Dates，booleans和byte[]s。

#### **XML-RPC 以及 JavaScript**

XML-RPC是完全适用于支持在Web应用程序的基本AJAX功能。红石XML-RPC的库包括JavaScript编写一个最小的XML-RPC客户端可被用于调用使用这个库发布的XML-RPC服务。客户端小，由于它依赖于一组由来自使用JSON符号调用处理程序序列化的返回值的库提供的自定义序列的事实。这意味着，XML-RPC的消息被发送到而JSON消息被返回到它可以立即评估，以JavaScript对象客户端的服务器：
```java
<html>
    <head>
        <script src="ajax.js"></script>
    </head>
    <body>
        <p>Result from invocation:</p>
        <script>
            var client = new AjaxService( 'http://example.net/ajax', 'HandlerName' );
            var result = client.invoke( 'methodName', [ 10, 20.5, '42' ] );
            document.write( '<p>' + result + '<p>' );
        </script>
    </body>
</html>
```

#### **温馨提示**

通过`XmlRpcArray` and `XmlRpcStruct`类来实现数组`array`和结构体`struts`的序列化。而不是接受集合或调用处理程序参数列表(或映射的结构)可以使用这些类代替具有调用处理参数接受集合或列表（或地图中的情况下的结构），可以使用这些类。一个XmlRpcArray实际上一个ArrayList和XmlRpcStruct是与从阵列exctracting值而不需要类型转换和转换到基元的几个实用方法一个HashMap：
```java
public void doSomething( XmlRpcArray array )
{
    boolean aBoolean = array.getStruct( 0 ).getBoolean( "someBoolean" );
}
```
下面的例子是旧的处理方式,这是非常好的,如果想要避免依赖Redstone的XML-RPC库。
```java
public void doSomething( List array )
{
    boolean aBoolean = ( ( Boolean ) ( ( Map ) array.get( 0 ) ).get( "someBoolean" ) ).booleanValue();
}
```
上面的两种方法都是合法的，但是第一种无疑更加简短和更具可读性。

#### **下载**

源代码以及JAR包请点击这里 [SourceForge](http://sourceforge.net/projects/xmlrpc/)

---
2015-03-03 Fancye



