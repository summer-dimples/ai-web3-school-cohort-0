# Indexing + Security + Dev Stack

> 对应 chxii 5.20 内容 | 学习日期：2026-05-20
> 面向：有 AI 背景的 Web3 新手

## Indexing（链上数据索引）

### 问题：链上数据很难查

区块链是一条"只追加"的交易记录，查询"某地址的全部转账记录"需要扫描所有区块，**极慢**。

**类比：** 相当于在没有索引的数据库里做全表扫描。

### Events 和 Logs

合约通过 `emit` 发出事件，事件存储在区块的 Logs 字段中：

```solidity
// 合约代码
event ValueChanged(address indexed setter, uint256 newValue);

function set(uint256 _value) public {
    storedValue = _value;
    emit ValueChanged(msg.sender, _value);  // 发出事件
}
```

**注意：** 合约必须主动 `emit`，否则 Remix/Etherscan 的 Events 标签是空的。

**`indexed` 关键字：** 最多 3 个参数可以 indexed，indexed 的参数可以被高效过滤查询。

### The Graph

**是什么：** 去中心化的区块链数据索引协议，相当于区块链的"Google 搜索引擎"。

**类比（AI 视角）：**
```
区块链原始数据 → The Graph 索引 → GraphQL 查询
     ≈
原始文本语料  → 向量数据库索引 → 语义搜索
```

**工作流程：**
```
1. 开发者定义 Subgraph（描述索引哪些合约、哪些事件）
2. The Graph 节点监听链上事件，建立索引
3. 应用通过 GraphQL 查询数据

示例查询（Uniswap Subgraph）：
{
  swaps(first: 5, orderBy: timestamp, orderDirection: desc) {
    id
    amount0
    amount1
    timestamp
  }
}
```

**应用场景：**
- 查询 DEX 的交易历史
- NFT 持有人列表
- 地址的全部 ERC-20 转账记录
- DeFi 协议的 TVL 历史

---

## Security（合约安全）

### 四大经典漏洞

#### 1. Reentrancy（重入攻击）

**最著名漏洞：** 2016 年 The DAO 被盗 360 万 ETH。

**原理：**
```solidity
// 漏洞合约
function withdraw() public {
    uint256 amount = balances[msg.sender];
    // 先转账（触发接收者的 fallback）
    (bool success, ) = msg.sender.call{value: amount}("");
    // 攻击者在 fallback 里再次调用 withdraw！
    // 此时 balances 还没有更新
    balances[msg.sender] = 0;  // 太晚了
}

// 修复（Checks-Effects-Interactions 模式）
function withdraw() public {
    uint256 amount = balances[msg.sender];
    balances[msg.sender] = 0;  // 先更新状态
    (bool success, ) = msg.sender.call{value: amount}("");  // 再转账
}
```

**防御工具：** OpenZeppelin 的 `ReentrancyGuard`

#### 2. Integer Overflow/Underflow

**Solidity 0.8+ 已自动检查，旧版本有风险。**

```solidity
// 旧版本（< 0.8）漏洞
uint8 x = 255;
x + 1 = 0  // 溢出！

// 0.8+ 会自动 revert，无需手动处理
```

#### 3. Access Control（访问控制）

```solidity
// 漏洞：任何人都能调用
function setOwner(address _new) public {
    owner = _new;
}

// 修复：加权限检查
function setOwner(address _new) public {
    require(msg.sender == owner, "Not owner");
    owner = _new;
}

// 更好：用 OpenZeppelin Ownable
import "@openzeppelin/contracts/access/Ownable.sol";
contract MyContract is Ownable {
    function sensitiveAction() public onlyOwner { ... }
}
```

#### 4. Front-Running（抢先交易）

**原理：** 交易在广播到内存池（mempool）后，矿工/验证者能看到并抢先执行。

```
用户发出：低价买入某 NFT
MEV Bot 看到后：用更高 Gas 费提前买入 → 再高价卖给用户
```

**防御：** Commit-Reveal 方案、Private Transaction（Flashbots）、时间锁。

### 安全工具

| 工具 | 类型 | 用途 |
|------|------|------|
| Slither | 静态分析 | 自动检测常见漏洞 |
| Mythril | 符号执行 | 深度漏洞分析 |
| echidna | 模糊测试 | 随机输入找边界漏洞 |
| OpenZeppelin | 安全库 | 经过审计的合约组件 |
| Foundry Fuzzing | 模糊测试 | 内置 Foundry |

**AI × 合约安全：** AI 生成的合约代码**不能跳过安全审查**，即使是简单合约也应至少用 Slither 检查一次。

---

## Dev Stack（开发工具链）

### 工具链全景

```
编写代码：VS Code + Solidity 插件
编译：solc（Solidity 编译器）
测试框架：Hardhat（JS/TS）或 Foundry（Rust）
本地节点：Hardhat Network 或 Anvil（Foundry）
前端交互：ethers.js 或 viem
节点服务：Alchemy / Infura / QuickNode
合约验证：Etherscan verify
```

### Hardhat vs Foundry

| 维度 | Hardhat | Foundry |
|------|---------|---------|
| 语言 | JavaScript/TypeScript | Rust（工具本身），Solidity 测试 |
| 学习曲线 | 较低（JS 生态熟悉）| 较高（但测试直接用 Solidity）|
| 速度 | 较慢 | 极快 |
| 测试风格 | JS 测试 + chai | Solidity 测试文件 |
| 适合 | 有 JS 背景 | 纯 Solidity 开发者 |
| 社区 | 大，文档多 | 增长快，Cyfrin 课程用 |

**Week 1 建议：** 先用 Remix（零配置），想深入再选 Hardhat 或 Foundry。

### ethers.js vs viem

```javascript
// ethers.js（老牌，更多教程）
const { ethers } = require("ethers");
const provider = new ethers.JsonRpcProvider("https://sepolia.alchemy.com/v2/KEY");
const balance = await provider.getBalance("0x1234...");

// viem（现代，TypeScript 优先，更高效）
import { createPublicClient, http } from "viem";
import { sepolia } from "viem/chains";
const client = createPublicClient({ chain: sepolia, transport: http() });
const balance = await client.getBalance({ address: "0x1234..." });
```

**Week 1 建议：** 知道它们是什么即可，实际写脚本时再深入。

### 节点服务申请（免费套餐）

1. [Alchemy](https://alchemy.com)：注册 → Create App → 选 Ethereum + Sepolia → 获取 HTTPS URL
2. 免费额度：300M compute units/月，够学习用

---

## 今日实践重点（Android/Termux 可完成）

由于你在 Termux 环境，以下是可操作的路径：

**必须完成（Remix 在手机浏览器可用）：**
- [ ] 打开 [remix.ethereum.org](https://remix.ethereum.org)，创建 SimpleStorage.sol
- [ ] 连接 MetaMask，部署到 Sepolia
- [ ] 点击 `set(42)` 写入，点击 `get()` 验证
- [ ] 在 [sepolia.etherscan.io](https://sepolia.etherscan.io) 找到合约地址，查看 Transactions 和 Events

**扩展了解：**
- [ ] 在 Etherscan 随便找一笔 Uniswap 交易，看它的 Logs 标签
- [ ] 注册 Alchemy 获取免费 RPC URL

---

## 个人理解 / 未解决问题

- Slither 能在 Termux 上安装吗？（Python 包，理论上可以）
- The Graph 的 Subgraph 索引延迟大概是多少？
- MEV 对普通用户的实际影响有多大？日常使用时需要担心吗？
