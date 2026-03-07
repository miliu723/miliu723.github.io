---
layout: post
title: LangChain学习与实践笔记
---

本文用于记录笔者在学习和实践LangChain时的一些笔记和经验。
使用的教程主要为吴恩达老师llm系列课程，以及DataWhale配套的notebook。

具体如下：

- [吴恩达老师原版课程](https://learn.deeplearning.ai/)
- [DataWhale llm-cookbook](https://github.com/datawhalechina/llm-cookbook?tab=readme-ov-file)
- [LangChain官方文档](https://python.langchain.com/en/latest/index.html)

关于llm-cookbook的使用说明：

1. llm-cookbook中使用的API是openai的API，建议改为使用deepseek或者千问等国产大模型的API，在学习过程中能避免许多网络导致的问题。
2. LangChain更新换代较快（短短两年已经面目全非），llm-cookbook中的许多代码年久失修，版本和包兼容不太行，建议在学习过程中多多查阅LangChain官方文档，以获取最新使用方法。
3. 关于Prompt，LangChain提供了一些基础的Prompt复用方法，切忌不动手，直接复制使用。

OK! Let's start!


