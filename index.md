---
layout: master
---
<style>
  /* 2. Grid Layout for Posts */
  .post-grid { 
    display: grid; 
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); 
    gap: 20px; 
  }

  /* 3. The "Box" Styling */
  .post-box { 
    border: 1px solid #eaecef; 
    border-radius: 8px; 
    padding: 20px; 
    box-shadow: 0 4px 6px rgba(0,0,0,0.05); 
    transition: transform 0.2s; 
    background: #ffffff;
  }
  .post-box:hover { 
    transform: translateY(-5px); /* Creates a slight "lift" effect when hovered */
    box-shadow: 0 6px 12px rgba(0,0,0,0.1);
  }
  .post-title { 
    font-size: 1.2em; 
    margin-bottom: 10px; 
    font-weight: bold; 
  }
  .post-date { 
    font-size: 0.9em; 
    color: #666; 
  }
</style>

## Recent Posts

<div class="post-grid">
  {% for post in site.posts %}
    <div class="post-box">
      <div class="post-title"><a href="{{ post.url }}">{{ post.title }}</a></div>
      <div class="post-date">{{ post.date | date: "%B %d, %Y" }}</div>
      
      <div style="font-size: 0.8em; margin-top: 10px; color: #007bff;">
        {{ post.categories | join: ", " | capitalize }}
      </div>
    </div>
  {% endfor %}
</div>
## About

[Click here to read about me](about.md)

## Recent Posts

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a> - {{ post.date | date: "%B %d, %Y" }}
    </li>
  {% endfor %}
</ul>
