---
layout: default
title: All Posts
menu: true
permalink: /posts/
---

<section id="posts">
  <div class="inner">
    <h1>{{ page.title }}</h1>
    
    {% if site.posts.size > 0 %}
      <ul class="post-list">
        {% for post in site.posts %}
          <li>
            <h3><a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a></h3>
            <span class="post-date">{{ post.date | date: "%b %-d, %Y" }}</span>
            {% if post.tagline %}
              <p class="post-tagline">{{ post.tagline }}</p>
            {% endif %}
          </li>
        {% endfor %}
      </ul>
    {% else %}
      <p>No posts found.</p>
    {% endif %}
  </div>
</section>

<style>
.post-list {
  list-style: none;
  padding: 0;
}

.post-list li {
  margin-bottom: 2em;
}

.post-list h3 {
  margin-bottom: 0.2em;
}

.post-date {
  color: #888;
  font-size: 0.9em;
}

.post-tagline {
  margin-top: 0.3em;
  font-style: italic;
}
</style>
