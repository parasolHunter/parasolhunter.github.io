---
layout:     post
title:      LangGraph 多步工作流编排：何时优于 LangChain Agent
subtitle:   State、Checkpoint、interrupt 与生产选型
date:       2026-09-04
order:      4
section:    ai-agent
author:     Parasol
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - AI
    - Agent
    - LangGraph
---

LangChain 的 `createAgent` 和 LangGraph 的图编排，是 Agent 工程里最常见的选型分歧。本文说明各自边界，以及 Checkpoint、人机协同在生产里的实际意义 — 这也是我在中台复杂编排与 SaaS 轻量助手之间做技术决策时的参考框架。

### 一、选型对照

| 维度 | LangChain Agent 循环 | LangGraph |
|------|----------------------|-----------|
| 抽象 | 隐式 ReAct 循环 | 显式 State / Node / Edge |
| 分支与并行 | 弱，靠 Prompt 碰运气 | 图结构原生支持 |
| 崩溃恢复 | 通常从头跑 | Checkpoint + `thread_id` |
| 人机协同 | 难插入暂停点 | `interrupt` 节点 |
| 适用 | 少量 Tool、单轮任务 | 多步、可恢复、需审核的工作流 |

我的实践原则：**中台复杂编排用图；三个 Tool 的 SaaS 助手用 `createAgent` 足够，避免过度设计。**

### 二、LangGraph 心智模型

```text
State（共享黑板）
  ├── messages[]
  ├── retrieved_docs[]
  ├── tool_results[]
  └── audit_status

Node: retrieve → plan → tool_execute → review → reply

Edge: 条件边（检索为空 → 拒答节点；审核驳回 → 重试或人工）
```

每个 Node 只做一件事，符合企业多 Agent **职责隔离** 思想 — LangGraph 用图把职责写进结构，而不是写进一篇超长 Prompt。

### 三、Checkpoint 为什么重要

长流程 Agent（理赔审核、多轮改单、代码评审流水线）最怕：

- Worker 重启丢上下文；
- 用户隔天继续会话；
- 审核员在中间步骤介入。

Checkpoint 把 State 持久化，配合 `thread_id` 隔离会话 — **崩溃后从上一 Node 恢复，而不是让用户重述需求**。

### 四、interrupt：审核门的图原生表达

```text
execute_node → interrupt(human_review) → 审核通过 → publish_node
                              ↘ 驳回 → revise_node
```

这比在 Prompt 里写「请等待人工」可靠 — **控制流在图里，不在模型输出里碰运气**。

### 五、最小三节点图

```text
retrieve(state)   # RAG 或空
tool_node(state)  # 必要时调业务 API
reply(state)      # 汇总 + 引用
```

设计时能说清每个节点读写 State 的哪些字段，比罗列 API 名更有工程价值。

### 六、与记忆的衔接

| 记忆类型 | 实现 | LangGraph 落点 |
|----------|------|----------------|
| 短记忆 | 最近 N 轮 messages | State.messages |
| 工作记忆 | 本轮 scratchpad / tool 结果 | State.tool_results |
| 长记忆 | 摘要或向量召回 | retrieve Node 或独立 memory Node |

对话产品可以先用当轮上下文；中台知识库类则把 **RAG 作为长记忆外挂**，用户级长期记忆按需演进。

### 七、学习路径参考

| 阶段 | 产出 |
|------|------|
| 入门 | 官方文档画 3 节点图 |
| 进阶 | 理解 Checkpoint / thread_id |
| 对照 | 记忆三层与现有项目缺口 |
| 扩展 | MCP 最小 Server 一个 tool |

官方文档： [LangGraph](https://langchain-ai.github.io/langgraph/) 、 [LangChain JS](https://docs.langchain.com/oss/javascript/langchain/overview)。

### 延伸阅读

- [RAG 全链路工程化](/2026/09/03/RAG全链路工程化/)
- [编码类 Agent 团队编排](/2026/09/06/编码类Agent团队编排与角色SOP/)
