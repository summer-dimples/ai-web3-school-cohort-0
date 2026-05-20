# 智能合约 + 网络层 + DeFi 基础

> 对应 chxii 5.19 内容 | 学习日期：2026-05-20
> 面向：有 AI 背景的 Web3 新手

## 用 AI 视角理解智能合约

| AI 类比 | Web3 对应 |
|---------|----------|
| 云函数（Lambda）| 智能合约 |
| 函数调用 | 合约交互 |
| 数据库持久化 | 链上状态存储 |
| API 调用费用 | Gas 费 |
| 无服务器 | 无运营者（代码即法律）|

**关键差异：** 云函数可以修改、下线、被公司控制；智能合约部署后**代码不变、任何人可调用、无法被停止**。

---

## 智能合约基础

### Solidity 核心概念

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SimpleStorage {
    // 状态变量：永久存储在链上（类似数据库字段）
    uint256 public storedValue;
    address public owner;

    // 构造函数：部署时执行一次
    constructor() {
        owner = msg.sender;  // 部署者的地址
    }

    // Write 函数：改变链上状态，需要 Gas + 签名
    function set(uint256 _value) public {
        storedValue = _value;
    }

    // Read 函数：只读，免费，不需要签名
    function get() public view returns (uint256) {
        return storedValue;
    }

    // Pure 函数：不读链上状态，纯计算，最便宜
    function add(uint256 a, uint256 b) public pure returns (uint256) {
        return a + b;
    }
}
```

### 函数可见性

| 修饰符 | 谁能调用 | 用途 |
|--------|---------|------|
| `public` | 任何人 | 对外接口 |
| `external` | 只能外部调用 | 节省 gas |
| `internal` | 合约内部 + 子合约 | 内部逻辑 |
| `private` | 只有本合约 | 最严格限制 |

### Storage vs Memory

```solidity
// Storage：永久存在链上，改变需要 Gas
uint256 public data;  // 状态变量默认 storage

// Memory：函数执行期间临时存在，函数返回后消失
function process(string memory input) public {
    // input 只在函数调用期间存在
}
```

**类比：** Storage = 数据库；Memory = 函数局部变量。

### Remix 操作指南

在 [remix.ethereum.org](https://remix.ethereum.org) 中：

```
蓝色按钮 = View/Pure 函数 = 读取，免费
橙色按钮 = Write 函数 = 写入，需要 Gas + MetaMask 签名
红色按钮 = Payable 函数 = 涉及 ETH 转账
```

**部署流程：**
1. 左侧 File Explorer 创建 `.sol` 文件
2. Solidity Compiler 编译（Ctrl+S）
3. Deploy & Run：选择 `Injected Provider - MetaMask`
4. 确认 MetaMask 弹出的交易
5. 记录合约地址

---

## 网络层

### 以太坊共识机制（PoS）

```
验证者（Validator）质押 32 ETH
    ↓
被随机选中提议新区块
    ↓
其他验证者验证并投票
    ↓
达到 2/3 票数 → 区块最终确定（Finalized）
    ↓
验证者获得 ETH 奖励
```

**vs PoW（已废弃）：** PoW 靠算力竞争（烧电），PoS 靠质押竞争（更环保）。

### L1 vs L2

| 特性 | L1（以太坊主网）| L2（Optimism/Arbitrum/Base）|
|------|--------------|---------------------------|
| 安全性 | 最高 | 继承自 L1 |
| Gas 费 | 高（$5-$50+）| 低（$0.01-$0.5）|
| 速度 | 慢（12秒/块）| 快（亚秒级）|
| 适合 | 高价值操作 | 高频小额操作 |

**为什么用 L2：** 大多数 DApp 在 L2 上，费用更低，体验更好。学习阶段用 Sepolia 测试网即可。

### RPC（Remote Procedure Call）

```
你的应用  →  RPC 请求  →  以太坊节点
          ←  链上数据  ←

格式示例：
POST https://eth-sepolia.g.alchemy.com/v2/YOUR_KEY
{
  "method": "eth_getBalance",
  "params": ["0x1234...", "latest"]
}
```

**RPC 提供商：** Alchemy、Infura、QuickNode（免费套餐够学习用）
**本地节点：** Hardhat 内置节点、Anvil（Foundry）

---

## DeFi 核心概念

### DEX（去中心化交易所）

**Uniswap 的 AMM（自动做市商）：**
```
x × y = k（恒积公式）

池子里有 100 ETH 和 300,000 USDC
k = 100 × 300,000 = 30,000,000

你要买 1 ETH：
新的 USDC 余额 = k / (100 - 1) = 30,000,000 / 99 ≈ 303,030
你需要付：303,030 - 300,000 = 3,030 USDC（含滑点）
```

**类比：** AMM 就像一个自动化的"汇率算法"，不需要对手方，用流动性池代替订单簿。

### 借贷协议（Compound/Aave）

```
存款者 → 存入 ETH → 获得利息
借款者 → 抵押 ETH → 借出 USDC → 支付利率

清算风险：
如果 ETH 价格下跌，抵押物价值 < 借款价值
→ 清算人可以折扣买走你的抵押品
```

### Oracle（预言机）

**问题：** 链上合约无法直接访问链外数据（ETH 价格、天气、随机数）。

**解决方案（Chainlink）：**
```
链外数据源 → Chainlink 节点网络（多个独立节点）→ 聚合 → 链上合约
```

**为什么需要去中心化预言机：** 单一数据源 = 单点失败 + 可操纵。Chainlink 用多节点投票防止操纵。

---

## Account Abstraction（账户抽象）

### ERC-4337 核心组件

```
用户操作（UserOperation）
    ↓
Bundler（打包者）
    ↓
EntryPoint 合约（单例，0x5FF1...）
    ↓
Smart Account（用户的智能账户）
         ↑
    Paymaster（可选，代付 Gas）
```

### 对 AI Agent 的意义

Session Key（会话密钥）是 AI × Web3 的关键原语：

```
用户 → 给 AI Agent 颁发 Session Key
Session Key 权限限制：
  - 只能操作特定合约
  - 每次最多花费 0.01 ETH
  - 有效期 24 小时
  - 不能提款到新地址
```

这让 AI Agent 能做一些有限授权的链上操作，而不需要接触主私钥。

---

## 个人行动清单

- [ ] 在 Remix 上部署 SimpleStorage，理解蓝色 vs 橙色按钮
- [ ] 看一笔 Uniswap 交易，在 Etherscan 上找 input data 和 logs
- [ ] 了解 Alchemy 免费 RPC，为后续脚本做准备
