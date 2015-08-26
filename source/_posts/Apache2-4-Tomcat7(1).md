title: 'Apache2.4 + Tomcat7负载均衡和集群（一）'
date: 2015-07-31 20:46:39
categories: [Apache,Tomcat]
tags: [Apache,Tomcat]
---

## 准备安装包
- [apr-1.5.2.tar.gz](http://archive.apache.org/dist/apr/apr-1.5.2.tar.bz2)
- [apr-util-1.5.4.tar.gz](http://archive.apache.org/dist/apr/apr-util-1.5.4.tar.bz2)
- [pcre-8.36.tar.gz](http://www.linuxfromscratch.org/blfs/view/svn/general/pcre.html)
- [httpd-2.4.12.tar.gz](http://apache.fayea.com/httpd/httpd-2.4.12.tar.gz)
- [tomcat-connectors-1.2.40-src.tar.gz](http://mirrors.hust.edu.cn/apache/tomcat/tomcat-connectors/jk/tomcat-connectors-1.2.40-src.tar.gz)

<!-- more -->
## 安装步骤

### 将事先下载好的安装包上传到Linux服务器上
新建一个目录用于存放安装包
```
# cd /usr/local
# mkdir software
```

### 安装apr
```
# cp /usr/local/software/apr-1.5.2.tar.gz /usr/local
# cd /usr/local
# tar -zxvf apr-1.5.2.tar.gz
# cd apr-1.5.2
# ./configure --prefix=/usr/local/apr
# make && make install
```
### 安装apr-util
```
# cp /usr/local/software/apr-util-1.5.4.tar.gz /usr/local
# cd /usr/local
# tar -zxvf apr-util-1.5.4.tar.gz
# cd apr-util-1.5.4
# ./configure --prefix=/usr/local/apr-util
# make && make install
```
### 安装pcre
```
# cp /usr/local/software/pcre-8.36.tar.gz /usr/local
# cd /usr/local
# tar -zxvf pcre-8.36.tar.gz
# cd pcre-8.36.tar.gz
# ./configure --prefix=/usr/local/pcre
# make && make install
```
### 安装httpd
```
# cp /usr/local/software/httpd-2.4.12.tar.gz /usr/local
# cd /usr/local
# tar -zxvf httpd-2.4.12.tar.gz
# cd httpd-2.4.12.tar.gz
# ./configure --prefix=/usr/local/apache --enable-so -enable-proxy -enable-proxy_http=shared --enable-module=so --enable-mods-shared=all --enable-proxy-ajp=shared  --enable-proxy-balancer -with-mpm=worker --with-apr=/usr/local/apr/ --with-apr-util=/usr/local/apr-util/ --with-pcre=/usr/local/pcre
# make && make install
```
### 安装jk
```
# cp /usr/local/software/tomcat-connectors-1.2.40-src.tar.gz /usr/local
# cd /usr/local
# tar -zxvf tomcat-connectors-1.2.40-src.tar.gz
# cd tomcat-connectors-1.2.40-src
# chmod 755 buildconf.sh
# ./buildconf.sh
# ./configure --with-apxs=/usr/local/apache/bin/apxs
# make && make install
# cp ./apache-2.0/mod_jk.so /usr/local/apache/modules/
```
## Apache配置及测试
修改apache的配置文件：
```
# vi /usr/local/apache/conf/httpd.conf
```

### 1.查找`ServerName`，修改：
```
ServerName 162.16.1.229:8000 # 对应Linux服务器的IP
```
### 2.查找`Listen`，修改为：
```
Listen 8000
```
### 3.查找以下内容，并在`index.html`后面添加`index.jsp`
```
<IfModule dir_module>
    <DirectoryIndex index.html index.jsp
</IfModule>
```
### 4.查找以下内容，并去掉前面的注释
```
#Include conf/extra/httpd-mpm.conf
#Include conf/extra/httpd-default.conf

#LoadModule slotmem_shm_module modules/mod_slotmem_shm.so
```

### 5.测试
启动tomcat：`# /url/local/apache/bin/apachectl start`
打开浏览器，输入：http://162.16.1.229:8000 (根据个人机器IP不同),如果页面出现`It's Workds！`说明`Apache`安装成功。
