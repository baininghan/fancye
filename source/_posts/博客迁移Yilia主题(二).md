title: 博客迁移Yilia主题(二)
date: 2015-02-17 21:02:33
categories: [hexo]
tags: [博客迁移Yilia主题]
---

### 修改导航`Margin-top`顶部间距

修改文件目录：`\themes\yilia\source\css\_partial\main.styl`
将`margin-top`的值修改为：10px
```
.header-menu{
		font-weight: 300;
		margin-left: 74px;
		margin-top: 10px;
		margin-bottom: 40px;
		line-height: 35px;
		cursor: pointer;
		text-transform: uppercase;
		float:none;
		li{
			cursor: default;
			a{
				min-width: 300px;
			}
			&:hover{
				font-weight: 900;
			}
		}
	}
```
<!-- more -->
### 外联挂件

在笔记本上(我的是14寸)，如果导航项数目超过三个，会导致`Github`、`Weibo`和`Rss`等这些挂件显示不全，或者完全被遮挡，目前还没找到办法。

---
2015/2/17 Fancye