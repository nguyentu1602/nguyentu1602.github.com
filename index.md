---
layout: page
title: Dreaming overdose
tagline:
---
{% include JB/setup %}

### Why this blog?
I was strongly motivated by [Jeff Atwood at codinghorror] [blog_link]. Basically he gives me a cool excuse to just muse the hack out of my mind and promises it will someday, with a probability, gets profound enough (like his).

Hell yeah to that. It recently dawned on me that life is only a certain number of keystrokes away, so why not trying to catch all these elusive thoughts when I still can. Oh, and I also need a place to pin all the shell commands and the maths titbits that take forever for me to figure out but only two and a half seconds for them to escape my mind.

[blog_link]: http://blog.codinghorror.com/how-to-achieve-ultimate-blog-success-in-one-easy-step/

### Posts

I still include the sample post because I must disect it before deleting it.
It's in the `_posts/core-samples` folder.

I can generate a list of posts here - not sure yet what language this is:

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

### More links to documentation
These nice people spent their time to write up the instructions here:

Read [Jekyll Quick Start](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)

Complete usage and documentation available at: [Jekyll Bootstrap](http://jekyllbootstrap.com)

Please read them, dear me.