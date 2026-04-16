# openclaw-feishu-multi-bot

One OpenClaw instance, multiple Feishu bot identities. Each Agent appears as an independent bot in Feishu with its own name, avatar, and group routing.

一个 OpenClaw 实例，多个飞书机器人身份。每个 Agent 在飞书中以独立机器人的形式出现，拥有各自的名称、头像和群组路由。

## What This Solves / 解决什么问题

When running multiple specialized Agents on OpenClaw, they share a single Feishu bot entry by default — all messages mix together. This skill teaches you how to give each Agent its own Feishu enterprise app, so users see completely separate assistants.

在 OpenClaw 上运行多个专业 Agent 时，它们默认共享同一个飞书机器人入口，所有消息混在一起。这个 Skill 教你如何给每个 Agent 配置独立的飞书企业应用，让用户看到完全独立的助手。

## What's Included / 包含内容

```
openclaw-feishu-multi-bot/
├── SKILL.md                           # Entry point — architecture + quick start / 入口 — 架构概览 + 快速开始
├── references/
│   ├── architecture.md                # Three-block config model / 三段式配置模型与 accountId 机制
│   ├── build-guide.md                 # Step-by-step guide / 从飞书后台到网关重启的完整步骤
│   ├── routing-deep-dive.md           # accountId consistency / accountId 一致性与路由规则
│   └── troubleshooting.md             # Diagnostic flowchart / 诊断流程图 + 7 个常见问题修复
└── scripts/
    └── setup-feishu-bots.sh           # Auto-generates config / 自动生成配置 JSON 块
```

## Quick Start / 快速开始

```bash
# Generate config for 3 agents / 为 3 个 Agent 生成配置
./scripts/setup-feishu-bots.sh \
  orchestrator:cli_xxx:secret1 \
  writer:cli_yyy:secret2 \
  coder:cli_zzz:secret3

# Merge output into openclaw.json, then: / 合并到 openclaw.json，然后：
openclaw doctor && openclaw gateway restart
```

## Key Lessons / 核心经验

- **accountId consistency / accountId 一致性** is the #1 failure point — must match in 3 places / 是首要故障点，必须在 3 处保持一致
- **Binding type must be `"route"` / 绑定类型必须是 `"route"`** — anything else crashes the gateway / 否则网关崩溃
- **Feishu apps must be published / 飞书应用必须发布** — draft apps silently drop all messages / 草稿状态会静默丢弃所有消息
- **`agentToAgent` must be OFF / `agentToAgent` 必须关闭** — breaks all sub-agent spawning (Bug #5813) / 否则子 Agent 调用全部失败
- **Incremental rollout / 增量上线** — add one bot at a time, not all at once / 一次加一个机器人，不要一次全加

## Who This Is For / 适用人群

- OpenClaw users running 3+ Agents who want per-role Feishu entry points / 运行 3 个以上 Agent 并希望每个角色有独立飞书入口的用户
- Teams using Feishu/Lark as their primary communication platform / 以飞书/Lark 为主要沟通平台的团队
- Anyone debugging Feishu bot routing issues in OpenClaw / 在 OpenClaw 中排查飞书机器人路由问题的人

## Author / 作者

[@simonlin1212](https://clawhub.ai/simonlin1212) — Based on production experience running 10+ Agents with independent Feishu bots / 基于运营 10+ 个独立飞书机器人 Agent 的生产经验
