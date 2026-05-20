# Week 1 知识框架总览

> 更新日期：2026-05-20

## 整体架构图

```
┌──────────────────────────────────────────────────────────────┐
│                    AI × Web3 知识框架                         │
│                                                              │
│  【AI 层】                        【Web3 层】                  │
│                                                              │
│  LLM（概率生成引擎）  ←→  智能合约（确定性执行引擎）            │
│  Context（工作内存） ←→  链上状态（全局持久化存储）             │
│  RAG（知识检索）     ←→  The Graph（链上数据索引）             │
│  Tool Calling       ←→  合约调用（链上工具执行）               │
│  Guardrails         ←→  访问控制（权限检查）                   │
│  MCP                ←→  RPC（外部系统连接协议）                │
│                                                              │
│  【桥接层：AI × Web3 交叉区】                                  │
│  Agent + Session Key → 有限授权的链上自动化                   │
│  Vibe Coding → AI 辅助合约开发（必须人工审查）                 │
│  人工确认节点 → 签名/转账/合约写入的强制暂停点                  │
└──────────────────────────────────────────────────────────────┘
```

## 笔记索引

### AI 基础（5.19 完成）

| 笔记 | 核心概念 | 难度 |
|------|---------|------|
| [week1-llm.md](week1-llm.md) | 自回归生成、幻觉、Token | ⭐⭐ |
| [week1-prompt.md](week1-prompt.md) | Few-shot、CoT、Prompt注入 | ⭐⭐ |
| [week1-context.md](week1-context.md) | 上下文窗口、四个控制层面 | ⭐⭐ |
| [week1-rag.md](week1-rag.md) | 向量检索、Embedding、Chunking | ⭐⭐⭐ |
| [week1-agent.md](week1-agent.md) | ReAct Loop、工具调用、Guardrails | ⭐⭐⭐ |
| [week1-frameworks.md](week1-frameworks.md) | LangGraph/OpenAI SDK/Hermes对比 | ⭐⭐⭐ |
| [week1-vibe-coding.md](week1-vibe-coding.md) | AI辅助编程、5类输出风险 | ⭐⭐ |
| [week1-mcp.md](week1-mcp.md) | MCP协议、三个原语、安全边界 | ⭐⭐⭐ |

### Web3 基础（5.20 补充）

| 笔记 | 核心概念 | 难度 |
|------|---------|------|
| [week1-web3-crypto-wallet.md](week1-web3-crypto-wallet.md) | 哈希、公私钥、签名、钱包 | ⭐⭐ |
| [week1-web3-contracts-network.md](week1-web3-contracts-network.md) | Solidity、Gas、L2、DeFi、AA | ⭐⭐⭐ |
| [week1-web3-indexing-security-devstack.md](week1-web3-indexing-security-devstack.md) | The Graph、4大漏洞、工具链 | ⭐⭐⭐ |

## 核心对照：AI vs Web3 概念

| AI 概念 | Web3 对应 | 共同点 |
|---------|----------|--------|
| Token（生成单位）| Token（资产单位）| 名字相同，完全不同 |
| API Key | 私钥 | 都是身份凭证，泄露都有损失 |
| 幻觉（Hallucination）| 合约 Bug | 都是系统产生的错误，必须验证 |
| RAG 检索 | The Graph 查询 | 都是给系统注入外部信息 |
| MCP 协议 | RPC 协议 | 都是"系统 ↔ 工具"的通信标准 |
| Guardrails | 访问控制 | 都是限制系统行为的边界机制 |
| Context Window | 链上状态 | 都是系统的"记忆"，但机制不同 |

## 关键安全原则（两个领域通用）

```
AI 侧安全                    Web3 侧安全
──────────────────────────────────────────
验证所有 AI 输出              验证所有交易内容
不信任模型给的链接            不信任 DApp 要求的授权
设置 Guardrails              设置权限控制
人工确认关键操作              人工确认所有签名
Tracing 记录所有行为         区块浏览器验证所有交易
```

## 学习进度追踪

```
5.18  [✓] 环境搭建（Hermes + GLM + GitHub）
5.19  [✓] AI 基础 8 章（8 份笔记）
5.20  [→] Web3 基础 + 实践任务
      [ ] 测试钱包创建
      [ ] Sepolia 测试网交互
      [ ] SimpleStorage 合约部署
      [ ] 最小交叉实验（模块 C）
Week2 [ ] 支付/身份/权限/安全/治理
```
