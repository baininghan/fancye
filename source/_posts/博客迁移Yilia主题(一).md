title: 博客迁移Yilia主题(一)
date: 2015-02-17 10:59:05
categories: [hexo]
tags: [博客迁移Yilia主题]
---

2015-02-17 博客开始使用[Yilia](https://github.com/litten/hexo-theme-yilia)主题。
<!-- more -->

### 导航 Menu

在`\themes\yilia\_config.yml`文件的`menu`中可以添加自定义的导航。
如果想加入某一标签类型的菜单，如增加`Hexo`，点击可以查看所有关于`hexo`的文章，可以在`hexo`前面加上标签目录`/tags`。
```
menu:
  Home: /
  Archives: /archives
  Hexo: /tags/hexo
  # Category: /categories
  About: /about
  Essay: /tags/essay
  # 随笔: /tags/随笔
```
页面效果图如下：
![](/img/menu.png)

### 底部外联 SubNav

** 目前有一个*bug*如果菜单导航多于2个的话，会在14寸屏幕下，显示不完全 **。
```
# SubNav
subnav:
  github: http://github.com/baininghan
  weibo: http://www.weibo.com/fancybai01
  rss: "/atom.xml"
  # facebook: "#"
  # google: "#"
  # twitter: "#"
  # linkedin: "#"
```
页面效果图如下：
![](/img/left_side.png)

### 多说

如果使用**多说**插件，可以在`\themes\yilia\_config.yml`配置如下：
```
duoshuo: fancye
```
**多说**代码模块`duoshuo.ejs`放在目录：`\themes\yilia\layout\_partial\post`。在`\themes\yilia\layout\_partial\article.ejs`进行调用。

另外，评论框的左右`Margin`边距没有留空白，与正文上下对齐。可以通过在`\yilia\source\css\_partial\main.styl`加入以下样式代码：
```
.ds-thread{
	padding: 0px 75px 30px 40px;
	margin-bottom: 15px;
}
```
### Disqus

[Yilia](https://github.com/litten/hexo-theme-yilia)主题的`Disqus`可以通过`\_config.yml`文件的`disqus_shortname: `中自行打开或者关闭。
```
disqus_shortname: Your shortname # 若要关闭，可设置为 false
```

### 归档 Archives

** 归档 **界面的顶部`Margin`没有预留空挡，后期需要自行修改样式。
可以通过在`\yilia\source\css\_partial\archive.styl`中，查找`.archives-wrap`样式，新增以下样式（具体间距可以自己设定）：
```
margin-top: 40px;
```

### 如何禁止文章评论功能

可以通过在`titile.md`文章头中添加`comments:false`来禁止本文章禁止评论。

### 关于自定义样式

样式都可以在`\yilia\source\css`目录下修改，而大部分都在`\yilia\source\css\_partial\main.styl`中。
左边栏的设置可以在`\yilia\layout\_partial\left-col.ejs`中自定义。

---
2015/2/17 Fancye