---
layout: post
title:  "Jekyll - Add Commentbox.io"
date:   2019-01-17 07:04:16 -0300
tags: jekyll commentbox disqus comments computers 
---
[Commentbox](https://commentbox.io) is a slim alternative to [Disqus](https://disqus.com). Lets add it to Jekyll.

Edit "\_config.yml" and add the following lines, where "id" comes from your Commentbox dashboard:
```
commentbox:
 id: XXXXXXXXXXXXX-proj
```

Create "\_includes/commentbox.html" as follows:
```
{% raw %}
{%- if page.comments != false  -%}
  <div class="commentbox"></div>
  <script src="https://unpkg.com/commentbox.io/dist/commentBox.min.js"></script>
  <script>
    commentBox('{{ site.commentbox.id }}');
  </script>
  <noscript>Please enable JavaScript to view the comments.</noscript>
{%- endif -%}
{% endraw %}
```

In your post layout, "\_layouts/post.html", include "commentbox.html" after the content:
```
{% raw %}
  <div class="post-content e-content" itemprop="articleBody">
    {{ content }}
  </div>

  {%- if site.commentbox.id -%}
    {%- include commentbox.html -%}
  {%- endif -%}"
{% endraw %}
```

That's it!

### References
* <https://commentbox.io/docs> 

