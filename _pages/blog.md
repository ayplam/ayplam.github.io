---
layout: galileo_default
title:  Blog History
permalink: /blog/
---	


<style>		

.post-meta {
    font-size: 14px;
    color: #828282;
}

.post-link {
    display: block;
    font-size: 24px;
}

.light-font {
    color: #b5b5b5;
}

a {
    color: #2a7ae2;
    text-decoration: none;
}
</style>

<h1>Posts</h1>
<br><br>
{% for post in site.posts %}
<span class="post-meta">{{ post.date | date: "%b %d, %Y" }}</span>
<span class="post-link"><a href="{{ post.url }}">{{ post.title }} </a></span>
<p class="small light-font">
{{ post.description }}
</p>
{% endfor %}