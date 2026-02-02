---
layout: default
title: Frontier-CS blog posts
pagination:
  enabled: true
  collection: posts
  permalink: /page/:num/
  per_page: 50
  sort_field: date
  sort_reverse: true
  trail:
    before: 1
    after: 3
# Only show posts from this year on the blog index
show_year: 2026
---

<div class="post">

  <div class="header-bar">
    <h1>Frontier-CS blog posts</h1>
    <h2>Frontier-CS blog posts</h2>
  </div>

  {% if page.show_year %}
  {% assign year_str = page.show_year | append: '' %}
  {% assign posts_to_iterate = site.posts | sort: "date" | reverse %}
  {% else %}
  {% assign posts_to_iterate = paginator.posts %}
  {% endif %}
  <ul class="post-list">
    {% for post in posts_to_iterate %}
    {% assign post_year = post.date | date: "%Y" %}
    {% unless page.show_year %}{% assign show_post = true %}{% else %}{% if post_year == year_str %}{% assign show_post = true %}{% else %}{% assign show_post = false %}{% endif %}{% endunless %}
    {% if show_post %}
    {% if post.external_source == blank %}
      {% assign read_time = post.content | number_of_words | divided_by: 180 | plus: 1 %}
    {% else %}
      {% assign read_time = post.feed_content | strip_html | number_of_words | divided_by: 180 | plus: 1 %}
    {% endif %}
    {% assign year = post.date | date: "%Y" %}
    {% assign tags = post.tags | join: "" %}
    {% assign categories = post.categories | join: "" %}

    <li>
      <h3>
        {% if post.redirect == blank %}
          <a class="post-title" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
        {% else %}
          {% if post.redirect contains '://' %}
            <a class="post-title" href="{{ post.redirect }}" target="_blank">{{ post.title }}</a>
            <svg width="2rem" height="2rem" viewBox="0 0 40 40" xmlns="http://www.w3.org/2000/svg">
              <path d="M17 13.5v6H5v-12h6m3-3h6v6m0-6-9 9" class="icon_svg-stroke" stroke="#999" stroke-width="1.5" fill="none" fill-rule="evenodd" stroke-linecap="round" stroke-linejoin="round"></path>
            </svg>
          {% else %}
            <a class="post-title" href="{{ post.redirect | relative_url }}">{{ post.title }}</a>
          {% endif %}
        {% endif %}
      </h3>
      <p>{{ post.description }}</p>
      <p class="post-meta">
        {{ read_time }} min read &nbsp; &middot; &nbsp;
        {{ post.date | date: '%B %-d, %Y' }}
        {%- if post.external_source %}
        &nbsp; &middot; &nbsp; {{ post.external_source }}
        {%- endif %}
      </p>
      <p class="post-tags">
        <a href="{{ year | prepend: '/blog/' | prepend: site.baseurl}}">
          <i class="fas fa-calendar fa-sm"></i> {{ year }} </a>

          {% if tags != "" %}
          &nbsp; &middot; &nbsp;
            {% for tag in post.tags %}
            <a href="{{ tag | slugify | prepend: '/blog/tag/' | prepend: site.baseurl}}">
              <i class="fas fa-hashtag fa-sm"></i> {{ tag }}</a> &nbsp;
              {% endfor %}
          {% endif %}

          {% if categories != "" %}
          &nbsp; &middot; &nbsp;
            {% for category in post.categories %}
            <a href="{{ category | slugify | prepend: '/blog/category/' | prepend: site.baseurl}}">
              <i class="fas fa-tag fa-sm"></i> {{ category }}</a> &nbsp;
              {% endfor %}
          {% endif %}
    </p>
    </li>
    {% endif %}
    {% endfor %}
  </ul>

  {% unless page.show_year %}
  {% include pagination.html %}
  {% endunless %}

</div>
