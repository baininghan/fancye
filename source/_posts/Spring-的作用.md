title: "Spring 的作用"
categories: [Java]
tags: [Spring]
date: 2014-09-26 22:37:00
---

Spring 的作用。
<!-- more -->
1.有些参数在某些阶段中是常量

比如：
	1. 在开发阶段我们连接数据库时的连接url，username，password，driverClass等 

	2. 分布式应用中client端访问server端所用的server地址，port，service等  

	3. 配置文件的位置

2.而这些参数在不同阶段之间又往往需要改变

比如：在项目开发阶段和交付阶段数据库的连接信息往往是不同的，分布式应用也是同样的情况。

期望：能不能有一种解决方案可以方便我们在一个阶段内不需要频繁书写一个参数的值，而在不同阶段间又可以方便的切换参数配置信息

解决：spring3中提供了一种简便的方式就是context:property-placeholder/元素

只需要在spring的配置文件里添加一句：<context:property-placeholder location="classpath:jdbc.properties"/> 即可，
这里location值为参数配置文件的位置，参数配置文件通常放在src目录下，而参数配置文件的格式跟java通用的参数配置文件相同，即键值对的形式，例如：

#jdbc配置
```java
test.jdbc.driverClassName=com.mysql.jdbc.Driver
test.jdbc.url=jdbc:mysql://localhost:3306/test
test.jdbc.username=root
test.jdbc.password=root
```
行内#号后面部分为注释

应用：

1.这样一来就可以为spring配置的bean的属性设置值了，比如spring有一个jdbc数据源的类DriverManagerDataSource

在配置文件里这么定义bean：
```java
<bean id="testDataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
    <property name="driverClassName" value="${test.jdbc.driverClassName}"/>
    <property name="url" value="${test.jdbc.url}"/>
    <property name="username" value="${test.jdbc.username}"/>
    <property name="password" value="${test.jdbc.password}"/>
</bean>
```
2.甚至可以将${ }这种形式的变量用在spring提供的注解当中，为注解的属性提供值

外在化应用参数的配置

在开发企业应用期间，或者在将企业应用部署到生产环境时，应用依赖的很多参数信息往往需要调整，比如LDAP连接、RDBMS JDBC连接信息。
对这类信息进行外在化管理显得格外重要。PropertyPlaceholderConfigurer和PropertyOverrideConfigurer对象，它们正是担负着外在化配置应用参数的重任。

<context:property-placeholder/>元素

PropertyPlaceholderConfigurer实现了BeanFactoryPostProcessor接口，它能够对<bean/>中的属性值进行外在化管理。开发者可以提供单独的属性文件来管理相关属性。
比如，存在如下属性文件，摘自userinfo.properties。
```java
db.username=scott
db.password=tiger
```
如下内容摘自propertyplaceholderconfigurer.xml。
正常情况下，在userInfo的定义中不会出现${db.username}、${db.password}等类似信息，这里采用PropertyPlaceholderConfigurer管理username和password属性的取值。
DI容器实例化userInfo前，PropertyPlaceholderConfigurer会修改userInfo的元数据信息（<bean/>定义），
它会用userinfo.properties中db.username对应的scott值替换${db.username}、db.password对应的tiger值替换${db.password}。
最终，DI容器在实例化userInfo时，UserInfo便会得到新的属性值，而不是${db.username}、${db.password}等类似信息。
```java
<bean id="propertyPlaceholderConfigurer"   
        class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">  
    <property name="locations">  
        <list>  
            <value>userinfo.properties</value>  
        </list>  
     </property>  
</bean>  
 
<bean name="userInfo" class="test.UserInfo">
	<property name="username" value="${db.username}"/>  
	<property name="password" value="${db.password}"/>  
</bean> 
```
 
通过运行并分析PropertyPlaceholderConfigurerDemo示例应用，开发者能够深入理解PropertyPlaceholderConfigurer。
为简化PropertyPlaceholderConfigurer的使用，Spring提供了<context:property-placeholder/>元素。
下面给出了配置示例，启用它后，开发者便不用配置PropertyPlaceholderConfigurer对象了。
 
```java
<context:property-placeholder location="userinfo.properties"/>
```
PropertyPlaceholderConfigurer内置的功能非常丰富，如果它未找到${xxx}中定义的xxx键，它还会去JVM系统属性（System.getProperty()）和环境变量（System.getenv()）中寻找。
通过启用systemPropertiesMode和searchSystemEnvironment属性，开发者能够控制这一行为。

---
原文链接：http://blog.sina.com.cn/s/blog_4550f3ca0100ubmt.html