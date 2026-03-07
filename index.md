---
layout: default
---

<h2>项目</h2>

<div class="projects-list">
  <div class="project-card">
    <h3><a href="{{ site.baseurl }}/projects/meeting-agent/">会议记录 AI Agent</a></h3>
    <p>一个自研的会议记录 AI Agent，能够自动识别会议内容，提取关键信息，并生成结构化的会议纪要。</p>
    <div class="project-meta">
      <span class="status">开发中</span>
      <span class="tech">Python, OpenAI API, NLP</span>
    </div>
  </div>
</div>

<h2>博客</h2>

<div class="posts">
  {% for post in site.posts %}
    <article class="post">

      <h3><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></h3>

      <div class="entry">
        {{ post.excerpt }}
      </div>

      <a href="{{ site.baseurl }}{{ post.url }}" class="read-more">Read More</a>
    </article>
  {% endfor %}
</div>

<style>
.projects-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
  margin-top: 20px;
  margin-bottom: 40px;
}

.project-card {
  border: 1px solid #eaeaea;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  transition: box-shadow 0.3s ease;
}

.project-card:hover {
  box-shadow: 0 4px 8px rgba(0,0,0,0.15);
}

.project-card h3 {
  margin-top: 0;
  margin-bottom: 10px;
}

.project-card a {
  color: #333;
  text-decoration: none;
}

.project-card a:hover {
  color: #0066cc;
}

.project-card p {
  margin-bottom: 15px;
  color: #666;
}

.project-meta {
  display: flex;
  justify-content: space-between;
  font-size: 14px;
  color: #888;
}

.status {
  background-color: #f0f0f0;
  padding: 2px 8px;
  border-radius: 12px;
}

.tech {
  font-style: italic;
}

.posts {
  margin-top: 20px;
}

.post {
  margin-bottom: 30px;
  padding-bottom: 20px;
  border-bottom: 1px solid #eaeaea;
}

.post h3 {
  margin-top: 0;
  margin-bottom: 10px;
}

.post a {
  color: #333;
  text-decoration: none;
}

.post a:hover {
  color: #0066cc;
}

.entry {
  margin-bottom: 15px;
  color: #666;
}

.read-more {
  display: inline-block;
  padding: 5px 15px;
  background-color: #f0f0f0;
  border-radius: 5px;
  color: #333;
  text-decoration: none;
  transition: all 0.3s ease;
}

.read-more:hover {
  background-color: #e0e0e0;
  color: #0066cc;
}

h2 {
  margin-top: 40px;
  margin-bottom: 20px;
  color: #333;
  border-bottom: 1px solid #eaeaea;
  padding-bottom: 10px;
}
</style>