# Digital Genesis

> **AstrBot × MaiBot × AIRI × Hermes × MemPalace — 五合一融合架构**
>
> MaiBot 是灵魂，AIRI 是化身，AstrBot 是大脑，Hermes 是双手，MemPalace 是记忆
> —— 创世，让数字生命降临。

## 核心理念

| 维度 | 负责者 | 说明 |
|------|--------|------|
| **怎么说** (Personality) | MaiBot | 人格、语气、情感、氛围感知 |
| **怎么表现** (Embodiment) | AIRI | 视觉形象、语音、动画、游戏 |
| **怎么决策** (Decision) | AstrBot | 大脑中枢、平台路由、任务调度、插件生态 |
| **怎么做到** (Execution) | Hermes | 技术执行、代码、部署、运维、调研、文件操作 |
| **怎么记** (Memory) | MemPalace | 记忆宫殿、知识图谱、语义检索 |

## 五大项目

| 项目 | Star | 语言 | 核心能力 |
|------|------|------|----------|
| [MemPalace](https://github.com/mempalace/mempalace) | 55.5k | Python | 记忆系统标杆，96.6% R@5，29 MCP 工具 |
| [AIRI](https://github.com/moeru-ai/airi) | 40.9k | TypeScript | 数字化身，Live2D/VRM，游戏，多端 |
| [AstrBot](https://github.com/AstrBotDevs/AstrBot) | 34.9k | Python | 大脑中枢，18+ 平台，1000+ 插件 |
| [Hermes](https://github.com/NousResearch/hermes-agent) | — | Python | 技术执行者，代码/部署/运维/调研 |
| [MaiBot](https://github.com/Mai-with-u/MaiBot) | 5.1k | Python | 拟人化数字生命，"最像而不是好" |

## 架构总览

```
用户 (QQ/微信/TG/Discord/直播/Web)
  │
  ▼
AstrBot (大脑) ── 决策中枢，路由到合适的 Agent
  ├── MaiBot (灵魂) ── 人格化回复
  ├── AIRI (化身)   ── 视觉/语音/游戏
  ├── Hermes (双手) ── 技术执行
  └── MemPalace (记忆) ── 所有系统共享
```

## 详细设计

👉 [ARCHITECTURE.md](./ARCHITECTURE.md)
