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
                <!--{{ post.content | strip_html | truncatewords:75}}-->
		{{ post.content | truncatehtml: 500 }}
                <a href="{{ post.url }}" class="read_more">Read more...</a><br/>
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
                  <li><a target="_blank" title="Los Techies" href="http://lostechies.com/"><strong>Los Techies</strong></a></li>
                  <li><a target="_blank" title="Scott Hanselman" href="http://www.hanselman.com/blog/"><strong>Scott Hanselman</strong></a></li>
                  <li><a target="_blank" title="Haacked" href="http://haacked.com/"><strong>Phil Haack</strong></a></li>
                  <li><a target="_blank" title="Martin Fowler" href="http://martinfowler.com"><strong>Martin Fowler</strong></a></li>
                  <li><a target="_blank" title="Ian Robinson" href="http://iansrobinson.com"><strong>Ian Robinson</strong></a></li>
                  <li><a target="_blank" title="High Scalability" href="http://highscalability.com/"><strong>High Scalability</strong></a></li>
                  <li><a target="_blank" title="Coding Horror" href="http://www.codinghorror.com/blog/"><strong>Coding Horror</strong></a></li>
                  <li><a target="_blank" title="Dr Dobbs" href="http://www.drdobbs.com/"><strong>Dr Dobbs</strong></a></li>
                  <li><a target="_blank" title="Udi Dahan" href="http://www.udidahan.com/?blog=true"><strong>Udi Dahan</strong></a></li>
                  <li><a target="_blank" title="Geek Monkey" href="http://geekmonkey.org/"><strong>Geek Monkey</strong></a></li>
                  <li><a target="_blank" title="Alex Maccaw" href="http://blog.alexmaccaw.com/"><strong>Alex Maccaw</strong></a></li>
                  <li><a target="_blank" title="How To Node" href="http://howtonode.org/"><strong>How To Node</strong></a></li>
                  <li><a target="_blank" title="David Catuhe" href="http://blogs.msdn.com/b/eternalcoding/"><strong>David Catuhe</strong></a></li>
                  <li><a target="_blank" title="Julien Dollon" href="http://julien.dollon.net/"><strong>Julien Dollon</strong></a></li>
                  <!--<strong><li><a target="_blank" title="" href=""></a></li></strong>-->
              </ul>
  </div>

  

</div>

