title: 'Apache2.4 + Tomcat7负载均衡和集群（二）'
date: 2015-07-31 20:46:24
categories: [Apache,Tomcat]
tags: [Apache,Tomcat]
---

接上篇[Apache2.4 + Tomcat7负载均衡和集群（一）](https://zybuluo.com/Fancy-Bai/note/118782)
## JK配置
打开`httpd.conf`配置文件
```
# vi /usr/local/apache/conf/httpd.conf
```
在文件最后加入以下jk配置内容
```
Include conf/mod_jk.conf
```
新建`mod_jk.logs`，`mod_jk.shm`，`mod_jk.conf`文件，并在`mod_jk.conf`文件中添加相关配置内容
```
# cd /usr/local/apache/logs
# touch mod_jk.logs
# touch mod_jk.shm
# cd /usr/local/apache/conf
# touch mod_jk.conf
# vi mod_jk.conf
```
```
LoadModule jk_module modules/mod_jk.so
<IfModule mod_jk.c>
    JKWorkersFile /usr/local/apache/conf/workers.properties
    JKLogFile /usr/local/apache/logs/mod_jk.logs
    JKLogLevel info
    JKLogStampFormat "[%a %b %d %H:%M:%S %Y]"
    JKOptions +ForwardKeySize +ForwardURICompat -ForwardDirectories
    JKRequestLogFormat "%w %V %T %q %U %R"
    JKShmFile /usr/local/apache/logs/mod_jk.shm
    JKMount /* loadbalancer
</IfModule>
```
新建`workers.properties`文件，并添加配置内容
```
# cd /usr/local/apache/conf
# touch workers.properties
# vi workers.properties
```
```
# workers.properties
#
# in unix, we use forward slashes:
ps=/

worker.list=tomcat1,tomcat2,loadbalancer,status

# tomcat1
# tomcat的server.xml文件中ajp13协议的端口号，默认是8009，
# 如果tomcat部署在不同的机器上端口号可不改
worker.tomcat1.port=8009
worker.tomcat1.host=162.16.1.229
worker.tomcat1.type=ajp13
worker.tomcat1.lbfactor=1

# 其他配置参数
# worker.tomcat1.cachesize=100
# worker.tomcat1.cachesize_timeout=600
# worker.tomcat1.reclycle_timeout=300
# worker.tomcat1.socket_keepalive=1
# worker.tomcat1.socket_timeout=300
# worker.tomcat1.local_worker=1
# worker.tomcat1.retries=3

# tomcat2
worker.tomcat2.port=9009
worker.tomcat2.host=162.16.1.229
worker.tomcat2.type=ajp13
worker.tomcat2.lbfactor=1 # 负载均衡权重值（1-100）

# load balancer worker
worker.loadbalancer.type=lb
worker.loadbalancer.balanced_workers=tomcat1,tomcat2
worker.loadbalancer.sticky_session=1
worker.loadbalancer.sticky_session_force=0

worker.status.type=status
```
## Tomcat配置

打开`tomcat/conf/server.xml`修改相关内容
1. 查找`<Server port="9007" shutdown="SHUTDOWN">`，修改此处端口号，我这里改为`9007`
2. 查找`HTTP/1.1`协议配置的`<Connector>`，因为我们使用的是mod_jk模式，此处可以不用修改，如果想越过Apache代理直接用http协议的方式访问web服务可以修改。也可以注释掉此段标签用于禁止http访问。
3. 查找`AJP/1.3`协议配置的`<Connector>`，修改默认端口号8009，如果tomcat部署在不同的机器上，则不需要修改。
```
<Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />
```

4.查找`<Engine>`标签，修改为：
```
<Engine name="Catalina" defaultHost="localhost" jvmRoute="tomcat1" >
```
我测试使用了两个tomcat，另一个tonmcat可以改为`tomcat2`
5. 在`<Engine>`标签内查找`<Cluster>`标签，并打开注释。默认的已经足够使用，也可以替换为以下更详细的内容：
```
<Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster"
        channelSendOptions="8"> 
        <Manager className="org.apache.catalina.ha.session.BackupManager"
          expireSessionsOnShutdown="false"
          notifyListenersOnReplication="true"
          mapSendOptions="6"/> 
  <Channel className="org.apache.catalina.tribes.group.GroupChannel"> 
    <Membership className="org.apache.catalina.tribes.membership.McastService"
                address="228.0.0.4"
                port="45564"
                frequency="500"
                dropTime="3000"/> 
    <Receiver className="org.apache.catalina.tribes.transport.nio.NioReceiver"
              address="162.16.1.229"
              port="4000"
              autoBind="100"
              selectorTimeout="5000"
              maxThreads="6"/> 
    <Sender className="org.apache.catalina.tribes.transport.ReplicationTransmitter"> 
      <Transport className="org.apache.catalina.tribes.transport.nio.PooledParallelSender"/> 
    </Sender> 
    <Interceptor className="org.apache.catalina.tribes.group.interceptors.TcpFailureDetector"/> 
    <Interceptor className="org.apache.catalina.tribes.group.interceptors.MessageDispatch15Interceptor"/> 
  </Channel> 
  <Valve className="org.apache.catalina.ha.tcp.ReplicationValve"
        filter=""/> 
  <Valve className="org.apache.catalina.ha.session.JvmRouteBinderValve"/> 
  <!--<Deployer className="org.apache.catalina.ha.deploy.FarmWarDeployer"
            tempDir="/tmp/war-temp/"
            deployDir="/tmp/war-deploy/"
            watchDir="/tmp/war-listen/"
            watchEnabled="false"/> -->
  <ClusterListener className="org.apache.catalina.ha.session.JvmRouteSessionIDBinderListener"/> 
  <ClusterListener className="org.apache.catalina.ha.session.ClusterSessionListener"/> 
</Cluster>
```
## web项目
web项目的web.xml中最后添加
```
<distributable />
```
这样才可以达成Session复制的功能。
至此所有配置已经完成。

## 测试
### 负载均衡集群Session测试
创建测试工程SessionTest

① 新建一个Java Web工程SessionTest，在工程下新建index.jsp，文件内容如下：
```
<%@ pageimport="java.util.*" %>

<html>

<head>

    <title> Session Test</title>

</head>

<body bgcolor="red">

<%

    out.print("session Id:"+session.getId());

%>

<formaction="index.jsp" method="post">

    key<input type="text" name="key"/>

    <br />

    value<input type="text"name="value" />

    <br />

    <input type="submit"value="Submit" />

    <br />

</form>

<%

    String key =request.getParameter("key");

    if(key!=null &&key.isEmpty()==false)

    {

        String value =request.getParameter("value");

        session.setAttribute(key, value);

        Enumeration e =session.getAttributeNames();

        while (e.hasMoreElements())

        {

            String sKey = (String)e.nextElement();

            String sValue = (String)session.getAttribute(sKey);

            out.print(sKey+ " ="+sValue+"<br>");

        }

    }

%>

</body>

</html>
```
修改SessionTest的web.xml，在web.xml末尾的`</web-app>`标签里添加`<distributable/>`。

将该项目放入同一组的tomcat的webapps下.

### session粘性测试
启动同组tomcat以及apache。浏览器访问apache所在的主机IP地址：http://162.16.1.229/, 从页面显示的session Id，假设请求访问的是tomcat1。多次刷新或submit后，请求访问的一直是tomcat1，并且session Id一直保持不变，session中的数据也能够保持，说明session粘性良好。
### session复制测试
启动同组tomcat以及apache。浏览器访问apache所在的主机IP地址：http://162.16.1.229/,假设页面显示的session Id是tomcat1。提交几组session数据。然后停掉tomcat1，再次提交session数据，或刷新页面，如果请求被转发给tomcat2，页面访问正常。页面颜色改变，但Session Id保持不变，session数据也全部传递给tomcat2，表明session复制良好。