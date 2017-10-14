---
layout: galileo_default
title:  Blog History
permalink: /blog/
---	


<style>		

.post-meta {
	font-family: Palatino;
    font-size: 16px;
    color: #828282;
}

.post-link {
	font-family: Palatino;
    display: block;
    font-size: 24px;
    margin: 10px 0px 0px 0px;
}

.light-font {
	font-family: Palatino;
    color: #b5b5b5;
}

a {
	font-family: Palatino;
    color: #2a7ae2;
    text-decoration: none;
}
</style>

# Posts

{% for post in paginator.posts %}
  HELLO
  {% include post.html post=post content=post.content %}
{% endfor %}

{% if paginator.total_pages > 1 %}
    {% include pagination.html maxPages=5 %}
{% endif %}


{% for post in site.posts %}
<span class="post-meta">{{ post.date | date: "%b %d, %Y" }}</span>
<span class="post-link">
	<a href="{{ post.url }}">{{ post.title }} </a>
</span>
<p class="small light-font">
{{ post.description }}
</p>
<span class="post">
{% endfor %}

