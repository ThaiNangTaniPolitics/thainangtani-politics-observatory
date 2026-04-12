---
layout: default
title: Image Gallery
---

<p><a href="/thainangtani-politics-observatory/">← Back to Home</a></p>

# Image Gallery

<style>
.gallery {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
  gap: 20px;
}
.gallery .img-container {
  width: 100%;
  aspect-ratio: 1 / 1;
  overflow: hidden;
  border-radius: 6px;
  background: #eee;
}
.gallery img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  cursor: zoom-in;
  transition: transform 0.3s ease;
}
.gallery img:hover {
  transform: scale(1.05);
}
</style>

<div class="gallery">
{% assign all_files = site.static_files | where_exp: "item", "item.path contains 'images/'" %}
{% for file in all_files %}
  {% assign ext = file.extname | downcase %}
  
  {% comment %} 
    Wir filtern Dubletten: 
    Wenn wir ein PNG/JPG finden, prüfen wir, ob ein WebP mit gleichem Namen existiert.
    Wenn ja, überspringen wir dieses File, da das WebP separat geladen wird.
  {% endcomment %}
  
  {% assign skip = false %}
  {% if ext == '.png' or ext == '.jpg' or ext == '.jpeg' %}
    {% assign base = file.path | remove: file.extname %}
    {% for check in all_files %}
      {% if check.path == "{{ base }}.webp" %}
        {% assign skip = true %}
      {% endif %}
    {% endfor %}
  {% endif %}

  {% unless skip %}
    {% if ext == '.webp' or ext == '.png' or ext == '.jpg' or ext == '.jpeg' %}
      <div class="img-container">
        <img src="{{ site.baseurl }}{{ file.path }}" alt="{{ file.basename }}">
      </div>
    {% endif %}
  {% endunless %}
{% endfor %}
</div>
