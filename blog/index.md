---
framework: thinny
layout: default
title: Home
mode: selfcontained
---

<div id="post-list">
  {{# pages }}{{# date }}
		<div>
			<span class="post-date">{{ date }}</span>
			<h1><a href="{{ link }}">{{ title }}</a></h1>
			<p class="post-description">{{ description }}</p>
		</div>
		<br>
	{{/ date }}{{/ pages }}
</div>


<footer class="clean" style="background-image: url({{site.url}}{{ site.cover }}); background-position: bottom; height: 60px;">
	<p class="copyright">&copy;{{ site.time | date: "%Y" }}, <a href="{{ site.copyright.url }}" target="_blank">{{ site.copyright.author }}</a>. <a href="{{ site.copyright.type_url }}" target="_blank">{{ site.copyright.type_title }}</a>.</p>
</footer>
