---
title: "Agent = LLM + Harness：为什么上下文管理是智能体的灵魂？"
description: "探讨为什么 stateless 的大模型需要 Harness 才能成为 Agent，以及为什么上下文管理是决定智能体能力上限与工程成败的关键。"
pubDate: 'Jun 11 2026'
heroImage: '../../assets/agent-harness-context.png'
tags: ['workflows']
---

在人工智能的黄金时代，“智能体（Agent）”已经成为最核心的叙事。然而，当我们谈论 Agent 时，我们到底在谈论什么？

一个广泛认可的行业共识公式是：

$$\text{Agent} = \text{LLM} + \text{Harness}$$

在这个公式中，<strong>LLM（大语言模型）</strong> 是充当计算核心的“脑”，而 <strong>Harness（运行环境/安全带）</strong> 则是将这个脑与客观世界连接起来的“身躯与神经系统”。

许多人直觉地认为，LLM 的能力决定了 Agent 的上限，Harness 不过是套在模型外层的一层薄薄的 API 胶水代码。但真实的工程实践得出的结论恰恰相反：<strong>大模型的底层能力正在迅速商品化（Commoditization），而 Harness 的设计才是决定一个 Agent 在生产环境中是惊艳还是灾难的关键。</strong>

而在整个 Harness 的建设中，最核心、最复杂、也最能体现工程壁垒的工作，莫过于<strong>上下文管理（Context Management）</strong>。可以说，上下文管理就是智能体的灵魂。

---

## 一、 为什么 LLM 只是“缸中之脑”？

要理解 Harness 的重要性，我们首先需要认清一个事实：孤立的 LLM，在本质上只是一个<strong>“缸中之脑”（Brain in a Vat）</strong>。

著名哲学思想实验“缸中之脑”设想了一个场景：一个科学家将人的大脑切下来放进盛有营养液的缸中，用计算机向其发送电信号，使其维持产生和正常大脑一样的感官体验。对于这个大脑而言，世界只是电信号的输入与输出，它没有任何真实的感知器官（眼、耳、皮肤）来直接接触物理世界，也没有任何效应器官（手、脚、肌肉）来对世界施加物理影响。

孤立的大语言模型（LLM）面临着完全相同的困境：
1. <strong>无状态的计算引擎（Stateless Engine）</strong>：LLM 的核心是一个数学函数 $f(x) \to y$。在推理过程中，模型参数是冻结的，它不会记住上一轮对话的交互，也不会因用户的反馈而实时调整自身的神经网络权重。每一次调用，对模型而言都是一次“初次见面”。
2. <strong>缺乏感知与行动器官（No Senses and Effectors）</strong>：LLM 无法主动读取数据库、无法调用 API、无法运行代码、无法获取当前时间，甚至不知道自己输出的结果是否正确。它被困在基于 Token 概率预测的逻辑世界里，无法直接作用于客观现实。

而 <strong>Harness</strong>，就是为这颗大脑提供养分的“缸”，以及它与外部世界连接的“躯体”：
* <strong>感知（Perception）</strong>：Harness 负责接收用户的输入，拦截并解析外部系统的通知、工具执行的报错，并以 Structured Context（结构化上下文）的形式，作为新的电信号输入给 LLM。
* <strong>行动（Action）</strong>：当 LLM 输出符合特定格式的“意图”（如 JSON 形式的 Tool Call）时，Harness 会解释并执行该意图，如在真实环境中运行一段 Python 脚本，然后将执行结果（stdout/stderr）反馈给 LLM。
* <strong>生命周期（Lifecycle）与流转（Control Flow）</strong>：Harness 驱动着整个 Agent 的 Agentic Loop（感知-规划-行动-反思循环），决定了模型何时应当继续推理，何时应当停下来等待人类确认，何时应该判定任务结束。

因此，如果没有 Harness 的封装，LLM 只能停留在“静态生成文本”的阶段，永远无法演变为能在复杂环境中自主决策并解决真实问题的“智能体（Agent）”。

---

## 二、 上下文管理：智能体的“工作记忆”

在认知心理学中，人类的大脑拥有<strong>工作记忆（Working Memory）</strong>和<strong>长期记忆（Long-term Memory）</strong>。工作记忆容量极小（经典理论认为是 $7 \pm 2$ 个信息单元），但读写速度极快，用于处理当前手头的复杂任务。

对于 Agent 而言，LLM 的<strong>上下文窗口（Context Window）</strong>就是它的工作记忆。

虽然近年来模型厂商不断推高物理上下文窗口（从 4k、32k 到 1M 甚至 2M tokens），但在实际的 Agent 工程中，我们依然面临着无法逾越的“上下文瓶颈”：

### 1. 注意力退化与“迷失中段”（Lost in the Middle）
学术研究（如 Stanford 发表于 2023 年的经典论文）表明，随着上下文长度的增加，大模型对于位于输入段落中部的关键信息的检索和遵从能力呈指数级下降。给模型喂 100 万 token 的文档，它很容易漏掉隐藏在第 50 万行的关键指令。

### 2. 延迟（Latency）与首字时间（TTFT）的痛楚
LLM 的预填充（Prefill）阶段耗时与输入 Token 长度呈线性（甚至在某些架构中呈二次方）增长。如果一个 Agent 在每一步决策时都要带上 10 万 Token 的历史上下文，其首字返回时间（TTFT）可能会飙升至数十秒，这在交互式应用中是完全不可接受的。

### 3. 昂贵的 Token 成本
Agent 的运行通常是高频循环的。假设一个任务需要循环 50 次决策，如果每次请求都携带 5 万 Token 的上下文，那么单次任务就会消耗 250 万 Token。随着调用规模的扩大，账单金额将呈指数级上升。

因此，<strong>Harness 的核心任务就是：在有限的 Token 预算、时间预算和模型注意力带宽下，通过精准的上下文管理，实现最高的“信息信噪比”。</strong>

---

## 三、 上下文管理的核心工程挑战

在构建真实的 Agent（例如自动编写代码的 Coding Agent，或分析海量财务数据的 Finance Agent）时，Harness 会遭遇以下三大上下文泥潭：

### 1. <strong>信息熵的无序膨胀</strong>
在多轮交互中，用户的对话、系统的提示词、每一次工具调用的返回，都会像滚雪球一样不断累积。如果不加干涉，上下文会在 5-10 轮交互后迅速爆仓。

### 2. <strong>工具输出的“吞吐黑洞”</strong>
这是工程中最常见的崩溃点。当 Agent 执行一个 `git diff`、运行一段带有大量调试信息的 Python 脚本，或者抓取一个包含万行 HTML 的网页时，工具输出可能会产生几十万甚至上百万字符。如果将这些原始数据原封不动地塞进上下文，模型会瞬间被噪音淹没，甚至直接超出窗口上限。

### 3. <strong>任务目标的“记忆漂移”</strong>
随着执行步骤变深，模型在接收了大量的工具报错、中间尝试后，会逐渐“忘记”最初用户给定的全局目标（System prompt 和 User initial query），开始偏离轨道，甚至陷入死循环。

---

## 四、 上下文管理的技术路径与设计模式

为了解决上述挑战，优秀的 Agent Harness 会采用一系列精妙的上下文管理设计模式：

### 1. <strong>滚动滑动窗口与主动总结 (Sliding Window & Rolling Summarization)</strong>
这是最基础但必不可少的模式。Harness 不会保存完整的聊天历史，而是维护一个固定大小的“活动窗口”。
*   当对话超出限制时，Harness 会调用一次廉价的快速模型（或触发后台异步任务），将较早的历史记录进行<strong>结构化提炼（Summary）</strong>。
*   在新的请求中，上下文结构变为：`[全局系统提示] + [历史提炼摘要] + [最近 N 轮对话明细]`。这极大节省了空间，同时保留了远期上下文的关键线索。

### 2. <strong>两阶段检索增强 (Two-Stage RAG & Rerank)</strong>
对于长文档阅读或代码库修改任务，Harness 不会一次性读入所有文件。
*   <strong>第一阶段（Rough Retrieval）</strong>：通过向量检索（Dense Retrieval）或 BM25 检索（Sparse Retrieval）粗筛出相关片段。
*   <strong>第二阶段（Reranking）</strong>：使用重排模型（Reranker）对检索出的片段进行精细化评分，只将 Top-K 最相关的片段注入当前上下文。

### 3. <strong>局部上下文隔离与子智能体路由 (Context Isolation & Sub-agent Routing)</strong>
在处理复杂任务时，将所有任务塞进一个大 LLM 的上下文中是愚蠢的。<strong>分治法</strong>是软件工程的永恒真理。
*   当主智能体（Supervisor）发现需要执行某个子任务（如“编写并运行一段测试”）时，它不会自己在主上下文中折腾，而是通过 Harness <strong>派生出一个独立的子智能体（Sub-agent）</strong>。
*   子智能体在一个全新的、干净的上下文空间中运行，只包含该子任务所必须的上下文。
*   子智能体执行完毕后，主智能体只接收子智能体返回的<strong>精炼结论</strong>（例如：“测试通过” 或 “测试失败，报错为 X”）。这就像编程中的<strong>函数调用栈</strong>，局部变量在退出函数时被销毁，避免了主上下文的污染。

### 4. <strong>符号化状态机与结构化记忆 (Symbolic State Machine & Structured Memory)</strong>
有些状态不需要用自然语言保存在 LLM 上下文中。Harness 可以在外部维护一个结构化的数据库或键值对（Key-Value Store）。
*   例如，Agent 在处理多步骤的表单填写时，Harness 将已填写的数据以 JSON 形式保存在外部状态机中。
*   LLM 只需要知道当前的“状态指针”以及获取特定数据的 API，而不需要在每次 Prompt 中都背负着完整的 JSON 树。

### 5. <strong>动态流裁剪与过滤器 (Stdout Pruning & Custom Filters)</strong>
对于工具的庞大输出，Harness 必须充当“空气净化器”。
*   如果执行的 Shell 命令输出了 1000 行编译日志，Harness 会通过启发式算法进行裁剪：只保留前 50 行（看启动信息）和后 100 行（看报错堆栈），中间部分用 `[... 850 lines truncated ...]` 替代。
*   在读取代码文件时，Harness 可以利用 AST（抽象语法树）解析，只把函数签名和类结构提供给 LLM，当 LLM 明确发出请求时，才展开特定函数的具体实现（惰性加载）。

---

## 五、 学术界关于上下文机制的理论支撑

上下文管理不仅是工程直觉，在学术界也有着深厚的理论基础：

### 1. <strong>认知负荷理论（Cognitive Load Theory）</strong>
澳大利亚教育心理学家 John Sweller 提出的认知负荷理论认为，人类的工作记忆容量是有限的，如果同时输入过多的无关干扰（外在认知负荷），会严重阻碍大脑对核心问题的解决。在 Agent 系统中，未经过滤的冗余上下文就是“外在认知负荷”，会直接削弱大模型的推理性能。

### 2. <strong>记忆的联想检索（Associative Retrieval）与海马体机制</strong>
认知神经科学研究表明，人类的大脑并非随时加载所有的记忆。边缘系统中的海马体（Hippocampus）起到了索引的作用，当外界刺激（当前上下文）触发时，海马体才会激活大脑皮层（长时记忆）中相关的神经元区域。Harness 中的 <strong>Vector RAG 机制</strong>就是对海马体联想检索功能的仿生实现。

---

## 结语：Agent 的终局是 Harness 的竞争

当大语言模型的 API 接口变得像自来水和电力一样便宜且无处不在时，开发一个 Agent 的门槛将降为零。但是，开发一个<strong>能够稳定在复杂生产环境中解决真实问题</strong>的 Agent，其门槛正在变得越来越高。

这其中的决定性分水岭，就是 <strong>Harness 的精致程度</strong>。

谁能将上下文窗口的信噪比做到极致，谁能让模型在执行了 100 步之后依然清晰地记得最初的使命，谁能用最少的 Token 成本完成最长路径的推理——谁就拥有了最强大的智能体。

Agent 的竞争，底层是模型的算力竞争；但应用层，终局必然是 Harness 的上下文管理之战。

---

## 参考文献与学术源链接

- [1] Liu, N. F., Gardner, M., Belinkov, Y., Peters, M. E., & Smith, N. A. (2023). <strong>Lost in the Middle: How Language Models Use Long Contexts.</strong> *arXiv preprint arXiv:2307.03172*. [点击阅读 arXiv 论文原文](https://arxiv.org/abs/2307.03172)
- [2] Sweller, J. (1988). <strong>Cognitive Load During Problem Solving: Effects on Learning.</strong> *Cognitive Science*. [点击阅读 Wiley Online Library 论文原文](https://onlinelibrary.wiley.com/doi/abs/10.1207/s15516709cog1202_4)
- [3] Baddeley, A. (2000). <strong>The Episodic Buffer: A New Component of Working Memory?</strong> *Trends in Cognitive Sciences*. [点击阅读 Cell Press 论文原文](https://doi.org/10.1016/S1364-6613(00)01538-2)
- [4] Shinn, N., Labash, B., & Gopinath, A. (2023). <strong>Reflexion: Language Agents with Systematic Self-Reflection.</strong> *arXiv preprint arXiv:2303.11366*. [点击阅读 arXiv 论文原文](https://arxiv.org/abs/2303.11366)
