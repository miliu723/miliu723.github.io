---
layout: post
title: LangChain学习与实践笔记
---

LangChain是什么？

LangChain是一个用于构建基于大模型API应用的框架，它提供了一些基础的组件，例如模型、提示词模板、内存、回调等，以及一些高级的组件，例如链、代理、工具等。

LangChain的目标是简化大模型API应用的开发过程，使开发者能够更专注于业务逻辑的实现，而不是底层的细节。



参考文献：

- [DataWhale llm-cookbook](https://github.com/datawhalechina/llm-cookbook?tab=readme-ov-file)
- [LangChain官方文档](https://python.langchain.com/en/latest/index.html)

关于llm-cookbook的个人使用建议：

1. llm-cookbook中使用的API是openai的API，建议改为使用deepseek或者千问等国产大模型的API，在学习过程中能避免许多网络导致的问题。
2. LangChain更新换代较快（短短两年已经面目全非），llm-cookbook中的许多代码年久失修，版本和包兼容不太行，建议多多查阅LangChain官方文档，以获取最新使用方法。
3. 关于Prompt，LangChain提供了一些基础的Prompt复用方法，切忌不动手，直接复制使用。