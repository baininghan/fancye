title: 博客迁移Yilia主题(三)
date: 2015-02-26 14:37:36
categories: [hexo]
tags: [博客迁移Yilia主题]
---

### 标题颜色修改

修改归档文章title鼠标滑过的颜色。修改路径为`\yilia\source\css\_partial\archive.styl`
```
.archive-article-title {
    font-size: 16px;
    color: #333;
    transition: color 0.3s;
    &:hover{
      color: #88ACDB;//归档文章标题鼠标悬浮颜色
    }
// other setter
```
<!-- more -->
### 头像logo修改

修改使得点击头像Logo也可以回到首页。修改路径`\yilia\layout\_partial\left-col.ejs`
```
<div class="intrude-less">
	<header id="header" class="inner">
		<div class="profilepic">
			<a href="/"><img src="<%=theme.avatar%>"></a>
		</div>
<!-- other setter -->
```