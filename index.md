---
layout: page
title: 我的博客
tagline: Supporting tagline
---
{% include JB/setup %}

{% for post in site.posts %}  
<div class="post">
<h2 class="title"><a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></h2>
<p class="meta"><span class="date">{{ post.date | date_to_string }}</span><span class="posted"> <a href="#">{{ post.author.name }}</a></span></p>
<div style="clear: both;">&nbsp;</div>
	<div class="entry">
        <p>
            {{ post.content | strip_html | truncatewords:1 }} 
        </p>
        <p class="links"><a href="{{ BASE_PATH }}{{ post.url }}" class="more">查看全文</a></p>
    </div>
</div>
{% endfor %}



