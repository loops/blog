---
layout: theme:post
---
{% content_for head %}
<title>{{ page.title }}</title>
{% endcontent_for %}

{{ content }}

{% content_for footer %}
{% include disqus.html %}
{% endcontent_for %}
