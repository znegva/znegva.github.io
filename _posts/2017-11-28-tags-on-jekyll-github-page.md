---
layout: post
title: Tags with Jekyll
tags: Jekyll snippets
---

How to use tags on your Jekyll blog (on GitHub-Pages) in three steps:

Add `tags` to your posts liquid tags, this posts liquid tags are:  
``` html
---
layout: post
title: Tags with Jekyll
tags: Jekyll snippets
---
```

Create a new `tags.html` page:  
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

Extend your `_layouts/post.html` file by something like this:
{% raw %}
``` html
{% if page.tags.size > 0 %}
<span class="tags">
    Tags:&nbsp;
    {% for tag in page.tags %}
        {% capture tag_name %}{{ tag }}{% endcapture %}
        <a class="no-underline" href="/tags/#{{ tag_name }}">
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


### worth knowing
To be able to post html and/or liquid snippets with Jekyll just enclose your
whole code-block with `{{ "{% raw " }} %}` and `{{ "{% endraw " }} %}`.

If you want to show `{{ "{% endraw " }} %}` ... you have to escape it :-)
