---
layout:     post
title:      LangGraph 多步工作流编排：何时优于 LangChain Agent
subtitle:   State、Checkpoint、interrupt 与面试加分表述
date:       2026-09-04
author:     Parasol
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - AI
    - Agent
    - LangGraph
---

LangChain 的 `createAgent` 和 LangGraph 的图编排，是 2026 面试里区分「用过 Demo」和「能设计系统」的高频对比题。本文说明各自边界，以及 Checkpoint、人机协同在生产里的意义。

### 一、一张表讲清选型

| 维度 | LangChain Agent 循环 | LangGraph |
|------|----------------------|-----------|
| 抽象 | 隐式 ReAct 循环 | 显式 State / Node / Edge |
| 分支与并行 | 弱，靠 Prompt 碰运气 | 图结构原生支持 |
| 崩溃恢复 | 通常从头跑 | Checkpoint + `thread_id` |
| 人机协同 | 难插入暂停点 | `interrupt` 节点 |
| 适用 | 少量 Tool、单轮任务 | 多步、可恢复、需审核的工作流 |

**面试加分句**：「中台复杂编排用图；三个 Tool 的 SaaS 助手用 `createAgent` 足够，避免过度设计。」

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

每个 Node 只做一件事，符合企业多 Agent **职责隔离** 思想 — 只是 LangGraph 用图把职责写进结构，而不是写进一篇超长 Prompt。

### 三、Checkpoint 为什么重要

长流程 Agent（理赔审核、多轮改单、代码评审流水线）最怕：

- Worker 重启丢上下文；
- 用户隔天继续会话；
- 审核员在中间步骤介入。

Checkpoint 把 State 持久化，配合 `thread_id` 隔离会话。口述：**「崩溃后从上一 Node 恢复，而不是让用户重述需求。」**

### 四、interrupt：审核门的图原生表达

```text
execute_node → interrupt(human_review) → 审核通过 → publish_node
                              ↘ 驳回 → revise_node
```

这比在 Prompt 里写「请等待人工」可靠 — **控制流在图里，不在模型良心上**。

### 五、最小三节点图（面试白板）

```text
retrieve(state)   # RAG 或空
tool_node(state)  # 必要时调业务 API
reply(state)      # 汇总 + 引用
```

能说清每个节点读写 State 的哪些字段，比背 API 名更有说服力。

### 六、与记忆的衔接

| 记忆类型 | 实现 | LangGraph 落点 |
|----------|------|----------------|
| 短记忆 | 最近 N 轮 messages | State.messages |
| 工作记忆 | 本轮 scratchpad / tool 结果 | State.tool_results |
| 长记忆 | 摘要或向量召回 | retrieve Node 或独立 memory Node |

对话产品若只有当轮上下文，面试诚实说 **长期记忆在补**；中台知识库类则是 **RAG 作为长记忆外挂**。

### 七、两周上手路径（自测用）

| 天 | 产出 |
|----|------|
| 1–2 | 官方文档画 3 节点图 |
| 3 | 讲清 Checkpoint / thread_id |
| 4 | 对比记忆三层与现有项目缺什么 |
| 5 | MCP 最小 Server 一个 tool |

官方文档优先： [LangGraph](https://langchain-ai.github.io/langgraph/) 、 [LangChain JS](https://docs.langchain.com/oss/javascript/langchain/overview)。

### 延伸阅读

- [RAG 全链路工程化](/2026/09/03/RAG全链路工程化/)
- [编码类 Agent 团队编排](/2026/09/06/编码类Agent团队编排与角色SOP/)
