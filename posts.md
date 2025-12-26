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
            {% if post.image %}
              <img src="{{ post.image | prepend: site.baseurl }}" alt="{{ post.title }}" class="post-thumb" />
            {% endif %}
            <div class="post-content">
              <h3><a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a></h3>
              <span class="post-date">{{ post.date | date: "%b %-d, %Y" }}</span>
              {% if post.tagline %}
                <p class="post-tagline">{{ post.tagline }}</p>
              {% endif %}
            </div>
          </li>
        {% endfor %}
      </ul>
    {% else %}
      <p>No posts found.</p>
    {% endif %}
  </div>
</section>

<style>
#posts .inner {
  text-align: center;
}

/* list container centered, items constrained */
.post-list {
  list-style: none;
  padding: 0;
  margin: 0 auto;
  max-width: 760px;
}

/* thumbnail + content side-by-side */
.post-list li {
  margin-bottom: 1.5em;
  display: flex;
  align-items: center;
  text-align: left;
  padding: 0.4em;
  border-radius: 6px;
}

/* small thumbnail on the left */
.post-thumb {
  width: 80px;
  height: 80px;
  aspect-ratio: 1/1;
  object-fit: cover;
  flex: 0 0 80px;
  border-radius: 6px;
  margin-right: 1rem;
  display: block;
}

/* content block */
.post-content h3 {
  margin: 0;
  font-size: 1.05rem;
}

.post-date {
  color: #888;
  font-size: 0.9em;
  display: block;
  margin-top: 0.25em;
}

.post-tagline {
  margin-top: 0.4em;
  font-style: italic;
  color: #787878ff;
}

/* stack on small screens */
@media (max-width: 640px) {
  .post-list li {
    flex-direction: column;
    align-items: center;
    text-align: center;
  }

  .post-thumb {
    margin-right: 0;
    margin-bottom: 0.6em;
    width: 100%;
    height: auto;
    max-height: 240px;
  }

  .post-content {
    width: 100%;
  }
}
</style>
