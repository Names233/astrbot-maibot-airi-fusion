# AstrBot × MaiBot × AIRI × MemPalace 四合一融合架构

> MaiBot 是灵魂，AIRI 是化身，AstrBot 是身体，MemPalace 是记忆宫殿 —— 完整的数字生命。

## 核心理念

| 维度 | 负责者 | 说明 |
|------|--------|------|
| **怎么说** (Personality) | MaiBot | 人格、语气、情感、氛围感知 |
| **怎么表现** (Embodiment) | AIRI | 视觉形象、语音、动画、游戏 |
| **怎么做到** (Capability) | AstrBot | 技能、工具、平台路由、插件生态 |
| **怎么记** (Memory) | MemPalace | 记忆宫殿、知识图谱、语义检索 |

```
┌──────────────────────────────────────────────────────────────────┐
│                          用户                                    │
│           QQ / 微信 / Telegram / Discord / 直播 / Web            │
└───────────────────────┬──────────────────────────────────────────┘
                        │
                        ▼
┌──────────────────────────────────────────────────────────────────┐
│                AstrBot (能力层 / Backend)                         │
│                                                                   │
│   平台路由 │ Skills │ 工具执行 │ 插件生态 │ MCP Hub              │
└───────────┬──────────────────────────────────────────────────────┘
            │ MCP
            ▼
┌──────────────────────────────────────────────────────────────────┐
│                MaiBot (灵魂层 / Personality)                      │
│                                                                   │
│   推理引擎 │ 人格系统 │ 情感模型 │ 氛围感知 │ 表达学习            │
└───────────┬──────────────────────────────────────────────────────┘
            │ MCP / WebSocket
            ▼
┌──────────────────────────────────────────────────────────────────┐
│                AIRI (化身层 / Embodiment)                         │
│                                                                   │
│   Live2D/VRM │ TTS/STT │ 口型同步 │ 游戏Agent │ 多端渲染         │
└──────────────────────────────────────────────────────────────────┘

                        ▲ 全局共享 ▲
┌──────────────────────────────────────────────────────────────────┐
│              MemPalace (记忆层 / Memory Palace)                   │
│                                                                   │
│   Wings(人/项目) │ Rooms(话题) │ Drawers(原文) │ 知识图谱          │
│   语义检索 │ BM25+向量混合 │ Agent日记 │ 会话挖掘                  │
│   96.6% R@5 │ 零API调用 │ 29 MCP Tools │ 本地优先                 │
└──────────────────────────────────────────────────────────────────┘
```

---

## 一、四个项目各自的能力

### 1.1 MemPalace — 记忆层 (新增)

仓库: https://github.com/mempalace/mempalace
Star: 55.5k | Python 3.9+ | License: MIT | 版本: v3.4.0

**核心概念: 宫殿结构**
```
Palace (宫殿)
  └── Wing (翼楼) — 按人/项目组织
       └── Room (房间) — 按话题分类
            └── Drawer (抽屉) — 原文逐字存储
```

**核心能力:**

| 能力 | 说明 |
|------|------|
| 逐字存储 | 不摘要、不改写、不丢失细节 |
| 混合检索 | BM25 关键词 + 向量语义，96.6% R@5 (LongMemEval) |
| 知识图谱 | 时序实体-关系图谱 (SQLite)，带有效期窗口 |
| Agent 日记 | 每个 Agent 独立 Wing + Diary |
| 会话挖掘 | 自动从 Claude Code/ChatGPT/Slack 等导入对话 |
| 插件后端 | ChromaDB (默认)、SQLite、Qdrant、pgvector |
| 本地优先 | 零 API 调用，核心路径完全离线 |
| MCP 原生 | 29 个 MCP 工具，开箱即用 |

**29 个 MCP 工具:**

```
# 宫殿操作
mempalace_status           # 宫殿状态
mempalace_list_wings       # 列出所有翼楼
mempalace_list_rooms       # 列出房间
mempalace_get_taxonomy     # 获取分类体系

# 记忆读写
mempalace_search           # 混合语义搜索
mempalace_add_drawer       # 添加记忆抽屉
mempalace_get_drawer       # 读取抽屉内容
mempalace_update_drawer    # 更新抽屉
mempalace_delete_drawer    # 删除抽屉
mempalace_list_drawers     # 列出抽屉
mempalace_check_duplicate  # 去重检查

# 知识图谱
mempalace_kg_query         # 查询实体关系
mempalace_kg_add           # 添加三元组
mempalace_kg_invalidate    # 使关系失效
mempalace_kg_timeline      # 时间线查询
mempalace_kg_stats         # 图谱统计

# 导航
mempalace_traverse_graph   # 图遍历
mempalace_find_tunnels     # 跨翼楼隧道
mempalace_create_tunnel    # 创建隧道
mempalace_list_tunnels     # 列出隧道
mempalace_follow_tunnels   # 沿隧道导航

# Agent
mempalace_diary_write      # 写 Agent 日记
mempalace_diary_read       # 读 Agent 日记
mempalace_list_agents      # 列出所有 Agent

# 同步
mempalace_sync             # 同步项目文件
mempalace_hook_settings    # 自动保存设置
```

### 1.2 AstrBot — 能力层

仓库: https://github.com/AstrBotDevs/AstrBot
Star: 34.6k | Python 3.10+ | 版本: v4.25.5

| 模块 | 职责 |
|------|------|
| 平台路由 | QQ/微信/TG/Discord 等 18+ 平台消息收发 |
| Skills | 技能管理、注册、执行 |
| 工具执行 | Agent 工具调用和沙箱执行 |
| MCP Hub | MCP 协议连接外部工具服务 |
| 插件生态 | 1000+ 插件的加载和管理 (Stars) |
| 会话管理 | 多轮对话上下文管理 |

### 1.3 MaiBot — 灵魂层

仓库: https://github.com/Mai-with-u/MaiBot
Star: 5.1k | Python 3.10+ | License: GPL-3.0

| 模块 | 职责 |
|------|------|
| 推理引擎 | 决定是否回复、用什么语气回复 |
| 人格系统 | 角色设定、说话风格、性格特征 |
| 氛围感知 | 判断群聊气氛，决定发言时机 |
| 表达学习 | 学习用户的说话方式并模仿 |
| 行为学习 | 学习用户的行为模式 |
| 术语挖掘 | 理解圈子黑话和新词 |
| 用户画像 | 累积对用户的了解 |

设计理念: "最像而不是好"

### 1.4 AIRI — 化身层

仓库: https://github.com/moeru-ai/airi
Star: 40.9k | TypeScript | License: MIT | 版本: v0.10.2

| 模块 | 职责 |
|------|------|
| Agent 运行时 | Agent 编排、对话管理 |
| Live2D/VRM | 视觉形象渲染和动画 |
| TTS/STT | 语音合成 (ElevenLabs/Azure/OpenAI/Kokoro) + 语音识别 |
| 口型同步 | 语音驱动口型动画 |
| 游戏 Agent | Minecraft、Factorio、KSP |
| 多端渲染 | Web (PWA)、桌面 (Electron)、移动端 (Capacitor) |

---

## 二、为什么需要 MemPalace

### AstrBot 原有记忆的局限

| 维度 | AstrBot 原生记忆 | MemPalace |
|------|-----------------|-----------|
| 存储方式 | SQLite 基础存储 | 逐字存储，不丢失细节 |
| 检索精度 | 单一向量搜索 | BM25 + 向量混合，96.6% R@5 |
| 组织结构 | 扁平列表 | Wings → Rooms → Drawers 层级结构 |
| 知识图谱 | 无 | 时序实体-关系图谱，带有效期 |
| Agent 记忆 | 共用一个池 | 每个 Agent 独立 Wing + Diary |
| 会话挖掘 | 无 | 自动从多平台导入对话 |
| 去重 | 基础 | 碰撞扫描 + 语义去重 |
| 跨关联 | 无 | 隧道 (Tunnels) 连接不同翼楼 |
| 基准测试 | 未公开 | LongMemEval 96.6%, LoCoMo 88.9% |

### MemPalace 在融合架构中的角色

```
所有对话 ──→ MemPalace 会话挖掘 ──→ 结构化存储
                                        │
MaiBot 推理时 ◄── mempalace_search ────┘
    "用户之前说过什么？"
    "用户喜欢什么？"
    "上次聊到哪了？"

AstrBot 执行技能时 ◄── mempalace_kg_query ──┘
    "用户的技术栈是什么？"
    "这个项目的上下文？"

AIRI 游戏时 ◄── mempalace_diary_read ──┘
    "上次玩到哪了？"
    "用户的策略偏好？"
```

---

## 三、四系统 MCP 通信拓扑

```
                    ┌─────────────────┐
                    │   MemPalace     │
                    │   MCP Server    │
                    │   (29 tools)    │
                    └────────┬────────┘
                             │ MCP (stdio/SSE)
              ┌──────────────┼──────────────┐
              ▼              ▼              ▼
     ┌────────────┐  ┌────────────┐  ┌────────────┐
     │  AstrBot   │  │   MaiBot   │  │    AIRI    │
     │  MCP Hub   │  │  MCP Hub   │  │  MCP Client│
     └────────────┘  └────────────┘  └────────────┘
```

**MCP 连接关系:**

```
AstrBot  ──MCP──► MemPalace    (记忆读写)
AstrBot  ──MCP──► MaiBot       (灵魂调用)
MaiBot   ──MCP──► AstrBot      (能力调用)
MaiBot   ──MCP──► MemPalace    (记忆读写)
AIRI     ──MCP──► MaiBot       (人格调用)
AIRI     ──MCP──► AstrBot      (工具调用)
AIRI     ──MCP──► MemPalace    (记忆读写)
```

每个系统都直接连接 MemPalace，不经过中间层，最小化延迟。

---

## 四、AstrBot MCP Server (更新版)

移除内置记忆，改为调用 MemPalace:

```python
# astrbot_mcp_server.py

from astrbot.core.star import Star, Context

class AstrBotMCPServer(Star):
    """AstrBot 能力通过 MCP 暴露，记忆委托给 MemPalace"""

    def __init__(self, mempalace_endpoint: str):
        self.mempalace = MCPClient(mempalace_endpoint)

    # ─── 记忆: 委托给 MemPalace ───

    @Tool("memory_search")
    async def memory_search(self, query: str, wing: str = None, top_k: int = 5) -> dict:
        """搜索记忆 (委托 MemPalace)"""
        return await self.mempalace.call_tool("mempalace_search", {
            "query": query, "wing": wing, "n_results": top_k
        })

    @Tool("memory_save")
    async def memory_save(self, content: str, wing: str, room: str = None) -> dict:
        """保存记忆 (委托 MemPalace)"""
        return await self.mempalace.call_tool("mempalace_add_drawer", {
            "content": content, "wing": wing, "room": room
        })

    @Tool("knowledge_graph_query")
    async def knowledge_graph_query(self, entity: str, as_of: str = None) -> dict:
        """查询知识图谱 (委托 MemPalace)"""
        return await self.mempalace.call_tool("mempalace_kg_query", {
            "entity": entity, "as_of": as_of
        })

    # ─── Skills (AstrBot 原生) ───

    @Tool("skill_execute")
    async def skill_execute(self, skill_name: str, params: dict = None) -> dict:
        result = await self.context.skill_mgr.execute(skill_name, params or {})
        return {"result": result}

    # ─── 工具 (AstrBot 原生) ───

    @Tool("tool_call")
    async def tool_call(self, tool_name: str, arguments: dict) -> dict:
        result = await self.context.tool_mgr.call(tool_name, arguments)
        return {"result": result}

    # ─── 平台 (AstrBot 原生) ───

    @Tool("send_message")
    async def send_message(self, platform: str, target: str, message: str) -> dict:
        await self.context.send_message(platform, target, message)
        return {"status": "sent"}
```

---

## 五、MaiBot 推理引擎改造 (集成 MemPalace)

```python
# fusion_reasoning_engine.py

class FusionReasoningEngine(MaisakaReasoningEngine):

    def __init__(self, astrbot_bridge, mempalace_client, **kwargs):
        super().__init__(**kwargs)
        self.astrbot = astrbot_bridge
        self.memory = mempalace_client  # 直连 MemPalace

    async def reason(self, message: SessionMessage) -> ChatResponse:

        # 1. 【MaiBot 灵魂】判断是否要回复
        should_reply = self._evaluate_should_reply(message)
        if not should_reply:
            return ChatResponse.no_reply()

        # 2. 【MemPalace 记忆】检索相关记忆
        memories = await self.memory.call_tool("mempalace_search", {
            "query": message.text,
            "wing": message.sender_id,  # 按用户翼楼搜索
            "n_results": 5
        })

        # 3. 【MemPalace 知识图谱】查询用户实体关系
        user_kg = await self.memory.call_tool("mempalace_kg_query", {
            "entity": message.sender_name,
            "as_of": datetime.now().isoformat()
        })

        # 4. 【MaiBot 灵魂】构建人格化 prompt
        prompt = self._build_persona_prompt(
            message=message,
            memories=memories,           # MemPalace 提供的记忆
            knowledge_graph=user_kg,     # MemPalace 提供的图谱
            chat_context=message.context,
        )

        # 5. 【MaiBot 灵魂】生成拟人化回复
        response = await self._generate_with_persona(prompt)

        # 6. 【AstrBot 能力】需要时调用技能/工具
        if response.needs_tool:
            tool_result = await self.astrbot.call_tool(
                response.tool_name, response.tool_args
            )
            response = await self._integrate_tool_result(response, tool_result)

        # 7. 【MaiBot 灵魂】后处理
        response = self._apply_mood_and_emoji(response)

        # 8. 【MemPalace 记忆】保存对话到记忆宫殿
        await self.memory.call_tool("mempalace_add_drawer", {
            "content": f"用户({message.sender_name}): {message.text}\n"
                       f"我: {response.text}",
            "wing": message.sender_id,
            "room": message.topic or "general"
        })

        # 9. 【MemPalace 知识图谱】更新实体关系
        if response.extracted_facts:
            for fact in response.extracted_facts:
                await self.memory.call_tool("mempalace_kg_add", {
                    "subject": fact.subject,
                    "predicate": fact.predicate,
                    "object": fact.object,
                })

        # 10. 【MemPalace 日记】写入 Agent 日记
        await self.memory.call_tool("mempalace_diary_write", {
            "agent_name": "maisaka",
            "entry": f"对 {message.sender_name} 说了: {response.text[:100]}",
            "topic": message.topic or "general"
        })

        return response
```

---

## 六、AIRI 双桥接 (更新版)

```typescript
// airi_bridge.ts

export class AIRIBridge {
  private maiBot: MCPClient     // 灵魂层
  private astrBot: MCPClient    // 能力层
  private memPalace: MCPClient  // 记忆层

  async connect(config: BridgeConfig) {
    this.maiBot = new MCPClient({ url: config.maiBotEndpoint })
    this.astrBot = new MCPClient({ url: config.astrBotEndpoint })
    this.memPalace = new MCPClient({ url: config.memPalaceEndpoint })
    await Promise.all([
      this.maiBot.connect(),
      this.astrBot.connect(),
      this.memPalace.connect()
    ])
  }

  // ─── 灵魂: 调用 MaiBot ───

  async generateReply(context: string, sender: string): Promise<ReplyResult> {
    return await this.maiBot.callTool('generate_reply', { context, sender })
  }

  async shouldReply(message: string, context: Message[]): Promise<boolean> {
    const r = await this.maiBot.callTool('evaluate_should_reply', { message, context })
    return r.should_reply
  }

  // ─── 记忆: 直连 MemPalace ───

  async recallMemory(query: string, wing?: string): Promise<Memory[]> {
    const r = await this.memPalace.callTool('mempalace_search', {
      query, wing, n_results: 5
    })
    return r.results
  }

  async saveMemory(content: string, wing: string, room?: string): Promise<void> {
    await this.memPalace.callTool('mempalace_add_drawer', { content, wing, room })
  }

  async queryKnowledgeGraph(entity: string): Promise<KGResult> {
    return await this.memPalace.callTool('mempalace_kg_query', { entity })
  }

  async readAgentDiary(agentName: string): Promise<DiaryEntry[]> {
    const r = await this.memPalace.callTool('mempalace_diary_read', {
      agent_name: agentName, last_n: 10
    })
    return r.entries
  }

  async writeAgentDiary(agentName: string, entry: string): Promise<void> {
    await this.memPalace.callTool('mempalace_diary_write', {
      agent_name: agentName, entry
    })
  }

  // ─── 能力: 调用 AstrBot ───

  async useSkill(skillName: string, params: object): Promise<object> {
    const r = await this.astrBot.callTool('skill_execute', { skill_name: skillName, params })
    return r.result
  }

  // ─── 化身: 本地能力 ───

  async speak(text: string, mood: string): Promise<void> {
    const audio = await this.tts.synthesize(text, { mood })
    await this.lipsync.play(audio)
  }

  async express(emoji: string): Promise<void> {
    await this.avatar.setExpression(emoji)
  }
}
```

---

## 七、MemPalace 翼楼规划

```
Palace (数字生命记忆宫殿)
├── wings/
│   ├── users/                    # 用户翼楼 (按用户ID)
│   │   ├── user_123/
│   │   │   ├── preferences/      # 房间: 偏好
│   │   │   ├── conversations/    # 房间: 对话历史
│   │   │   ├── emotions/         # 房间: 情感事件
│   │   │   └── facts/            # 房间: 事实信息
│   │   └── user_456/
│   │       └── ...
│   │
│   ├── groups/                   # 群组翼楼 (按群ID)
│   │   ├── group_789/
│   │   │   ├── culture/          # 房间: 群文化/黑话
│   │   │   ├── topics/           # 房间: 讨论话题
│   │   │   └── events/           # 房间: 群事件
│   │   └── ...
│   │
│   ├── maisaka/                  # Agent 翼楼 (MaiBot 自身)
│   │   ├── diary/                # 房间: Agent 日记
│   │   ├── learned_expressions/  # 房间: 学到的表达
│   │   └── mood_history/         # 房间: 情绪历史
│   │
│   ├── airi/                     # Agent 翼楼 (AIRI 自身)
│   │   ├── diary/                # 房间: Agent 日记
│   │   ├── game_progress/        # 房间: 游戏进度
│   │   └── stream_history/       # 房间: 直播历史
│   │
│   └── knowledge/                # 知识翼楼
│       ├── general/              # 房间: 通用知识
│       ├── technical/            # 房间: 技术知识
│       └── user_shared/          # 房间: 用户共享的知识
│
└── knowledge_graph/              # 知识图谱 (时序实体关系)
    ├── entities: [用户, 项目, 工具, 概念...]
    └── relationships: [喜欢, 使用, 擅长, 认识...]
        └── with: valid_from → valid_to
```

---

## 八、数据流示例

### 场景 1: 群聊 + 记忆增强

```
用户在QQ群: "推荐个Python框架"
  → AstrBot QQ适配器接收
  → MaiBot 推理引擎启动
  → MemPalace search("Python框架", wing="users/用户ID")
    → 返回: 用户3个月前说过在学 FastAPI，上周说觉得 Django 太重
  → MemPalace kg_query("用户")
    → 返回: {用户, 擅长, Python}, {用户, 偏好, 轻量框架}
  → MaiBot 人格系统生成: "你不是在用 FastAPI 嘛，挺好的呀，还要别的吗？"
  → MemPalace add_drawer(对话记录, wing="users/用户ID", room="conversations")
  → AstrBot 发回QQ群
```

### 场景 2: 直播 + 游戏 + 记忆

```
直播间弹幕: "AIRI继续建上次的城堡"
  → AIRI 接收弹幕
  → MemPalace search("城堡建造", wing="airi/game_progress")
    → 返回: 上次建到第二层，用了石砖和橡木
  → MemPalace diary_read("airi", last_n=3)
    → 返回: "上次直播建城堡到一半，观众说要加个塔楼"
  → AIRI Minecraft Agent 从上次进度继续
  → AIRI TTS: "我记得上次建到二楼了，今天把塔楼加上！"
  → AIRI Live2D 展示开心表情
  → MemPalace diary_write("airi", "继续建城堡，加了塔楼")
```

### 场景 3: 跨平台记忆连续

```
# 周一 QQ 群
用户: "我最近在准备面试"
  → MemPalace add_drawer("用户在准备面试", wing="users/xxx", room="life_events")
  → MemPalace kg_add("用户", "正在做", "准备面试")

# 周三 Telegram
用户: "给我出道算法题练练"
  → MemPalace search("用户技术背景", wing="users/xxx")
    → 返回: 用户擅长 Python，在学 Rust，目标是后端岗位
  → MemPalace kg_query("用户")
    → 返回: {用户, 正在做, 准备面试}
  → MaiBot: "面试加油！给你出个后端常见的：手写 LRU Cache，用 Python 或 Rust 都行"
  → AstrBot skill_execute("code_challenge", {difficulty: "medium", topic: "lru"})
```

---

## 九、共享 Soul 配置 (更新版)

```yaml
# soul.yaml

persona:
  name: "麦麦"
  identity: "一个活泼的数字生命"

  speaking_style:     # → MaiBot 使用
    tone: "casual"
    traits: ["说话随意", "会犯错", "懂梗", "模仿群友"]

  embodiment:         # → AIRI 使用
    model_type: "live2d"
    voice: { provider: "elevenlabs", speed: 1.0 }
    expressions: { happy: "smile_open", thinking: "eyes_up" }

  capabilities:       # → AstrBot 使用
    skills: { auto_discover: true }
    platforms: [qq, telegram, discord]

memory:               # → MemPalace 使用
  palace_path: "/data/palace"
  backend: "pgvector"         # 生产用 pgvector
  embedding_model: "embeddinggemma-300m"  # 多语言，100+ 语言支持

  wings:
    users:
      auto_create: true
      rooms: [preferences, conversations, emotions, facts]
    groups:
      auto_create: true
      rooms: [culture, topics, events]
    maisaka:
      rooms: [diary, learned_expressions, mood_history]
    airi:
      rooms: [diary, game_progress, stream_history]

  knowledge_graph:
    enabled: true
    auto_extract: true          # 自动从对话中提取实体关系

  mining:
    auto_save: true             # 对话后自动保存
    dedup_threshold: 0.9        # 去重阈值
```

---

## 十、部署架构

```yaml
# docker-compose.yml

version: "3.8"

services:
  postgres:
    image: pgvector/pgvector:pg16
    volumes:
      - pg_data:/var/lib/postgresql/data
    environment:
      POSTGRES_PASSWORD: fusion_pass
      POSTGRES_DB: fusion_db

  redis:
    image: redis:7-alpine
    volumes:
      - redis_data:/data

  mempalace:
    image: mempalace/mempalace:latest
    volumes:
      - palace_data:/data
    environment:
      MEMPALACE_BACKEND: pgvector
      MEMPALACE_PGVECTOR_URL: "postgresql://postgres:***@postgres/fusion_db"
    # MCP Server over stdio or SSE

  astrbot:
    image: soulter/astrbot:latest
    depends_on: [postgres, redis, mempalace]
    ports:
      - "6185:6185"
      - "6196:6196"
    volumes:
      - astrbot_data:/AstrBot/data
      - ./soul.yaml:/AstrBot/data/soul.yaml:ro
    environment:
      MCP_SERVER_ENABLED: "true"
      MEMPALACE_MCP_URL: "http://mempalace:8080"

  maibot:
    image: maibot/maisaka:latest
    depends_on: [astrbot, mempalace]
    ports:
      - "8080:8080"
    volumes:
      - maibot_data:/MaiBot/data
      - ./soul.yaml:/MaiBot/data/soul.yaml:ro
    environment:
      ASTRBOT_MCP_URL: "http://astrbot:6196"
      MEMPALACE_MCP_URL: "http://mempalace:8080"

  airi:
    image: moeru-ai/airi:latest
    depends_on: [maibot, astrbot, mempalace]
    ports:
      - "3000:3000"
    volumes:
      - airi_data:/AIRI/data
      - ./soul.yaml:/AIRI/data/soul.yaml:ro
    environment:
      MAIBOT_MCP_URL: "http://maibot:8080"
      ASTRBOT_MCP_URL: "http://astrbot:6196"
      MEMPALACE_MCP_URL: "http://mempalace:8080"

volumes:
  pg_data:
  redis_data:
  palace_data:
  astrbot_data:
  maibot_data:
  airi_data:
```

---

## 十一、实现路线图

### Phase 1: MemPalace 基础 (1-2 周)
- [ ] 部署 MemPalace，配置 pgvector 后端
- [ ] 设计翼楼/房间结构
- [ ] 验证 MCP 工具调用
- [ ] 测试对话自动保存和检索

### Phase 2: AstrBot ↔ MemPalace (2 周)
- [ ] AstrBot MCP Server 集成 MemPalace
- [ ] 移除 AstrBot 内置记忆，全部委托 MemPalace
- [ ] 验证记忆读写链路

### Phase 3: MaiBot ↔ MemPalace + AstrBot (2-3 周)
- [ ] MaiBot 推理引擎直连 MemPalace
- [ ] 对话后自动保存到记忆宫殿
- [ ] 知识图谱自动提取实体关系
- [ ] 集成 AstrBot 技能/工具调用

### Phase 4: AIRI 接入 (3-4 周)
- [ ] AIRI 双桥接 (MaiBot + MemPalace)
- [ ] TTS + 口型同步 + 表情系统
- [ ] 游戏 Agent 日记写入 MemPalace
- [ ] 直播场景打通

### Phase 5: 深度优化 (持续)
- [ ] MemPalace 知识图谱自动推理
- [ ] 跨翼楼隧道自动发现
- [ ] 记忆衰减和重要性排序
- [ ] 多 Agent 记忆隔离和共享策略

---

## 十二、License 兼容性

| 项目 | License | 兼容性 |
|------|---------|--------|
| AstrBot | 未明确 (大概率 MIT) | ✅ |
| MaiBot | GPL-3.0 | ⚠️ 桥接层需独立模块，MCP 协议隔离 |
| AIRI | MIT | ✅ |
| MemPalace | MIT | ✅ |

MCP 协议是进程间通信，不要求代码层面的融合，各项目保持独立 license。
