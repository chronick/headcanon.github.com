---
layout: home
title: headcanon. Don't mess it up.
---
{% include JB/setup %}

#Hi.
##I am headcanon.

But my real name is Nick Donohue!

You can contact me by email at ndonohue at gmail.

I also post on the twitter: [@headcanon](http://www.twitter.com/headcanon)

I make websites and apps, and generally like to mess around with things I can program.

The code that I make (and fork) is on my [github](http:///www.github.com/headcanon).

I also make [music](http://www.soundcloud.com/errorbody)! 

---

##Here are my blogthings:

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
