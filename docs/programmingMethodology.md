---
layout: default
---

# programmingMethodology

<br>

<ul>
    {% for post in site.posts %}
        {% if post.category == "ProgrammingMethodology" %}
            <li>
                <a href="{{ post.url | absolute_url }}">
                    [{{ post.date | date : "%Y-%m-%d" }}] {{ post.title }}
                </a>
            </li>
        {% endif %}
    {% endfor %}
</ul>