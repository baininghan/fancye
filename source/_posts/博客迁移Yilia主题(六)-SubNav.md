title: 博客迁移Yilia主题(六)-SubNav
date: 2015-03-04 13:03:33
categories: [hexo]
tags: [博客迁移Yilia主题]
---

Yilia主题的左边栏底部有一个SubNav外联，可以放诸如常用的Github、weibo和Rss等。也可以自定义。
<!-- more -->

---

### ejs代码位置

```
<nav class="header-nav">
	<div class="social">
		<% for (var i in theme.subnav){ %>
			<a class="<%= i %>" target="_blank" href="<%- url_for(theme.subnav[i]) %>" title="<%= i %>"><%= i %></a>
		<%}%>
	</div>
</nav>
```

### css样式位置

`css`样式保存在`themes\yilia\source\css\_partial\main.styl`,查找样式`.header-nav .social`，在后面添加

例如增加一个豆瓣的外联
```
a.douban {
	background:url('/img/douban.png') center no-repeat #06c611;
	border:1px solid #06c611;
	&:hover {
		border:1px solid #06c611;
	}
}
```
豆瓣的Logo图片存放在`/img`,相应的图片可以到主站获取。

### 主题配置_config.yml

修改主题配置`_config.yml`（\themes\yilia\_config.yml），在`subnav`一栏下添加你需要的。
```
subnav:
  github: http://github.com/baininghan
  weibo: http://www.weibo.com/fancybai01
  rss: "/atom.xml"
  mail: "mailto:xxx@163.com"
  douban: "http://www.douban.com/people/your_douban_id/"
  # facebook: "#"
  # google: "#"
  # twitter: "#"
  # linkedin: "#"
```

完成，`hexo s`去看看效果吧。

---
2015-03-04 Fancye