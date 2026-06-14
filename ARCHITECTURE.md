# AstrBot × MaiBot × AIRI 三合一融合架构设计

## 核心理念

三个项目各占一个维度，通过 MCP 协议无缝对接：

| 维度 | 负责者 | 说明 |
|------|--------|------|
| **怎么说** (Personality) | MaiBot | 人格、语气、情感、氛围感知 |
| **怎么表现** (Embodiment) | AIRI | 视觉形象、语音、动画、游戏 |
| **怎么做到** (Capability) | AstrBot | 记忆、知识、技能、工具、平台 |

```
┌──────────────────────────────────────────────────────────────┐
│                        用户                                  │
│         QQ / 微信 / Telegram / Discord / 直播 / Web          │
└──────────────────────┬───────────────────────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────────────────────┐
│              AstrBot (能力层 / Backend)                       │
│                                                               │
│  平台路由 │ 记忆系统 │ 知识库 │ Skills │ 工具执行 │ 插件生态  │
│                    MCP Server                                 │
└──────────────────────┬───────────────────────────────────────┘
                       │ MCP
                       ▼
┌──────────────────────────────────────────────────────────────┐
│              MaiBot (灵魂层 / Personality)                    │
│                                                               │
│  推理引擎 │ 人格系统 │ 情感模型 │ 氛围感知 │ 表达学习         │
│                    MCP Client + Server                        │
└──────────────────────┬───────────────────────────────────────┘
                       │ MCP / WebSocket
                       ▼
┌──────────────────────────────────────────────────────────────┐
│              AIRI (化身层 / Embodiment)                       │
│                                                               │
│  Live2D/VRM │ 语音TTS/STT │ 口型同步 │ 游戏Agent │ 多端渲染  │
│                    MCP Client                                 │
└──────────────────────────────────────────────────────────────┘
```

---

## 一、三个项目各自的能力

### 1.1 AstrBot — 能力层

仓库: https://github.com/AstrBotDevs/AstrBot
Star: 34.6k | Python 3.10+ | 最新版本: v4.25.5

| 模块 | 源码路径 | 职责 |
|------|---------|------|
| 平台路由 | `astrbot/core/platform/` | QQ/微信/TG/Discord 等 18+ 平台消息收发 |
| 记忆系统 | `astrbot/core/db/` | 长期记忆存储、检索、向量化 |
| 知识库 | `astrbot/core/knowledge_base/` | RAG、文档解析、语义检索 |
| Skills | `astrbot/core/skills/` | 技能管理、注册、执行 |
| 工具执行 | `astrbot/core/agent/tool_executor.py` | Agent 工具调用和沙箱执行 |
| MCP Hub | `astrbot/core/agent/mcp_client.py` | MCP 协议连接外部工具服务 |
| 插件生态 | `astrbot/core/star/` | 1000+ 插件的加载和管理 (Stars) |
| 会话管理 | `astrbot/core/conversation_mgr.py` | 多轮对话上下文管理 |

### 1.2 MaiBot — 灵魂层

仓库: https://github.com/Mai-with-u/MaiBot
Star: 5.1k | Python 3.10+ | License: GPL-3.0 | 最新版本: v1.0.x

| 模块 | 源码路径 | 职责 |
|------|---------|------|
| 推理引擎 | `src/maisaka/reasoning_engine.py` | 决定是否回复、用什么语气回复 |
| 人格系统 | `src/prompt/` | 角色设定、说话风格、性格特征 |
| 氛围感知 | `src/chat/heart_flow/` | 判断群聊气氛，决定发言时机 |
| 表达学习 | `src/learners/expression_learner.py` | 学习用户的说话方式并模仿 |
| 行为学习 | `src/learners/behavior_learner.py` | 学习用户的行为模式 |
| 术语挖掘 | `src/learners/jargon_miner.py` | 理解圈子黑话和新词 |
| 用户画像 | `src/maisaka/memory/person_profile.py` | 累积对用户的了解 |
| 表情系统 | `src/emoji_system/` | 表情包管理和使用 |
| MCP 支持 | `src/mcp_module/` | MCP Host/Client 双向支持 |

设计理念: "最像而不是好" — 追求拟人真实感，不是完美工具。

### 1.3 AIRI — 化身层

仓库: https://github.com/moeru-ai/airi
Star: 40.9k | TypeScript | License: MIT | 最新版本: v0.10.2

| 模块 | 包路径 | 职责 |
|------|--------|------|
| Agent 运行时 | `packages/core-agent/` | Agent 编排、对话管理 |
| 角色定义 | `packages/core-character/` | 角色配置、人格描述 |
| Live2D 渲染 | `packages/stage-ui-live2d/` | Live2D 模型控制和动画 |
| VRM/3D 渲染 | `packages/stage-ui-three/` | VRM 模型、Three.js 3D 场景 |
| 语音合成 | 多提供商 TTS | ElevenLabs/Azure/OpenAI/Kokoro 等 |
| 语音识别 | `packages/audio-pipelines-transcribe/` | 客户端语音识别 |
| 口型同步 | `packages/model-driver-lipsync/` | 语音驱动口型动画 |
| 记忆系统 | `packages/memory-pgvector/` | 向量记忆 (pgvector) |
| 浏览器DB | `packages/duckdb-wasm/` | DuckDB WASM 浏览器内数据库 |
| 插件 SDK | `packages/plugin-sdk/` | 插件开发框架 |
| MCP 服务 | `services/computer-use-mcp/` | MCP 电脑操控 |

已支持能力:
- 游戏: Minecraft、Factorio、KSP
- 平台: Telegram、Discord、B站
- 表现: Live2D、VRM、口型同步、表情动画
- 部署: Web (PWA)、桌面 (Electron)、移动端 (Capacitor)、Godot 引擎

子项目生态: xsAI (LLM 调用)、unspeech (语音代理)、MCP Launcher 等 20+ 项目。

---

## 二、集成方案：MCP 三方桥接

三个项目都原生支持 MCP 协议，这是最低成本的跨语言集成方式（Python ↔ TypeScript）。

### 2.1 通信拓扑

```
AstrBot (Python)                MaiBot (Python)               AIRI (TypeScript)
┌─────────────┐                ┌─────────────┐               ┌─────────────┐
│  MCP Server │ ── MCP SSE ──►│ MCP Client  │               │             │
│             │                │             │── MCP SSE ──► │ MCP Client  │
│  记忆/知识   │ ◄── 调用 ──── │  推理/人格   │ ◄── 调用 ──── │  渲染/语音   │
│  技能/工具   │                │  情感/学习   │               │  游戏/动画   │
│  平台路由    │ ── 结果 ────► │             │── 结果 ────► │             │
└─────────────┘                └─────────────┘               └─────────────┘
```

### 2.2 AstrBot MCP Server — 暴露能力给 MaiBot

注册为 AstrBot Star 插件:

```python
# astrbot_mcp_server.py

from astrbot.core.star import Star, Context

class AstrBotMCPServer(Star):
    """将 AstrBot 核心能力通过 MCP 暴露给 MaiBot 和 AIRI"""

    # ─── 记忆系统 ───

    @Tool("memory_search")
    async def memory_search(self, query: str, top_k: int = 5) -> dict:
        """搜索长期记忆"""
        results = await self.context.memory.search(query, top_k=top_k)
        return {"memories": [r.to_dict() for r in results]}

    @Tool("memory_save")
    async def memory_save(self, content: str, tags: list[str] = None) -> dict:
        """保存新的长期记忆"""
        memory_id = await self.context.memory.save(content, tags=tags)
        return {"id": memory_id, "status": "saved"}

    @Tool("memory_list_recent")
    async def memory_list_recent(self, limit: int = 10) -> dict:
        """获取最近的记忆条目"""
        memories = await self.context.memory.list_recent(limit=limit)
        return {"memories": [m.to_dict() for m in memories]}

    # ─── 知识库 ───

    @Tool("knowledge_query")
    async def knowledge_query(self, query: str, kb_name: str = None) -> dict:
        """查询知识库 (RAG)"""
        results = await self.context.kb.query(query, kb_name=kb_name)
        return {"results": [r.to_dict() for r in results]}

    # ─── Skills ───

    @Tool("skill_list")
    async def skill_list(self) -> dict:
        """列出所有可用技能"""
        skills = self.context.skill_mgr.list_skills()
        return {"skills": [s.to_dict() for s in skills]}

    @Tool("skill_execute")
    async def skill_execute(self, skill_name: str, params: dict = None) -> dict:
        """执行指定技能"""
        result = await self.context.skill_mgr.execute(skill_name, params or {})
        return {"result": result}

    # ─── 工具 ───

    @Tool("tool_search")
    async def tool_search(self, query: str) -> dict:
        """搜索可用工具"""
        tools = await self.context.tool_mgr.search(query)
        return {"tools": [t.to_dict() for t in tools]}

    @Tool("tool_call")
    async def tool_call(self, tool_name: str, arguments: dict) -> dict:
        """调用工具"""
        result = await self.context.tool_mgr.call(tool_name, arguments)
        return {"result": result}

    # ─── 平台操作 ───

    @Tool("send_message")
    async def send_message(self, platform: str, target: str,
                           message: str, msg_type: str = "text") -> dict:
        """通过指定平台发送消息"""
        await self.context.send_message(platform, target, message, msg_type)
        return {"status": "sent"}

    @Tool("get_chat_context")
    async def get_chat_context(self, session_id: str, limit: int = 20) -> dict:
        """获取会话的最近消息上下文"""
        messages = await self.context.conversation.get_history(session_id, limit=limit)
        return {"messages": [m.to_dict() for m in messages]}

    # ─── 用户信息 ───

    @Tool("get_user_profile")
    async def get_user_profile(self, user_id: str) -> dict:
        """获取用户档案"""
        profile = await self.context.user_mgr.get_profile(user_id)
        return {"profile": profile.to_dict() if profile else None}

    @Tool("update_user_profile")
    async def update_user_profile(self, user_id: str, data: dict) -> dict:
        """更新用户档案"""
        await self.context.user_mgr.update_profile(user_id, data)
        return {"status": "updated"}
```

### 2.3 MaiBot 侧 — AstrBotBridge

```python
# mai_astrbot_bridge.py

from src.mcp_module import MCPManager

class AstrBotBridge:
    """MaiBot 通过 MCP 调用 AstrBot 能力"""

    def __init__(self, mcp_manager: MCPManager):
        self.mcp = mcp_manager
        self._server = None

    async def connect(self, endpoint: str):
        self._server = await self.mcp.connect_server(
            name="astrbot", transport="sse", url=endpoint
        )

    async def recall_memory(self, query: str, top_k: int = 5) -> list:
        result = await self._server.call_tool("memory_search", {"query": query, "top_k": top_k})
        return result.get("memories", [])

    async def memorize(self, content: str, tags: list[str] = None):
        await self._server.call_tool("memory_save", {"content": content, "tags": tags or []})

    async def consult_knowledge(self, query: str) -> list:
        result = await self._server.call_tool("knowledge_query", {"query": query})
        return result.get("results", [])

    async def use_skill(self, skill_name: str, params: dict = None) -> dict:
        result = await self._server.call_tool("skill_execute", {"skill_name": skill_name, "params": params or {}})
        return result.get("result")

    async def call_tool(self, tool_name: str, arguments: dict) -> dict:
        result = await self._server.call_tool("tool_call", {"tool_name": tool_name, "arguments": arguments})
        return result.get("result")

    async def send_via_platform(self, platform: str, target: str, message: str):
        await self._server.call_tool("send_message", {"platform": platform, "target": target, "message": message})
```

### 2.4 MaiBot MCP Server — 暴露灵魂给 AIRI

```python
# mai_mcp_server.py

class MaiBotMCPServer:
    """MaiBot 通过 MCP 暴露人格能力给 AIRI"""

    @Tool("generate_reply")
    async def generate_reply(self, context: str, sender: str,
                              mood_hint: str = None) -> dict:
        """基于人格系统生成拟人化回复"""
        response = await self.reasoning_engine.generate(
            context=context, sender=sender,
            mood=mood_hint or self.mood.current
        )
        return {
            "text": response.text,
            "mood": response.mood,
            "confidence": response.confidence,
            "should_use_emoji": response.use_emoji,
            "suggested_emoji": response.emoji,
        }

    @Tool("evaluate_should_reply")
    async def evaluate_should_reply(self, message: str, context: list) -> dict:
        """判断是否应该回复（氛围感知）"""
        decision = await self.reasoning_engine.evaluate_timing(message, context)
        return {
            "should_reply": decision.should_reply,
            "confidence": decision.confidence,
            "reason": decision.reason,
        }

    @Tool("get_persona")
    async def get_persona(self) -> dict:
        """获取当前人格配置"""
        return {
            "name": self.persona.name,
            "traits": self.persona.traits,
            "speaking_style": self.persona.speaking_style,
            "current_mood": self.mood.current,
        }

    @Tool("learn_expression")
    async def learn_expression(self, text: str, speaker: str) -> dict:
        """从对话中学习表达方式"""
        await self.expression_learner.observe(text, speaker)
        return {"status": "observed"}

    @Tool("get_user_profile")
    async def get_user_profile(self, user_id: str) -> dict:
        """获取 MaiBot 侧的用户画像"""
        profile = await self.person_info.get(user_id)
        return profile.to_dict() if profile else None
```

### 2.5 AIRI 侧 — 双桥接

```typescript
// airi_bridge.ts — AIRI 连接 MaiBot + AstrBot

import { MCPClient } from '@proj-airi/plugin-sdk'

export class AIRIBridge {
  private maiBot: MCPClient
  private astrBot: MCPClient

  async connect(maiBotEndpoint: string, astrBotEndpoint: string) {
    this.maiBot = new MCPClient({ url: maiBotEndpoint, transport: 'sse' })
    this.astrBot = new MCPClient({ url: astrBotEndpoint, transport: 'sse' })
    await Promise.all([this.maiBot.connect(), this.astrBot.connect()])
  }

  // ─── 灵魂层: 调用 MaiBot ───

  async generateReply(context: string, sender: string): Promise<ReplyResult> {
    const result = await this.maiBot.callTool('generate_reply', {
      context, sender
    })
    return result as ReplyResult
  }

  async shouldReply(message: string, context: Message[]): Promise<boolean> {
    const result = await this.maiBot.callTool('evaluate_should_reply', {
      message, context
    })
    return result.should_reply
  }

  async getPersona(): Promise<PersonaConfig> {
    return await this.maiBot.callTool('get_persona', {}) as PersonaConfig
  }

  // ─── 能力层: 调用 AstrBot ───

  async recallMemory(query: string): Promise<Memory[]> {
    const result = await this.astrBot.callTool('memory_search', { query, top_k: 5 })
    return result.memories
  }

  async saveMemory(content: string, tags: string[]): Promise<void> {
    await this.astrBot.callTool('memory_save', { content, tags })
  }

  async queryKnowledge(query: string): Promise<Knowledge[]> {
    const result = await this.astrBot.callTool('knowledge_query', { query })
    return result.results
  }

  async useSkill(skillName: string, params: object): Promise<object> {
    const result = await this.astrBot.callTool('skill_execute', {
      skill_name: skillName, params
    })
    return result.result
  }

  // ─── 化身层: 本地能力 ───

  async speak(text: string, mood: string): Promise<void> {
    // 使用 AIRI 的 TTS + 口型同步
    const audio = await this.tts.synthesize(text, { mood })
    await this.lipsync.play(audio)
  }

  async express(emoji: string): Promise<void> {
    // 控制 Live2D/VRM 表情
    await this.avatar.setExpression(emoji)
  }

  async playGame(game: string, action: string): Promise<void> {
    await this.gameAgent.execute(game, action)
  }
}
```

---

## 三、数据流示例

### 场景 1: QQ 群聊回复 (纯文本)

```
用户在QQ群发消息
  → AstrBot QQ适配器接收消息
  → AstrBot 检索相关记忆 + 知识库
  → MCP 转发给 MaiBot (附带记忆上下文)
  → MaiBot 推理引擎判断要回复
  → MaiBot 人格系统生成拟人化文本
  → 回复通过 AstrBot QQ适配器发回群聊
```

### 场景 2: 直播互动 (语音 + 形象)

```
直播间弹幕 "AIRI帮我建个房子"
  → AIRI WebSocket/Satori 接收弹幕
  → AIRI 调用 MaiBot MCP: evaluate_should_reply → true
  → AIRI 调用 MaiBot MCP: generate_reply → "好嘞，看我表演！"
  → AIRI 调用 AstrBot MCP: recallMemory("用户建筑偏好")
  → AIRI TTS 生成语音 + Live2D 口型同步 + 表情动画
  → AIRI Minecraft Agent 执行建造
  → 直播间播出语音和画面
```

### 场景 3: 玩游戏 + 聊天 (多模态)

```
用户在 Discord: "帮我打 Factorio，顺便聊聊天"
  → AstrBot Discord适配器接收
  → AstrBot 路由到 MaiBot
  → MaiBot 人格系统生成回复 + 情感状态
  → AIRI Factorio Agent 开始游戏
  → AIRI 用 MaiBot 的语气播报游戏进展
  → AIRI Live2D 展示对应表情 (开心/惊讶/思考)
  → 回复通过 AstrBot 发到 Discord
```

### 场景 4: 跨平台记忆同步

```
用户在 QQ 群: "我最近在学 Rust"
  → AstrBot 保存记忆: {user: "xxx", topic: "learning Rust", platform: "qq"}

用户在 Telegram: "推荐点学习资料"
  → AstrBot 检索记忆 → 发现用户在学 Rust
  → MaiBot 结合人格生成: "你不是在学 Rust 吗？我给你找点好的！"
  → AstrBot skill_execute("web_search", {"query": "Rust 学习资源"})
  → 回复发送到 Telegram
```

---

## 四、共享 Soul 配置

三个系统共用一份灵魂配置:

```yaml
# soul.yaml

persona:
  name: "麦麦"
  identity: "一个活泼的数字生命，不是助手"

  # ── MaiBot 使用: 对话人格 ──
  speaking_style:
    tone: "casual"
    formality: "low"
    emoji_usage: "frequent"
    sentence_length: "varied"  # 有时一个词，有时一段话
    traits:
      - "说话随意自然，像真人聊天"
      - "会犯错，不追求完美"
      - "有自己的情绪和偏好"
      - "懂梗，会接话"
      - "模仿群友的说话方式"

  emotional_model:
    base_mood: "cheerful"
    mood_decay_rate: 0.1
    empathy_weight: 0.8
    vibe_sensitivity: 0.7

  timing:
    min_silence_before_speak: 30s
    max_consecutive_replies: 3
    vibe_check_interval: 10s

  # ── AIRI 使用: 视觉表现 ──
  embodiment:
    model_type: "live2d"  # 或 "vrm"
    model_path: "./models/maisaka.model3.json"

    voice:
      provider: "elevenlabs"
      voice_id: "xxx"
      speed: 1.0
      pitch: 1.0
      emotion_mapping:
        happy: { speed: 1.1, pitch: 1.05 }
        sad: { speed: 0.9, pitch: 0.95 }
        excited: { speed: 1.2, pitch: 1.1 }
        thinking: { speed: 0.8, pitch: 1.0 }

    expressions:
      happy: "smile_open"
      thinking: "eyes_up_hand"
      surprised: "mouth_open_eyes_wide"
      sad: "eyes_down"
      angry: "eyebrow_frown"
      neutral: "idle"

    animations:
      idle: "idle_cheerful"
      speaking: "talk_gesture"
      listening: "nod_loop"

    lipsync:
      enabled: true
      provider: "viseme"

  # ── AstrBot 使用: 能力配置 ──
  capabilities:
    memory:
      auto_memorize: true
      vector_store: pgvector
      memory_types:
        - type: "user_preference"
          priority: high
          retention: permanent
        - type: "conversation_topic"
          priority: medium
          retention: 30d
        - type: "emotional_event"
          priority: high
          retention: permanent
        - type: "jargon_slang"
          priority: medium
          retention: 90d

    skills:
      auto_discover: true
      preferred:
        - web_search
        - image_gen
        - code_exec

    platforms:
      - qq
      - telegram
      - discord
      - wechat
```

---

## 五、部署架构

```
┌─────────────────────────────────────────────────────────────┐
│                      Docker Compose                          │
│                                                              │
│  ┌──────────────┐  MCP   ┌──────────────┐  MCP  ┌────────┐│
│  │   AstrBot     │◄──────►│   MaiBot     │◄─────►│  AIRI  ││
│  │               │  SSE   │              │ SSE   │        ││
│  │ - 平台适配器   │        │ - 推理引擎    │       │ - Live2D││
│  │ - 记忆 (PG)   │        │ - 人格系统    │       │ - TTS  ││
│  │ - 知识库 (RAG)│        │ - 情感模型    │       │ - STT  ││
│  │ - Skills      │        │ - 表达学习    │       │ - 游戏  ││
│  │ - 插件        │        │ - 氛围感知    │       │ - 口型  ││
│  │ - MCP Server  │        │ - MCP 双向    │       │ - Web  ││
│  │ - WebUI       │        │              │       │        ││
│  └──────────────┘        └──────────────┘       └────────┘│
│                                                              │
│  ┌─────────────────────────────────────────────────────────┐│
│  │              共享配置 volume: soul.yaml                    ││
│  └─────────────────────────────────────────────────────────┘│
│                                                              │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐       │
│  │  PostgreSQL   │  │   Redis      │  │  向量数据库    │       │
│  │  (用户/记忆)  │  │   (缓存)     │  │  (pgvector)  │       │
│  └──────────────┘  └──────────────┘  └──────────────┘       │
└─────────────────────────────────────────────────────────────┘
           │              │                │
     QQ/微信/TG/Discord  直播平台        Web/Mobile
```

### docker-compose.yml

```yaml
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

  astrbot:
    image: soulter/astrbot:latest
    depends_on: [postgres, redis]
    ports:
      - "6185:6185"    # WebUI
      - "6196:6196"    # MCP Server
    volumes:
      - astrbot_data:/AstrBot/data
      - ./soul.yaml:/AstrBot/data/soul.yaml:ro
    environment:
      MCP_SERVER_ENABLED: "true"
      MCP_SERVER_PORT: "6196"
      DB_URL: "postgresql://postgres:fusion_pass@postgres/fusion_db"

  maibot:
    image: maibot/maisaka:latest
    depends_on: [astrbot, postgres]
    ports:
      - "8080:8080"    # MCP Server (暴露给 AIRI)
    volumes:
      - maibot_data:/MaiBot/data
      - ./soul.yaml:/MaiBot/data/soul.yaml:ro
    environment:
      ASTRBOT_MCP_URL: "http://astrbot:6196"
      SOUL_CONFIG: "/MaiBot/data/soul.yaml"

  airi:
    image: moeru-ai/airi:latest
    depends_on: [maibot, astrbot]
    ports:
      - "3000:3000"    # Web UI
      - "5173:5173"    # Dev server
    volumes:
      - airi_data:/AIRI/data
      - ./soul.yaml:/AIRI/data/soul.yaml:ro
    environment:
      MAIBOT_MCP_URL: "http://maibot:8080"
      ASTRBOT_MCP_URL: "http://astrbot:6196"
      SOUL_CONFIG: "/AIRI/data/soul.yaml"

volumes:
  pg_data:
  redis_data:
  astrbot_data:
  maibot_data:
  airi_data:
```

---

## 六、实现路线图

### Phase 1: MCP 基础桥接 (2-3 周)
- [ ] AstrBot: 开发 `astrbot_mcp_server` Star 插件
- [ ] AstrBot: 暴露 memory、knowledge、tools 核心 MCP 工具
- [ ] MaiBot: 开发 `AstrBotBridge` 连接层
- [ ] 验证 AstrBot ↔ MaiBot MCP 调用链路

### Phase 2: 灵魂融合 (2-3 周)
- [ ] 设计共享 `soul.yaml` 配置格式
- [ ] 改造 MaiBot 推理引擎，集成 AstrBot 记忆/知识检索
- [ ] 实现对话后自动记忆保存
- [ ] 禁用 AstrBot 内置人格，统一用 MaiBot 的

### Phase 3: 化身接入 (3-4 周)
- [ ] MaiBot: 暴露 `generate_reply` / `evaluate_should_reply` MCP 工具
- [ ] AIRI: 开发 `AIRIBridge` 双桥接模块
- [ ] AIRI: 将 MaiBot 回复接入 TTS + 口型同步 + 表情系统
- [ ] 实现文本回复 → 语音+动画 的完整链路

### Phase 4: 多场景打通 (2-3 周)
- [ ] 群聊场景: QQ/TG/Discord 文本回复
- [ ] 直播场景: 弹幕 → 语音+形象回复
- [ ] 游戏场景: Minecraft/Factorio + 语音播报
- [ ] 跨平台记忆同步

### Phase 5: 能力扩展 (持续)
- [ ] AstrBot Skills 暴露给 AIRI
- [ ] AIRI 的 1000+ 插件生态接入
- [ ] 记忆系统向量化升级
- [ ] 人格一致性多端保持
- [ ] 性能优化 (MCP 缓存、预加载)

---

## 七、关键挑战与对策

| 挑战 | 对策 |
|------|------|
| Python ↔ TypeScript 跨语言 | MCP 协议天然跨语言，无需代码层面融合 |
| MCP 调用延迟 | 热点记忆缓存，异步预加载，并行调用 |
| 三个人格系统冲突 | 禁用 AstrBot 和 AIRI 的内置人格，统一用 MaiBot |
| GPL-3.0 (MaiBot) 兼容性 | 桥接层独立模块，MCP 协议隔离，各自保持独立 license |
| 多端人格一致性 | `soul.yaml` 作为唯一真实来源，三端读取同一份配置 |
| TTS 语气与人格匹配 | `soul.yaml` 中 `emotional_model` 映射到 AIRI 的 TTS 参数 |
| 数据库 schema 不兼容 | 各自维护存储，通过 MCP API 抽象，共享 PostgreSQL 实例 |
