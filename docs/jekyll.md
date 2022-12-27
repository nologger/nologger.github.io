---
layout: default
---

# jekyll

<br>

<ul>
    {% for post in site.posts %}
        {% if post.category == "Jekyll" %}
            <li>
                <a href="{{ post.url | absolute_url }}">
                    [{{ post.date | date : "%Y-%m-%d" }}] {{ post.title }}
                </a>
            </li>
        {% endif %}
    {% endfor %}
</ul>