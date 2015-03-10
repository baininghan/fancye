title: draft_test
tags: hexo tags
---

### Block Quote
{% blockquote @Fancye http://baininghan.com %}
This a blockquote from (@Fancye)[http://baininghan.com]
{% endblockquote%}

### Code Block
{% codeblock .java lang:java http://A.java %}
 class A {
	void print(){
		System.out.print("Hello World!");
	}
 }
{% endcodeblock%}

### Backtick Code Block
>```[language][title][url][link text]
code snippet
```

### Pull Quote
{% pullquote pull_content%}
content
{% endpullquote%}

### jsFiddle
Embeds jsFiddle snippets in posts.
>{% jsfiddle shorttag [tabs] [skin] [width] [height] %}

### Gist
Embeds Gist snippets in posts.
>{% gist gist_id [filename] %}

### iframe
Embeds an iframe in posts.
>{% iframe url [width] [height] %}

### Image
Inserts an image in posts with specified size.
>{% img [class names] /path/to/image [width] [height] [title text [alt text]] %}

### Link
Inserts a link with `target="_blank"` attribute.
>{% link text url [external] [title] %}

### Include Code
Inserts code snippet in `source` folder.
>{% include_code [title] [lang:language] path/to/file %}

### Youtube
Inserts a Youtube video in posts.
>{% youtube video_id %}

### Vimeo
Inserts a Vimeo video in posts.
> {% vimeo video_id %}

### Raw
If there’re some contents can’t be processed in posts, you can wrapped it with `rawblock` tag.
>{% rawblock %}
content
{% endrawblock /%}