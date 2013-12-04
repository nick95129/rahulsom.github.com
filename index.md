---
layout: page
title: Hello World!
tagline: 
---
{% include JB/setup %}

Hi! I'm Rahul. I'm a programmer, cyclist and collector of weird facts.

As a programmer, I primarily work on the JVM - java and groovy mostly, but I also use jython 
and scala on and off. As part of my job, I'm now working with [Grails](http://grails.org/). Of 
late I've put a lot of my work on github. So this blog definitely belongs on github.

I work on Healthcare, and having done that for long enough, I tend to appreciate [Captain 
Barbossa](http://www.youtube.com/watch?v=b6kgS_AwuH0); after all, he was a healthcare programmer 
before he became a pirate.

I started road biking in 2009 with a Cannondale CAAD8. In 2010, I acquired environment allergies. 
In 2011, Car Magnetism Syndrome. In 2012, Novice on Clip Pedals Syndrome. Anything related to 
bikes, *I know it*.

This blog is built using an assortment of tools I'm not good at, so it may be rough on the edges. 
However if you like the design, feel free to fork it and don't forget to send me pull requests for 
making it better.

<!--
Read [Jekyll Quick Start](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)

Complete usage and documentation available at: [Jekyll Bootstrap](http://jekyllbootstrap.com)

## Update Author Attributes

In `_config.yml` remember to specify your own data:
    
    title : My Blog =)
    
    author :
      name : Name Lastname
      email : blah@email.test
      github : username
      twitter : username

The theme should reference these variables whenever needed.
    
## Sample Posts

This blog contains sample posts which help stage pages and blog data.
When you don't need the samples anymore just delete the `_posts/core-samples` folder.

    $ rm -rf _posts/core-samples

Here's a sample "posts list".
-->
## Recent Posts

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
<!--
## To-Do

This theme is still unfinished. If you'd like to be added as a contributor, 
[please fork](http://github.com/plusjade/jekyll-bootstrap)!
We need to clean up the themes, make theme usage guides with theme-specific markup examples.
-->

