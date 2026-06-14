# AstrBot × MaiBot Fusion

> MaiBot 作为人格灵魂，AstrBot 作为能力身体 —— 一个有温度、有记忆、有技能的 AI 伙伴。

## 架构概览

```
MaiBot (灵魂)                    AstrBot (身体)
─────────────                    ─────────────
"说什么"                         "怎么做"
"怎么说"                         "怎么记"
"何时说"                         "怎么找"
"对谁说什么"                     "怎么发"
```

- **MaiBot** 负责：人格系统、推理引擎、氛围感知、表达学习、情感模型
- **AstrBot** 负责：记忆系统、知识库 (RAG)、Skills、工具执行、多平台路由 (18+)

## 集成方式

通过 **MCP (Model Context Protocol)** 双向桥接：

- AstrBot 作为 MCP Server，暴露记忆/知识/技能/工具
- MaiBot 作为 MCP Client，调用 AstrBot 的能力

## 详细设计

👉 [ARCHITECTURE.md](./ARCHITECTURE.md)

## 相关项目

- [AstrBotDevs/AstrBot](https://github.com/AstrBotDevs/AstrBot) - AI Agent 聊天机器人平台 (34.6k ⭐)
- [Mai-with-u/MaiBot](https://github.com/Mai-with-u/MaiBot) - 拟人化数字生命 Agent (5.1k ⭐)
