# Week 1 模块 B｜Web3 基础：账户、钱包、签名与链上执行

## 核心知识点清单

- [ ] 账户、地址和钱包的关系：私钥 → 地址，钱包是管理入口
- [ ] 助记词 / 私钥 / 地址分别是什么，为什么不能泄露
- [ ] 安全底线：地址 ≠ 匿名，签名 ≠ 普通登录，授权 ≠ 转账
- [ ] AI Agent 不应直接接触私钥 / 助记词；签名 / 授权 / 转账必须保留人工确认
- [ ] 签名与交易的关系：签名 = 授权一个具体动作
- [ ] Gas 的含义：链上执行成本 / 失败 / 等待确认
- [ ] L1 / L2 与执行成本的关系
- [ ] 智能合约 vs 普通后端：状态公开 / 执行公开 / 升级权限可检查
- [ ] 主网 vs 测试网：学习实验先在测试网完成
- [ ] 区块浏览器 / 钱包提示 / 交易回执如何辅助理解链上行为

## 任务 1｜创建测试钱包

**目标：** 创建钱包，理解地址 / 助记词 / 私钥，建立安全意识。

**工具：** MetaMask（推荐）或其他 EVM 兼容钱包

**完成标准：**
- [ ] 钱包已创建
- [ ] 能解释地址 / 助记词 / 私钥分别是什么
- [ ] 助记词已安全离线保存（不截图 / 不上传 / 不记录在 repo 中）
- [ ] 以下内容记录在本文件中（仅记录地址，不记录其他）：

**测试钱包地址：**
<!-- 0x... -->

**概念解释（用自己的话）：**
<!-- 地址是什么：-->
<!-- 助记词是什么，为什么不能泄露：-->
<!-- 私钥是什么，与地址的关系：-->

## 任务 2｜测试网交互

**目标：** 切换到测试网，领取测试币，发送一笔测试交易，在区块浏览器中验证。

**测试网：** Sepolia（推荐）

**步骤：**
1. [ ] 在 MetaMask 中切换到 Sepolia 测试网
2. [ ] 从 Sepolia Faucet 领取测试 ETH
3. [ ] 向另一个地址（或自己的第二个钱包）发送一笔测试交易
4. [ ] 在 Sepolia Etherscan 中找到交易记录

**Faucet 地址：** https://sepoliafaucet.com 或 https://faucet.sepolia.dev

**区块浏览器：** https://sepolia.etherscan.io

**记录：**

| 项目 | 值 |
|------|----|
| 交易哈希 (Tx Hash) | |
| 交易状态 | |
| Gas Used | |
| 区块高度 | |
| 区块浏览器链接 | |

**遇到的问题：**
<!-- 记录卡点和解决方法 -->

## 任务 3｜部署最小智能合约

**目标：** 用 Remix 部署一个最小合约，完成一次读取和一次写入。

**工具：** https://remix.ethereum.org

**最小合约示例（SimpleStorage）：**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SimpleStorage {
    uint256 public storedValue;

    function set(uint256 _value) public {
        storedValue = _value;
    }

    function get() public view returns (uint256) {
        return storedValue;
    }
}
```

**步骤：**
1. [ ] 在 Remix 中创建并编译合约
2. [ ] 连接 MetaMask（Sepolia 测试网）并部署
3. [ ] 调用 `set()` 写入一个值
4. [ ] 调用 `get()` 读取验证

**记录：**

| 项目 | 值 |
|------|----|
| 合约地址 | |
| 部署交易哈希 | |
| 写入交易哈希 | |
| 区块浏览器链接 | |

## 高级挑战（可选）

- [ ] 比较 EOA、智能账户、多签在权限控制和自动化执行上的差异
- [ ] 用 Hardhat 或 Foundry 完成合约开发、测试与部署，将测试记录写入 repo
- [ ] 了解 ERC-4337 账户抽象：Session Key / 权限限额 / 社交恢复
- [ ] 理解为什么链上执行不能简单理解成"普通后端调用"

## 推荐材料

- Ethereum Accounts 文档
- MetaMask Getting Started
- Ethereum Development Documentation
- Remix IDE: https://remix.ethereum.org
- Sepolia Faucet
- OpenZeppelin Contracts
- Safe Overview / ERC-4337 文档

## 状态

- [ ] 任务 1 完成（测试钱包）
- [ ] 任务 2 完成（测试网交互）
- [ ] 任务 3 完成（合约部署）
