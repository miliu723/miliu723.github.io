---
layout: page
title: 项目
permalink: /projects/
---

<div class="projects-list">
  <div class="project-card">
    <h2><a href="{{ site.baseurl }}/projects/academic-ai-assistant/">学术论文AI搜索助手</a></h2>
    <p>一个基于人工智能技术的学术论文搜索和分析工具，帮助研究人员更高效地查找、分析和理解学术论文。</p>
    <div class="project-meta">
      <span class="status">开发中</span>
      <span class="tech">Python, React, AI, NLP</span>
    </div>
  </div>
  <div class="project-card">
    <h2><a href="{{ site.baseurl }}/projects/ai-weekend/">AI Weekend</a></h2>
    <p>一款面向城市年轻群体的AI驱动型周末出行决策工具，快速生成个性化周末行程方案，支持动态调整。</p>
    <div class="project-meta">
      <span class="status">开发中</span>
      <span class="tech">Python, React, AI, NLP</span>
    </div>
  </div>
</div>

<style>
.projects-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
  margin-top: 20px;
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

.project-card h2 {
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
</style>
