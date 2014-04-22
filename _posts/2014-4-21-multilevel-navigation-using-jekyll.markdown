---
comments: true
date: 2014-4-21 14:42:38
layout: post
slug: multilevel-navigation-using-jekyll
title: Multilevel navigation using Jekyll
summary: How you can easily generate multilevel navigation using Jekyll
image: PostNo3.jpg
tags:
- Jekyll
- Front-end
- HTML
- CSS
- Coding
---

Today I'll show you how easily you guys can create a multilevel navigation in Jekyll.

*Step 1* : Let's define hierarchical structure for our navigation in  `_config.yml` file.

	navigation:
	- root: Root Menu Text
	  url: /link.html
	  icon: icon-you-want

	- root: Root Menu Text 1
	  url: #
	  icon: icon-you-want-1
	  first-level:
	    - text: First Level 1
	      url: first-level-link.html
	      second-level:
	        - text: Second Level
	          url: second-level-link.html
	    - text: First Level 2
	      url: first-level-2.html



<br><br>
  *Step 2* : Now, open your HTML file where you want it to be rendered. Here it is the liquid template code.

```
{% raw %}
	<ul class="navigation" style="clear:both;">
	  {% for link in site.navigation %}
	    {% assign url = page.url|remove:'index.html' %}
	    {% assign hasFirst = link.first-level != undefined %}		   
	    <li 
	    	{% if hasFirst %} class="nested" {% endif%} {% if url == link.url %} class="current"{% endif %}>
			{% if hasFirst %} 
				<ul class="first-level">
			    	{% for firstlevel in link.first-level %}  
			    		<li {% if url == firstlevel.url %} class="current" {% endif %} >
			    			{% assign hasSecond = firstlevel.second-level != undefined %}
		    				{% if hasSecond %} 
			    				<ul class="second-level">
			    					{% for secondlevel in firstlevel.second-level %}
										<li>
											<a href="{{ secondlevel.url }}">{{ secondlevel.text }}</a>
										</li>
			    					{% endfor %}
			    				</ul>
		    				{% endif %}
			    			<a {% if firstlevel.target != undefined %} target="{{firstlevel.target}}" {% endif %} href="{{ firstlevel.url }}">{{ firstlevel.text }}{% if hasSecond %} <i class='icon-right-dir-2'></i>{% endif %}</a>
		    			</li>
			  		{% endfor %}
		  		</ul>			    	
			{% endif %}
	  		<a href="{{link.url}}"  class="{{link.icon}}">{{link.root}} {% if hasFirst %} <i class='icon-down-dir-2'></i>{% endif %}</a>
	    </li>
	  {% endfor %}
	</ul>
	{% endraw %}
```
<br><br>
*Step 3* :  Step 2 will generate markup for you! It will also maintain on which page you're on right now and based on that set a class to respective `<li>`. Here are CSS classes you can use to style your generated markup.


```
.navigation {
    margin-bottom: 0;
    margin-left: 0;
    margin-top: 20px;
    list-style-type:none;
}
.navigation li {
    float: right;
    position: relative;
    list-style: none;
}
.navigation li a {
    color: #333;
    font-weight: normal;
    font-size: 12px;
    border-radius: 5px;
    padding: 8px 10px;
    display: block;
    font-weight: bold;
    text-transform: uppercase;
    white-space: nowrap;
    line-height:25px;
}
.navigation ul li a {
    border-radius: 0;
    padding: 5px 10px;
    margin:5px;
    font-weight: normal;
    text-transform: initial;
}
.navigation li > ul {
    background:white;
    position:absolute;
    list-style-type: none;
    top: 30px;
    margin: 0;
    padding: 0;
    border:2px solid #666;
    border-radius: 5px;
    left:0;
    display: none;
    min-width: 100%;
}
.navigation li:hover > ul {
    display: block;
    list-style: none;
}
.navigation li ul li {
    display: block;
    float: none;
    margin-left: 0;
}
.navigation li ul li a:hover {
    border-radius: 0;
    color: #444;
    background: #eeeeee;
}
.navigation > li a:hover, .navigation li > ul:hover + a, .navigation li.current > a {
    color: #86569A;
}
.navigation li.current > a {
    font-weight: bold
}
.navigation li a[href^=http]:after {
    font-family:"fontello";
    font-style: normal;
    font-weight: normal;
    content:'\e9c8';
    speak: none;
    display: inline-block;
    text-decoration: inherit;
    width: 1em;
    margin-right: .3em;
    text-align: center;
    font-variant: normal;
    text-transform: none;
    line-height: 1em;
    margin-left: .3em;
}
.navigation .second-level {
    left: 100%;
    top: 0;
}

```

<br><br>
Here it is what it will look like! I've recently impleted this in the [project](http://www.go.cd) I'm contributing to.

![](/img/posts/Jekyll-nav.png)