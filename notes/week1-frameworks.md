# AI Frameworks 开发框架

> 参考：https://aiweb3.school/zh/handbook/ai/frameworks/
> 学习日期：2026-05-19

## 为什么需要框架

直接调用 LLM API 能做很多事，但当系统变复杂时——多工具、多步骤、多模型、状态管理——手写这些逻辑既繁琐又容易出错。框架提供了：

- 工具调用的标准接口
- 状态管理和上下文传递
- 多 agent 协作的编排能力
- 可观测性（tracing、logging）
- 错误处理和重试机制

## 主流框架一览

### LangChain

最早期、最广泛使用的 LLM 框架。

**核心概念：**
- **Chain**：把多个 LLM 调用和工具串成一条流水线
- **LCEL（LangChain Expression Language）**：用 `|` 管道符组合组件
- **Agent**：内置多种 agent 类型（ReAct、OpenAI Functions 等）
- **Memory**：多种记忆类型（buffer、summary、vector）

```python
from langchain_anthropic import ChatAnthropic
from langchain_core.prompts import ChatPromptTemplate

chain = ChatPromptTemplate.from_template("解释 {concept}") | ChatAnthropic()
result = chain.invoke({"concept": "Gas"})
```

**优点：** 生态丰富，文档多，组件多  
**缺点：** 抽象层次高，调试困难，版本变化大

---

### LangGraph

LangChain 团队推出，专为**有状态、有循环**的 agent 设计。

**核心概念：**
- **Graph**：节点（Node）+ 边（Edge）构成的有向图
- **State**：图中所有节点共享读写的状态对象
- **Conditional Edge**：根据状态决定走哪条边

```python
from langgraph.graph import StateGraph

graph = StateGraph(MyState)
graph.add_node("plan", plan_node)
graph.add_node("execute", execute_node)
graph.add_edge("plan", "execute")
```

**适合：** 需要循环、条件分支、多 agent 协作的复杂 workflow  
**不适合：** 简单线性任务（过度设计）

---

### OpenAI Agents SDK

OpenAI 官方 agent 框架，设计简洁，围绕 `Agent`、`Tool`、`Handoff` 三个原语。

**核心概念：**
- **Agent**：一个模型 + 一组工具 + 系统指令
- **Handoff**：把任务转交给另一个 agent
- **Guardrails**：输入/输出验证
- **Tracing**：内置可观测性

```python
from agents import Agent, tool

@tool
def get_eth_balance(address: str) -> str:
    """查询地址的 ETH 余额"""
    ...

agent = Agent(name="web3-helper", tools=[get_eth_balance])
```

**适合：** 多 agent 协作，handoff 场景明确的系统

---

### Hermes Agent

去中心化、长期运行的 agent 框架，支持 MCP、Skills、多消息平台。

**核心概念：**
- **Skills**：可复用的高层指令集，可自动发现和动态调用
- **MCP 集成**：通过 MCP 连接外部工具
- **持久化**：跨 session 保存状态和记忆
- **多平台**：Telegram、微信、Discord 等消息平台接入

这也是本课程 Day 0 实际使用的框架（GLM-5.1 + Hermes + Telegram）。

---

### 其他值得了解的框架

| 框架 | 特点 | 适合场景 |
|------|------|---------|
| AutoGen（微软） | 多 agent 对话，角色扮演 | 复杂多角色协作 |
| CrewAI | 角色驱动，任务分工 | 模拟团队协作 |
| Dify | 低代码 agent 构建 | 快速原型，非开发者 |
| n8n | 可视化 workflow | 自动化流程，无代码 |

## 框架选择决策

```
简单问答 / 单次生成
    → 直接调用 API，不用框架

线性工作流，步骤固定
    → LangChain LCEL 或 简单脚本

有状态、有循环的复杂 agent
    → LangGraph

多 agent 协作，handoff 明确
    → OpenAI Agents SDK

长期运行，需要跨 session 记忆
    → Hermes Agent
```

## 框架 ≠ 解决一切

框架带来的抽象同时也带来：
- 调试难度增加（错误信息被包裹）
- 版本升级风险
- 不必要的依赖

**原则：** 先不用框架手写，理解每一层在做什么；再引入框架简化重复工作。

## 个人理解 / 未解决问题

- LangGraph 的"图"和普通函数调用链有什么实质区别？
- 框架的 tracing 数据如何与 Web3 的链上日志结合？
- Hermes 的 Skills 是否有社区共享的 Web3 相关 skill？
