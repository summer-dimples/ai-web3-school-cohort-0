# MCP Model Context Protocol

> 参考：https://aiweb3.school/zh/handbook/ai/mcp/
> 学习日期：2026-05-19

## MCP 是什么

MCP（Model Context Protocol）是 Anthropic 于 2024 年发布的开放标准，解决一个核心问题：

**如何让 AI 模型以统一的方式连接各种外部工具和数据源？**

在 MCP 之前：每个应用、每个工具都需要写一套自定义集成代码，重复且不可复用。

MCP 之后：工具开发者写一次 MCP Server，任何支持 MCP 的 AI 应用都能使用。

```
┌─────────────────────────────────────────────────────┐
│  MCP 架构                                             │
│                                                      │
│  ┌──────────────┐     MCP 协议     ┌──────────────┐  │
│  │  MCP Host    │ ←─────────────→ │  MCP Server  │  │
│  │  (Claude /   │                 │              │  │
│  │   IDE / App) │                 │  工具/数据   │  │
│  └──────────────┘                 └──────┬───────┘  │
│                                          │           │
│                              ┌───────────┼─────────┐ │
│                              ↓           ↓         ↓ │
│                           文件系统     数据库    Web API │
└─────────────────────────────────────────────────────┘
```

## MCP 的三个原语

| 原语 | 类型 | 说明 | 示例 |
|------|------|------|------|
| **Resources** | 数据 | 只读数据，类似文件或数据库记录 | 当前钱包余额、合约 ABI |
| **Tools** | 动作 | 模型可以调用的函数，有副作用 | 发送交易、读取链上数据 |
| **Prompts** | 模板 | 预定义的 prompt 模板，可带参数 | "分析这个合约地址的交易历史" |

## 传输方式

- **stdio**：通过标准输入/输出通信，适合本地工具
- **SSE（Server-Sent Events）**：通过 HTTP 流通信，适合远程服务

## MCP vs 传统 Function Calling 的区别

| 维度 | Function Calling | MCP |
|------|-----------------|-----|
| 标准化 | 各厂商不同格式 | 统一协议 |
| 可复用性 | 绑定单个应用 | 任何 MCP 宿主都能用 |
| 工具发现 | 需手动配置 | Server 自动声明工具列表 |
| 安全边界 | 应用层自行处理 | Server 端可设置权限 |

## MCP 的 Web3 应用场景

```python
# 一个简化的 Web3 MCP Server 示例
from mcp.server import Server
from mcp import types

server = Server("web3-tools")

@server.list_tools()
async def list_tools():
    return [
        types.Tool(
            name="get_eth_balance",
            description="查询以太坊地址的 ETH 余额",
            inputSchema={
                "type": "object",
                "properties": {
                    "address": {"type": "string", "description": "0x 开头的地址"}
                }
            }
        ),
        types.Tool(
            name="get_transaction",
            description="查询交易详情（只读，不需要签名）",
            inputSchema={
                "type": "object",
                "properties": {
                    "tx_hash": {"type": "string"}
                }
            }
        )
    ]
```

**注意：涉及签名和转账的工具不应该放在 MCP Server 中直接暴露给模型，必须有人工确认节点。**

## 安全考量

MCP Server 是 AI 和外部世界之间的网关，安全设计至关重要：

| 风险 | 描述 | 应对 |
|------|------|------|
| 权限过大 | Server 暴露了比必要更多的操作 | 最小权限原则 |
| 信任链 | 模型可能被注入恶意 prompt 触发危险工具 | 对写入操作加确认 |
| 数据泄露 | Resources 暴露敏感数据 | 严格控制 Resources 范围 |
| 工具滥用 | 无限制调用导致费用/资源消耗 | 速率限制 |

**核心原则：只读操作可以让 AI 自动调用；写入操作（签名、转账、合约部署）必须人工确认。**

## 当前生态

MCP 正在快速成长，已有的 MCP Server 包括：
- 文件系统操作
- 浏览器控制（Puppeteer）
- 数据库（SQLite、PostgreSQL）
- GitHub、Slack、Notion
- 各类 Web3 工具（Etherscan、RPC 节点）

Claude Code 本身就是一个 MCP 宿主，可以通过 `.claude/settings.json` 配置 MCP Server。

## 个人理解 / 未解决问题

- MCP Server 的身份验证机制是什么？如何防止未授权调用？
- Hermes Agent 如何集成 MCP Server？（已看到 Day 0 配置中用了 Hermes）
- 是否有专门针对 Web3/Ethereum 的开源 MCP Server 可以直接使用？
