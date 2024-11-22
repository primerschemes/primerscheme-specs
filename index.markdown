---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
---

{% assign pages = site.pages | where_exp: 'page', 'page.title' %}

<ul>
{% for page in pages %}
<li>
    <a href='{{site.url}}{{page.url}}'>{{page.title}}</a>
    <br>
    </li>
{% endfor %}
</ul>