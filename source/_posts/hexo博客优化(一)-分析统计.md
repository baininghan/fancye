title: hexo博客优化(一)-分析统计
date: 2015-02-17 22:18:54
categories: [hexo]
tags: [hexo博客优化]
---

### 流量分析

[1] [百度统计](http://tongji.baidu.com)

国内，还是使用百度统计吧。

首先，在主题的`_config.yml`文件中添加：
```
baidu_tongji: true
```
其次，在主题的`/layout/_partial/`目录下新建文件`baidu_tongji.ejs`：
```
<% if (theme.baidu_tongji){ %>
#你的百度统计代码
<% } %>
```
最后，在主题的`/layout/_partial/head.ejs`文件末尾添加：
```
<%- partial('baidu_tongji') %>
```
<!-- more -->
[2] [谷歌统计](http://www.google.com/analytics/)

如果你不惧*被墙*，也可以使用** Google Analytics **。
首先，你需要注册Google Analytics账号，之后获取跟踪ID和跟踪代码
然后，在主题的`_config.yml`中添加获得的跟踪ID：
```
google_analytics:
	enable: true
	id: enter your google analytics id
	site: baininghan.com
```
其次，在主题的`/layout/_partial/`目录下新建文件`google_analytics.ejs`：
```
<% if (theme.baidu_tongji){ %>
#你的百度统计代码
<% } %>
```
最后，在主题的`/layout/_partial/head.ejs`文件末尾添加：
```
<%- partial('google-analytics') %>
```

---
2015-02-17 Fancye