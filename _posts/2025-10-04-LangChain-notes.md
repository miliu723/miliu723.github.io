---
layout: post
title: LangChain学习与实践笔记
---

**LangChain是什么？**

简单来说就是一个使用大模型API进行应用开发的框架，把大模型API的调用封装成一些组件，例如模型、提示词模板、内存、回调等，以及一些高级的组件，例如链、代理、工具等。

**为什么会出现LangChain这样的工具？**

因为有需求。LangChain在2022年发布，早期并没有引起特别大的反响，直到ChatGPT系列以及其他llm的出现，大模型应用爆发式增长，大家忽然发现，工作流变得越来越复杂，需要一个框架来管理流程，让开发者专注开发，省去了很多撰写中间代码的精力。

说到中间代码，隐含的一个信息是，没有LangChain或者类似的框架，开发者也能干活，只是更多的精力会用在造轮子上。

**LLM应用开发者面对的主要痛点是什么？**

1. 有多个LLM和API需要调用时，**手动集成非常繁琐**。例如，在学术论文AI搜索这个项目中（我的个人项目，可前往主页查看），用于论文搜索的是Google的serpAPI，而用于AI总结的是deepseek-V3。对于更复杂的应用，生成、摘要、问答等流程需要用到多个API，每个 LLM 的接口和数据处理方式各不相同，一个个了解所有API的处理方式将会非常耗时；
2. 多步骤处理流程不能自动化。例如，对于一个简单的查询请求，分析请求→数据库查询→文本生成→回答生成，需要构建这样一个多步骤的处理流程，对于开发者来说通常是非常复杂的。
3. 存储和管理记忆困难。开发多轮对话和上下文记忆时，如何存储、检索和管理记忆信息是一个难题，尤其是在RAG系统中，高效的索引和查询机制非常重要。
4. 其他。。。

以上痛点，重要性是依次降低。那么**针对这些痛点，LangChain是怎么解决的？**

1. 统一的接口和模块化设计。LangChain 提供了一个统一的框架，允许开发者在不同的 LLM 之间进行切换。开发者只需用一个统一的接口来访问各种不同的语言模型、数据库、API 等，这样便于快速集成和替换。
2. 多步骤链式任务（Chains）。LangChain 支持通过 **“Chains”** 组合多个任务。每个任务可以调用不同的 LLM 或外部服务，输入一个模型的输出作为另一个模型的输入，从而完成复杂的多步骤任务。
3. 对话上下文管理。它帮助开发者管理多轮对话中的信息流转（例如：保持对话状态、记忆和用户上下文）。
4. 其他，例如LangChain对RAG提供了良好的支持，支持与许多外部工具的结合（例如搜索引擎、数据库等）。

这里笔者主要介绍几种LangChain的重要机制。分别是：

- Chains（最重要，LangChain的核心）
- Prompt template
- Memory
- Agent

笔者会尽量避开代码和晦涩的术语，用大白话阐述，让我们依次开始！

### Chains

Chains，简而言之，就是LangChain把复杂的任务分成一个个小任务，小任务之间彼此的输入输出相连，得到一个最终结果。链和链组合起来，就能完成更复杂的任务。

举个栗子，现在你是一个电商客服，正在对接一位法国客户，你需要准确用一清二白的法语基础回答客户的问题，这时候Chains的逻辑是什么呢？

- Chain1:将客户输入的法语Query翻译成中文
- Chain2:对于Chain1输出的中文，用一句话总结
- Chain3:对于Chain2输出的一句话总结，用中文写一段态度友好的回复
- Chain4:将Chain3输出的中文回复翻译成法语

于是你可以看到，Chains之间有输入输出的关联，每个输入是上一个Chain的输出，最终返回给客户的答案是Chain4的回答，这样的链叫做“顺序链”。

那除了顺序链，LangChain还有其他的链吗？

当然有，LangChain还提供了非常有趣的“路由链”，顾名思义就是，有一个类似于路由器的链，由它来决定将任务交给哪个子链来完成。当有多个子链，每个子链负责特定类型的输入，我们可以考虑用路由链。下面也举个例子：

在通用知识搜索中，当用户提出一个问题，这个问题可能属于不同的领域，包括计算机、数学、历史、物理等等，但是我们不能期望模型达到AGI（通用人工智能）的水准，最好的办法是，将问题交给一个个“领域专家”，数学问题给数学家，历史问题给历史学者，诸如此类。这时候的Chains是怎么设计的呢？

- Chain1:物理教授。prompt：“You are a very smart physics professor. You are great at answering questions about physics in a concise and easy to understand manner.“
- Chain2:数学家。prompt类似
- Chain3:历史学家。
- Chain4:计算机科学家。
- Route_Chain：首先判断用户问题属于什么学科，然后将问题交给对应的领域专家（对应子链）

在这个设计中有两点值得注意：

1. 如果用户询问的问题并不属于物理数学历史计算机中的任何一类，该怎么办？所以应该设计一条“Default_Chain“，当问题不属于已知领域时，路由链会把它交给这个默认链。默认链也就是”什么特别的背景都没有，什么都懂一些的大学者“，相当于直接调用了API进行询问。
2. 即使是“领域专家”，也可能遇到回答不了的困难。如果不增加合理的约束，专家们为了尽可能给出答案，可能会根据已有知识胡说八道。所以最好在prompt中增加“承认无知”约束，也就是告诉大模型：“如果你不知道，请直接回答不知道，不要编造答案。”

### Prompt template

提示词模版是什么？能干什么？

简单来说，就是在提示词里挖坑，用户给出的关键信息会被填充到坑里，形成完整的提示词。不变的那部分逻辑就是提示词模版。

提示词模版最大的好处就是可以复用prompt，对于需要对不同用户信息执行多次相同逻辑的任务，用Prompt template可以节省很多写提示词的时间。

LangChain提供了专门的Prompt template接口，可以快速方便地构造prompt。

具体来看一个例子。

你是一个淘宝五星商家，每天都会收到成百上千的complaint和reviews，这时候一条一条回复变得exhausting，prompt template就派上用场了。

LangChain的提示模版prompt_template中，我们只需要输入两个变量：style和text，其中style表示回复的语言风格，text表示用户的原始文本。

在淘宝商家这个例子里，style可以是“正式、尊敬的语调“，text是客户的评论，然后调用大模型进行回答，就可以得到对该客户评论的一个“正式、尊敬的”回复。对于每一个回复评论的任务，只需要修改text变量就可以得到符合同样要求的答案，是不是很方便？

### Memory

为什么需要存储记忆？当然需要，不存储上下文，大模型怎么连续对话。

LangChain提供了很多种记忆模块，这里我们主要介绍四种：

- ConversationBufferMemory
- ConversationBufferWindowMemory
- ConversationTokenBufferMemory
- ConversationSummaryBufferMemory

简单来说，第一种ConversationBufferMemory是最普通的，用户聊什么我存什么，就算是128K tokens的上下文，我也给存下来。这样很明显是鲁莽的：一方面，随着token数的增加，成本会急剧增加，另一方面，就算是128K tokens，事实上其中的有效信息也很少，反而会降低推理的正确率。

于是出现了第二种ConversationBufferWindowMemory，这里的“Window”顾名思义，就是给了一个固定大小的窗口，比如说5，只存最近的5条message。这样的窗口大小可以在创建缓存区时通过参数k来设置。

第三种ConversationTokenBufferMemory，和第二种很类似，区别在于，第二种只存最近k轮对话信息，而第三种是存储最近max_token_limit个tokens的对话信息，其中，max_token_limit是自己设置的参数。

看到这里，你可能会有疑问，不管是设置窗口大小还是设置max_token_limit，不都是设定了一个阈值？再往前就完全失忆了。没错，所以出现了第四种记忆模块ConversationSummaryBufferMemory，综合了以上三种的优点，既不会完全失去远古记忆，又可以保持较小的缓存区体量：只存储一段简洁的对话总结，每发生一次新对话，就根据已有的总结和新对话内容，更新缓存记忆。既经济又实用的做法！

### Agent

Agent是什么？简单来说，就是一个通过LLM调用各种工具来帮助我们完成任务的小助理。

那么有哪些工具可以调用呢？

这里主要介绍一下以下三种方法：

- LangChain内置tools。就是LangChain封装好的，可以直接拿来用的工具。例如：
    - llm-math：就是计算器，计算任务都可以交给它
    - wikipedia：可以理解为是LangChain内部接了一个wikipedia API，帮助完成搜素查询的
- PythonREPLTool，即，进行python交互，通过执行python代码来解决LLM中需要计算或者数据处理的任务。
- 自定义Tool。在实际项目中，我们往往需要调用公司内部的 API 或执行特定的业务逻辑。这时，我们可以将任何我们想要的函数封装成 Agent 能够理解和调用的工具，再加上tool函数装饰器，就可以自由地调用这个tool了。


参考文献：

- [DataWhale llm-cookbook](https://github.com/datawhalechina/llm-cookbook?tab=readme-ov-file)
- [LangChain官方文档](https://python.langchain.com/en/latest/index.html)

关于llm-cookbook的个人使用建议：

1. llm-cookbook中使用的API是openai的API，建议改为使用deepseek或者千问等国产大模型的API，在学习过程中能避免许多网络导致的问题。
2. LangChain更新换代较快（短短两年已经面目全非），llm-cookbook中的许多代码年久失修，版本和包兼容不太行，建议多多查阅LangChain官方文档，以获取最新使用方法。
3. 关于Prompt，LangChain提供了一些基础的Prompt复用方法，切忌不动手，直接复制使用。