title: 博客迁移Yilia主题(五) - 绑定域名
date: 2015-03-03 17:02:24
categories: [hexo]
tags: [绑定域名,博客迁移Yilia主题]
---

之前搭建完博客之后，在绑定自己的域名时，也折腾了很长一段时间，在这里做个简明的记录。
<!-- more -->

### 主域名绑定

例如我的域名为 `baininghan.com`
1. 我们要在`Github Blog`项目下新建一个`CNAME`文件，内容为`baininghan.com`
2. 将我们的域名A记录到 207.97.227.245这个IP

### 子域名绑定

例如 我的子域名为 `blog.baininghan.com`
1. 我们要在`Github Blog`项目下新建一个`CNAME`文件，内容为`blog.baininghan.com`
2. 将我们的域名`CNAME`记录到`xxx.github.io`(例如：baininghan.github.io)

最后需要等待大概10分钟左右的解析，解析期间我们可以
```
ping baininghan.com
```
来方便的获取解析情况。

---
2015-03-03 Fancye