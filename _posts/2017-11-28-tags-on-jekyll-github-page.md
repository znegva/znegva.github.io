---
layout: post
title: Tags with Jekyll
tags: Jekyll snippets
update: 2017-11-29
---

How to use tags on your Jekyll blog (on GitHub-Pages) in three steps:

### Add tags to your posts
Add `tags` to your posts liquid tags,
the liquid tags of this post you are reading right now are:  
``` html
---
layout: post
title: Tags with Jekyll
tags: Jekyll snippets
---
```

### Extend your `_layouts/post.html` file
by something like this:
{% raw %}
``` html
{% if page.tags.size > 0 %}
<span class="tags">
    Tags:&nbsp;
    {% for tag in page.tags %}
        {% capture tag_name %}{{ tag }}{% endcapture %}
        <a href="/tags/#{{ tag_name }}">
            {{ tag_name }}
        </a>
        {% unless forloop.last %},&nbsp;{% endunless %}
    {% endfor %}
</span>
{% endif %}
```
{% endraw %}

This adds the tags listed in the liquid of each post and creates links to
the tags-page.

### Create a new `tags.html` page
{% raw %}
``` html
---
layout: default
title: Tags
permalink: /tags/
---
{% for tag in site.tags %}
    {% capture tag_name %}{{ tag | first }}{% endcapture %}
    <a name="{{ tag_name }}"></a>
    <h1>
        {{ tag_name }}
    </h1>
    <ul class="postlist">
    {% for post in site.tags[tag_name] %}
        <li>
            <a href="{{ post.url }}">
                {{ post.title }}
            </a>
            <span class="date">
                {{ post.date | date: "%d. %b %Y" }}
            </span>
        </li>
    {% endfor %}
    </ul>
{% endfor %}
```
{% endraw %}

This creates an overview page of your tags, beginning with your oldest tags
(the tags you used on your first posts). This can be reversed by changing
{% raw %}`{% for tag in site.tags %}` to `{% for tag in site.tags reversed %}`{% endraw %}.


#### Ordered tags
To order the tags on your tags-page by number of posts-per-tag, you can
change your `tags.html` to:
{% raw %}
``` html
---
layout: default
title: Tags
permalink: /tags/
---
{% assign tags_max = 0 %}
{% for tag in site.tags %}
    {% if tag[1].size > tags_max %}
        {% assign tags_max = tag[1].size %}
    {% endif %}
{% endfor %}


{% for i in (1..tags_max) reversed %}
    {% for tag in site.tags %}
        {% if tag[1].size == i %}
            {% capture tag_name %}{{ tag | first }}{% endcapture %}
            <a name="{{ tag_name }}"></a>
            <h1>
                {{ tag_name }}
            </h1>
            <ul class="postlist">
            {% for post in site.tags[tag_name] %}
                <li>
                    <a href="{{ post.url }}">
                        {{ post.title }}
                    </a>
                    <span class="date">
                        {{ post.date | date: "%d. %b %Y" }}
                    </span>
                </li>
            {% endfor %}
            </ul>
        {% endif %}
    {% endfor %}
{% endfor %}
```
{% endraw %}

This snippet is inspired by [this page](http://blog.jupo.org/2013/05/05/sandboxed-jekyll-hacks/).




### worth knowing
To be able to post html and/or liquid snippets with Jekyll just enclose your
whole code-block with `{{ "{% raw " }} %}` and `{{ "{% endraw " }} %}`.

If you want to show an `{{ "{% endraw " }} %}` ... you have to escape it :-)
