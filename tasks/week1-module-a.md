# Week 1 模块 A｜AI 基础：从 LLM 到 Agent Workflow

## 核心知识点清单

- [ ] LLM 工作方式：基于上下文的概率生成，token 预测
- [ ] LLM 擅长 vs 不擅长的边界（语言理解 / 代码生成 vs 精确事实 / 跨会话状态）
- [ ] 四个控制层面：上下文窗口 / 系统指令 / 提示词 / 工具调用
- [ ] MaaS 概念：无需 GPU，API Key 按 token 付费
- [ ] Prompt → Workflow → Agent 的边界与风险差异
- [ ] AI Coding 工具的价值（加速原型）与限制（架构决策 / 测试设计）
- [ ] AI 输出必须验证的五类风险：事实错误 / 引用错误 / 推理漂移 / 执行越权 / 工具误用
- [ ] Agent 核心组件：状态管理 / 长期记忆 / MCP / Tool Calling / Tracing / Guardrails / Handoff / 错误恢复

## 任务 1｜搭建 Learning Agent

**目标：** 选定主工具，完成安装与配置，跑通一次真实学习任务。

**工具选择（三选一）：**
- [ ] Claude Code（已有 Claude 会员）
- [ ] Codex CLI（已有 OpenAI 会员）
- [ ] GLM / Z.ai API（无会员，申请 API Key）

**完成标准：**
- [ ] 工具安装 / 登录 / API 配置完成
- [ ] 把 Week 1 大纲交给 agent，让它生成：
  - 个人学习计划
  - 关键概念解释
  - 待完成 checklist
  - 不懂问题清单
- [ ] 记录一次完整对话日志（放入 `experiments/` 或 `daily/`）

**配置说明：**
<!-- 记录你选择的工具、API 路径、配置步骤 -->

**Agent 协助学习日志：**
<!-- 粘贴或链接到对话记录 -->

## 任务 2｜创建个人 GitHub Repo

**目标：** 本 repo 即为学习工作区，持续更新。

**完成标准：**
- [x] Repo 已创建并初始化
- [ ] README 已更新，包含本周目标
- [ ] 目录结构已建立
- [ ] 至少有一个 commit 记录

**Repo 链接：** https://github.com/summer-dimples/ai-web3-school-cohort-0

## 任务 3｜用 Agent 生成可交互学习产物

**目标：** 选一个 Week 1 概念，让 agent 帮你生成可交互产物。

**选定概念：**
<!-- LLM / workflow / agent / 钱包 / 签名 / 交易 / Gas / 合约执行 -->

**产物类型（选一）：**
- [ ] 小页面（HTML/JS）
- [ ] CLI 工具
- [ ] 流程图（Mermaid）
- [ ] Quiz / 概念卡片
- [ ] 最小 demo

**完成标准：**
- [ ] 产物已放入 `experiments/` 目录
- [ ] README 中说明：让 agent 做了什么 / 人工修改了什么 / 哪些输出不可靠 / 下一步如何改进

**产物位置：** `experiments/`

**说明：**
<!-- agent 做了什么，你改了什么，输出哪里不可靠 -->

## 高级挑战（可选）

- [ ] 用 Claude Code / Codex / Hermes 中至少两个工具处理同一学习任务，比较差异
- [ ] 让同一任务分别用"纯提示词" / "AI coding workflow" / "agent + skills"三种方式完成
- [ ] 设计或调用一个与 Ethereum 相关的 skill，记录可靠边界

## 推荐材料

- What is a Large Language Model?（视频）
- Hugging Face LLM Course Chapter 1
- Anthropic: Building with the Claude API
- Z.ai API 开发者文档（GLM MaaS）
- Claude Code 101
- AI Agent 入门（视频）
- Microsoft《AI Agents for Beginners》
- OpenAI Agents SDK Intro

## 状态

- [ ] 任务 1 完成
- [ ] 任务 2 完成
- [ ] 任务 3 完成
