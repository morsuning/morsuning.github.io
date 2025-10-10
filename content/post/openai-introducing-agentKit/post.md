---
title: "【翻译】OpenAI - 推出 AgentKit"

date: 2025-10-09

author: morsuning

categories: ["Translation"]

tags: ["OpenAI", "R&D Efficiency"]
---
面向构建、部署与优化智能代理的新一整套工具

从今天起，我们推出 **AgentKit**，一套面向开发者和企业，用于构建、部署与优化智能代理（agents）的完整工具集。到目前为止，构建代理意味着你要处理零散的工具：复杂的编排（orchestration）没有版本控制、自定义连接器、手动评估流程、提示(prompt) 调优，以及上线前几周的前端工作。使用 AgentKit，开发者现在可以可视化设计工作流，并且使用新的构建模块更快地嵌入具“代理特性”的 UI，包括：

* **Agent Builder**：用于可视化创建和版本管理多 Agent 工作流的画布
* **Connector Registry**：集中管理 OpenAI 产品间的数据与工具连接方式
* **ChatKit**：一个用于在你的产品中嵌入可定制聊天型代理体验的工具包

我们还扩展了评估能力（Evals），新增功能如数据集、跟踪评分（trace grading）、自动提示优化、第三方模型支持，以便衡量与提升代理性能。

自从 3 月发布 Responses API 和 Agents SDK 以来，我们看到很多开发者和企业构建了端到端的 agent 工作流，用于深入研究、客服支持等。Klarna 构建了一个可处理三分之二工单的客服代理，Clay 用一个销售代理把增长提升了 10 倍。AgentKit 构建在 Responses API 之上，帮助开发者更高效、更可靠地构建代理。 ([OpenAI][1])

---

## 使用 Agent Builder 设计工作流

随着代理工作流日益复杂，开发者需要更清晰地了解其内部运作。
**Agent Builder** 提供一个可视化画布，用于拖拽节点组成逻辑、连接工具、配置定制 Guardrails（安全界限）。它支持预览运行、内联评估配置以及完整版本控制，非常适合快速迭代。 ([OpenAI][1])

开发者可以从空白画布开始，也可以使用预构建模板。
在 Ramp 团队，他们在几小时内即可从空白画布构建出一个买家代理：

> “Agent Builder 将原本需要数月、涉及复杂编排、自定义代码和手动优化的工作，转变为只需几小时即可完成。这个可视化画布让产品、法律和工程团队保持在同一页面，迭代周期缩短 70%，能够在两个冲刺内上线，而不是两个季度。”
> — Ramp ([OpenAI][1])

类似地，日本领先的技术及互联网服务公司 LY Corporation 使用 Agent Builder，在不到两小时内构建了一个工作助理代理：

> “Agent Builder 让我们以全新的方式编排代理，在一个界面里让工程师与领域专家协作。我们在不到两小时内构建了第一个多 agent 工作流并运行，大幅加速了代理的创建与部署时间。”
> — LY Corporation ([OpenAI][1])

我们还推出了 **Connector Registry**，供企业治理和维护跨多个工作区与组织中的数据。**Connector Registry** 将数据源整合到一个管理面板，可跨 ChatGPT 与 API 使用。这个注册表包含所有预构建连接器（如 Dropbox、Google Drive、SharePoint、Microsoft Teams）和第三方 MCP（多渠道平台）连接器。 ([OpenAI][1])

开发者还可以在 Agent Builder 中启用 **Guardrails**——一种开源、模块化的安全层，用以防止代理发生意外或恶意行为。Guardrails 可以掩码或标记个人识别信息（PII）、检测越狱（jailbreak）行为，并应用其他保护措施，更便于构建和部署可靠、安全的代理。Guardrails 可以独立部署，也可以通过 Guardrails 库在 **Python** 和 **JavaScript** 中使用。 ([OpenAI][1])

---

## 用 ChatKit 嵌入具代理特性的聊天体验

部署代理聊天 UI（用户界面）往往比预期更复杂 —— 你要处理流式响应、线程管理、显示模型“思考”状态、设计交互流畅的聊天体验。**ChatKit** 使得在你的产品中嵌入聊天型代理更简单。它可以被嵌入到应用或网站中，并可定制外观以匹配你的主题或品牌。 ([OpenAI][1])

> “我们用 ChatKit 为 Canva 开发者社区构建支持代理，节省了超过两周的开发时间，并在不到一小时内完成集成。这个支持代理将改变开发者与我们文档的交互方式，使其变为对话式体验，从而更容易在 Canva 上构建应用和集成。”
> — Canva ([OpenAI][1])

ChatKit 已经服务于多个用例，从内部知识助理、入门引导、客服支持到研究代理。HubSpot 的客服代理是其中一个例子。 ([OpenAI][1])

---

## 用新的 Evals 能力衡量代理性能

要构建可靠、可投入生产环境的代理，就必须进行严格的性能评估。去年我们推出 **Evals** 平台，帮助开发者测试 prompts 和衡量模型行为。如今我们为 Evals 添加了四个新功能，使得创建评估（evals）更容易：

1. **Datasets** — 从零快速构建 agent 评估，并通过自动评分器与人工批注扩展
2. **Trace grading** — 对 agent 工作流做端到端评估并自动打分，以找出缺陷
3. **自动提示优化**（Automated prompt optimization）— 基于人工批注和评分器输出生成更好的 prompts
4. **第三方模型支持** — 在 OpenAI Evals 平台内部评估其他厂商的模型 ([OpenAI][1])

我们已经看到客户通过使用这些功能获得了重大性能提升。

> “这个评估平台使我们多 agent 尽职调查框架的开发时间缩短超过 50%，同时使代理准确率提升了 30%。”
> — Carlyle ([OpenAI][1])

（在原文中有一个界面截图，展示 dataset 表格，一些评分、语气、反馈、准确度列） ([OpenAI][1])

---

## 用强化微调推动代理性能

**强化微调**（Reinforcement fine-tuning, RFT）使得开发者可以定制我们的推理模型。RFT 在 OpenAI o4-mini 上已广泛可用，并且在 GPT-5 上处于私有 beta 阶段。我们正与多家客户紧密合作，在更广泛发布前优化 GPT-5 的 RFT。 ([OpenAI][1])

今天，我们在该 RFT beta 中引入了两项新特性，以进一步提升代理性能：

* **定制工具调用**（Custom tool calls）— 让模型学会在正确的时机调用正确工具，以改善推理能力
* **定制评分器**（Custom graders）— 为你的用例设定专属评估标准，聚焦最关键的性能指标 ([OpenAI][1])

---

## 定价与可用性

从今天起，**ChatKit** 和新的 **Evals 能力** 向所有开发者广泛开放。Agent Builder 处于 Beta 阶段，而 Connector Registry 正在向部分 API、ChatGPT Enterprise 与教育用户推出 Beta。要启用 Connector Registry，需要 **Global Admin 控制台**（Global Admin Console）-- 只有具有全局所有者身份的用户才能管理域、单点登录 (SSO)、多个 API 组织。 ([OpenAI][1])

所有这些工具都包含在标准 API 模型定价中。我们计划很快推出独立的 Workflows API 和代理部署选项（用于 ChatGPT）。 ([OpenAI][1])

我们迫不及待地想看看你们会构建出什么。 ([OpenAI][1])

[1]: https://openai.com/index/introducing-agentkit/
