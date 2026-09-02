---
layout:     post
title:      2026 Agent 面试核心：ReAct 与 Function Calling 怎么讲透
subtitle:   从 CoT 到工具循环，附失败处理与全栈落地要点
date:       2026-09-01
author:     Parasol
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - AI
    - Agent
    - 面试
---

2026 年国内大模型岗位面试里，Agent 方向几乎不再考「会不会写 Prompt」，而是考**会不会决策、会不会调工具、失败时系统会不会崩**。ReAct 与 Function Calling 是这两类能力在面试里最常出现的载体。本文把考点、手写骨架和工程落地串成一条线，方便面试前快速复盘。

### 一、CoT 和 ReAct 别混着说

| 模式 | 核心 | 能调工具吗 | 典型场景 |
|------|------|------------|----------|
| Chain-of-Thought (CoT) | 把推理步骤写出来 | 否 | 纯推理、数学题、分类 |
| ReAct | Thought → Action → Observation 循环 | 是 | 查库、调 API、改状态 |

面试口述可以这样说：**CoT 解决「想明白」，ReAct 解决「想明白之后还要动手，并根据结果继续想」**。

### 二、ReAct 循环手写骨架（白板友好）

```text
scratchpad = ""
for step in range(max_iterations):
    thought = LLM(goal, scratchpad)
    if thought.done:
        return thought.answer
    action, args = parse(thought)
    try:
        observation = execute_tool(action, args)
    except Exception as e:
        observation = f"TOOL_ERROR: {e}"   # 错误也是 Observation
    scratchpad += format(thought, action, observation)
return summarize_partial(scratchpad)       # 步数用尽时的降级
```

三个追问几乎必跟：

1. **Action 失败怎么办？** — 把错误字符串写回 scratchpad，让模型改参数或换工具，而不是让进程直接抛异常退出。
2. **步数到了任务没完成？** — 返回已完成部分的摘要 + 建议人工介入或缩小任务范围。
3. **怎么防无限循环？** — `max_iterations`、重复 Action 检测、相同 Observation 熔断。

### 三、Function Calling 在工程里长什么样

模型侧只产出结构化 `tool_calls`（名称 + JSON 参数），**执行器在服务端**跑真实函数，再把 `ToolMessage` 塞回对话。面试要强调 Schema 比函数名更重要：

- 什么时候该调用这个工具？
- 每个参数的业务含义和合法范围？
- 失败时返回什么（错误码、可读说明）？

```typescript
// 伪代码：服务端执行边界
const allowed = new Set(['fetch_playlist', 'check_quota', 'get_plans']);
if (!allowed.has(call.name)) throw new ForbiddenTool(call.name);
const args = validateSchema(call.name, call.arguments); // class-validator / zod
const result = await registry.execute(call.name, args, { userId, ip });
return { role: 'tool', content: JSON.stringify(result) };
```

**安全底线**：工具白名单、参数校验、密钥与 OAuth Token 永远不进入 Prompt；模型只调「已校验过的业务 API」，不直连第三方。

### 四、和全栈产品的对应关系

独立项目里若用 LangChain `createAgent` + `@tool`，面试可以这样对标：

- 用户说「帮我解析这个 Spotify 链接」→ 模型产出 `fetchByUrl` 的 tool_call。
- 服务端 tool 走已登录用户的配额与 Provider 注册表。
- 解析失败 → Observation 里是业务错误，前端 toast，**不让模型编造曲目列表**。

这比空谈 ReAct 更有说服力：你讲的是**权限、配额、失败 UX** 一体的 Agent，不是聊天 Demo。

### 五、面试 60 秒收束句

> ReAct 是带工具反馈的推理循环；Function Calling 是工程上把「动手」标准化。生产里关键不是循环写得多漂亮，而是执行器可控、失败可恢复、工具最小权限，以及步数与成本的硬上限。

### 延伸阅读

- [MCP 与 Agent 工具协议（2026）](/2026/09/02/MCP与Agent工具协议2026/)
- [企业级多智能体协同架构](/2026/09/05/企业级多智能体协同架构设计/)
