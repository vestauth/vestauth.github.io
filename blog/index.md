---
title: "Blog"
---

<style>
  .blog-index {
    max-width: 980px;
    margin: 0 auto;
    padding: 2rem 1.25rem 3rem;
  }

  .blog-index h1 {
    font-size: clamp(2rem, 4vw, 2.6rem);
    line-height: 1.1;
    letter-spacing: -0.02em;
    margin: 0 0 1.5rem;
  }

  .blog-grid {
    list-style: none;
    margin: 0;
    padding: 0;
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 1rem;
  }

  .blog-card {
    display: block;
    text-decoration: none;
    color: inherit;
    background: #ffffff;
    border: 1px solid #e4e4e7;
    border-radius: 14px;
    overflow: hidden;
    transition: transform 120ms ease, box-shadow 120ms ease;
    height: 100%;
  }

  .blog-card:hover {
    transform: translateY(-2px);
    box-shadow: 0 14px 30px rgba(0, 0, 0, 0.08);
    text-decoration: none;
  }

  .blog-card img {
    width: 100%;
    aspect-ratio: 16 / 9;
    object-fit: cover;
    display: block;
    background: #f4f4f5;
  }

  .blog-card-body {
    padding: 0.9rem 1rem 1rem;
  }

  .blog-card time {
    color: #52525b;
    font-size: 0.82rem;
    display: block;
    margin-bottom: 0.35rem;
  }

  .blog-card h2 {
    margin: 0;
    line-height: 1.3;
    font-size: 1.07rem;
    letter-spacing: -0.01em;
  }

  @media (prefers-color-scheme: dark) {
    .blog-card {
      background: #0b2319;
      border-color: #1f4436;
    }

    .blog-card time {
      color: #a1a1aa;
    }

    .blog-card:hover {
      box-shadow: 0 14px 30px rgba(0, 0, 0, 0.22);
    }
  }
</style>

<section class="blog-index">
  <h1>Blog</h1>
  <ul class="blog-grid">
    {% for post in site.categories.blog %}
      <li>
        <a href="{{ post.url }}" class="blog-card">
          {% if post.image %}
            <img alt="{{ post.title }}" loading="lazy" width="800" height="450" decoding="async" src="{{ post.image }}">
          {% endif %}
          <div class="blog-card-body">
            <time datetime="{{ post.date | date: "%Y-%m-%d" }}">{{ post.date | date: "%B %d, %Y" }}</time>
            <h2>{{ post.title }}</h2>
          </div>
        </a>
      </li>
    {% endfor %}
  </ul>
</section>
