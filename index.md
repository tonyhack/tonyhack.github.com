---
layout: page
title: Tony's blog
description: focus on server side programming
tagline: C/C++, Linux, network programming, Online Game, etc.
---
{% include JB/setup %}

<div class="row">
  <div class="nine columns">
    <div>
        {% for post in site.posts %}	
            <h3><a href="{{ post.url }}">{{ post.title }}</a></h3>
            <p>
                {{ post.content | strip_html | truncatewords:50}}
		<!--{{ post.content | truncatehtml: 500 }}-->
                <a href="{{ post.url }}" class="read_more">Read more.</a><br/>
            </p>
            <p>
                <strong>
                    {{ post.date | date: "%B %e, %Y" }}
                </strong>
                | {{ post.category }}
                | <a href="http://tonyhack.github.com{{ post.url }}/#disqus_thread" data-disqus-identifier="{{ post.url }}">comments</a>
            </p>
            {% if forloop.last %}
            {% else %}
                <hr>
            {% endif %}
            			
        {% endfor %}
    </div>
  </div>
  
  <div class="two columns offset-by-one">
              <a href="categories.html"><h4>Category</h4></a>
              <strong><ul>
                {% assign categories_list = site.categories %}
                {% include JB/categories_list %}
              </ul></strong>
  </div>

  <div class="two columns offset-by-one">
              <h4>Blogroll</h4>
              <ul>
                  <!--<li><a target="_blank" title="Los Techies" href="http://lostechies.com/"><strong>Los Techies</strong></a></li>
                  <strong><li><a target="_blank" title="" href=""></a></li></strong>-->
              </ul>
  </div>

  

</div>

