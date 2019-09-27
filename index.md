---
layout: default
title: Home
---

<body>
<div>
<a class="github-button" href="https://github.com/unfor19/unfor19.github.io" data-icon="octicon-star" data-size="large" data-show-count="true" aria-label="Star unfor19/unfor19.github.io on GitHub">Star</a>

<!-- Place this tag where you want the button to render. -->

<a class="github-button" href="https://github.com/unfor19" data-size="large" data-show-count="true" aria-label="Follow @unfor19 on GitHub">Follow @unfor19</a>

</div>
<h2>Posts</h2>
<ul>
  {% for post in site.posts %}
    <li>
      [{{ post.publication_date }}] - <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
<script async defer src="https://buttons.github.io/buttons.js"></script>
</body>
