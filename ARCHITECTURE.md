# AstrBot × MaiBot 融合架构设计

## 核心理念

```
┌─────────────────────────────────────────────────────┐
│                    用户 (多平台)                       │
│         QQ / 微信 / Telegram / Discord / ...          │
└──────────────────────┬──────────────────────────────┘
                       │ 消息
                       ▼
┌─────────────────────────────────────────────────────┐
│              AstrBot (身体 / 能力层)                    │
│                                                       │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌─────────┐ │
│  │ 平台路由  │ │ 记忆系统  │ │ Skills   │ │ 工具执行 │ │
│  │ Adapter  │ │ Memory   │ │ Manager  │ │ Tools   │ │
│  └──────────┘ └──────────┘ └──────────┘ └─────────┘ │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌─────────┐ │
│  │ 知识库    │ │ 插件市场  │ │ MCP Hub  │ │ 沙箱    │ │
│  │ KB/RAG   │ │ Stars    │ │ Client   │ │ Sandbox │ │
│  └──────────┘ └──────────┘ └──────────┘ └─────────┘ │
│                     │ MCP / API                       │
└─────────────────────┼────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────┐
│              MaiBot (灵魂 / 人格层)                    │
│                                                       │
│  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐ │
│  │ 推理引擎      │ │ 人格系统      │ │ 情感/氛围    │ │
│  │ Reasoning    │ │ Persona      │ │ Mood/Vibe    │ │
│  └──────────────┘ └──────────────┘ └──────────────┘ │
│  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐ │
│  │ 发言时机      │ │ 表达学习      │ │ 用户理解     │ │
│  │ Timing       │ │ Expression   │ │ PersonInfo   │ │
│  └──────────────┘ └──────────────┘ └──────────────┘ │
│  ┌──────────────┐ ┌──────────────┐                   │
│  │ 黑话/术语    │ │ 行为模式      │                   │
│  │ Jargon       │ │ Behavior     │                   │
│  └──────────────┘ └──────────────┘                   │
└─────────────────────────────────────────────────────┘
```

**一句话**: MaiBot 决定"说什么、怎么说、何时说"，AstrBot 负责"怎么做到"。

---

## 职责划分

### MaiBot 持有的 (灵魂层)

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

### AstrBot 提供的 (能力层)

| 模块 | 源码路径 | 职责 |
|------|---------|------|
| 平台路由 | `astrbot/core/platform/` | QQ/微信/TG/Discord 等 18+ 平台消息收发 |
| 记忆系统 | `astrbot/core/db/` | 长期记忆存储、检索、向量化 |
| 知识库 | `astrbot/core/knowledge_base/` | RAG、文档解析、语义检索 |
| Skills | `astrbot/core/skills/` | 技能管理、注册、执行 |
| 工具执行 | `astrbot/core/agent/tool_executor.py` | Agent 工具调用和沙箱执行 |
| MCP Hub | `astrbot/core/agent/mcp_client.py` | MCP 协议连接外部工具服务 |
| 插件生态 | `astrbot/core/star/` | 1000+ 插件的加载和管理 |
| 会话管理 | `astrbot/core/conversation_mgr.py` | 多轮对话上下文管理 |

---

## 集成方案：MCP 双向桥接

两个项目都原生支持 MCP 协议，这是最自然的集成方式。

### 数据流

```
用户消息 → AstrBot 平台适配器
         → AstrBot 会话管理 (记录原始消息)
         → AstrBot 记忆系统 (检索相关记忆)
         → 通过 MCP 转发给 MaiBot
         → MaiBot 推理引擎 (决定是否回复)
         → MaiBot 人格系统 (生成回复内容和风格)
         → MaiBot 调用 AstrBot MCP 工具 (需要时)
             ├── memory_search: 搜索长期记忆
             ├── memory_save: 保存新记忆
             ├── skill_execute: 执行技能
             ├── knowledge_query: 查询知识库
             ├── tool_call: 调用其他工具
             └── platform_send: 发送消息
         → 回复通过 AstrBot 发送到对应平台
```

### MCP Server：AstrBot 能力暴露

AstrBot 作为 MCP Server，向 MaiBot 暴露以下工具：

```python
# astrbot_mcp_server.py — 注册为 AstrBot Star 插件

from astrbot.core.star import Star, Context

class AstrBotMCPServer(Star):
    """将 AstrBot 的核心能力通过 MCP 暴露给 MaiBot"""

    async def on_load(self):
        # 注册 MCP Server 工具
        pass

    # ─── 记忆系统 ───

    @Tool("memory_search")
    async def memory_search(self, query: str, top_k: int = 5) -> dict:
        """搜索长期记忆。返回与 query 最相关的记忆片段。"""
        results = await self.context.memory.search(query, top_k=top_k)
        return {"memories": [r.to_dict() for r in results]}

    @Tool("memory_save")
    async def memory_save(self, content: str, tags: list[str] = None) -> dict:
        """保存一条新的长期记忆。"""
        memory_id = await self.context.memory.save(content, tags=tags)
        return {"id": memory_id, "status": "saved"}

    @Tool("memory_list_recent")
    async def memory_list_recent(self, limit: int = 10) -> dict:
        """获取最近的记忆条目。"""
        memories = await self.context.memory.list_recent(limit=limit)
        return {"memories": [m.to_dict() for m in memories]}

    # ─── 知识库 ───

    @Tool("knowledge_query")
    async def knowledge_query(self, query: str, kb_name: str = None) -> dict:
        """查询知识库，返回相关文档片段 (RAG)。"""
        results = await self.context.kb.query(query, kb_name=kb_name)
        return {"results": [r.to_dict() for r in results]}

    # ─── Skills ───

    @Tool("skill_list")
    async def skill_list(self) -> dict:
        """列出所有可用技能。"""
        skills = self.context.skill_mgr.list_skills()
        return {"skills": [s.to_dict() for s in skills]}

    @Tool("skill_execute")
    async def skill_execute(self, skill_name: str, params: dict = None) -> dict:
        """执行指定技能。"""
        result = await self.context.skill_mgr.execute(skill_name, params or {})
        return {"result": result}

    # ─── 工具调用 ───

    @Tool("tool_search")
    async def tool_search(self, query: str) -> dict:
        """搜索可用工具。"""
        tools = await self.context.tool_mgr.search(query)
        return {"tools": [t.to_dict() for t in tools]}

    @Tool("tool_call")
    async def tool_call(self, tool_name: str, arguments: dict) -> dict:
        """调用指定工具并返回结果。"""
        result = await self.context.tool_mgr.call(tool_name, arguments)
        return {"result": result}

    # ─── 平台操作 ───

    @Tool("send_message")
    async def send_message(self, platform: str, target: str,
                           message: str, msg_type: str = "text") -> dict:
        """通过指定平台发送消息。"""
        await self.context.send_message(platform, target, message, msg_type)
        return {"status": "sent"}

    @Tool("get_chat_context")
    async def get_chat_context(self, session_id: str,
                               limit: int = 20) -> dict:
        """获取指定会话的最近消息上下文。"""
        messages = await self.context.conversation.get_history(
            session_id, limit=limit
        )
        return {"messages": [m.to_dict() for m in messages]}

    # ─── 用户信息 ───

    @Tool("get_user_profile")
    async def get_user_profile(self, user_id: str) -> dict:
        """获取用户档案信息。"""
        profile = await self.context.user_mgr.get_profile(user_id)
        return {"profile": profile.to_dict() if profile else None}

    @Tool("update_user_profile")
    async def update_user_profile(self, user_id: str,
                                   data: dict) -> dict:
        """更新用户档案。"""
        await self.context.user_mgr.update_profile(user_id, data)
        return {"status": "updated"}
```

### MCP Client：MaiBot 调用 AstrBot

MaiBot 侧连接 AstrBot 的 MCP Server：

```python
# mai_astrbot_bridge.py — MaiBot 侧的 AstrBot 桥接

from src.mcp_module import MCPManager

class AstrBotBridge:
    """MaiBot 通过 MCP 调用 AstrBot 能力的桥接层"""

    def __init__(self, mcp_manager: MCPManager):
        self.mcp = mcp_manager
        self._astrbot_server = None

    async def connect(self, astrbot_mcp_endpoint: str):
        """连接到 AstrBot MCP Server"""
        self._astrbot_server = await self.mcp.connect_server(
            name="astrbot",
            transport="sse",  # 或 stdio
            url=astrbot_mcp_endpoint,
        )

    # ─── 封装为 MaiBot 内部可直接调用的高层接口 ───

    async def recall_memory(self, query: str, top_k: int = 5) -> list:
        """回忆：搜索长期记忆"""
        result = await self._astrbot_server.call_tool(
            "memory_search", {"query": query, "top_k": top_k}
        )
        return result.get("memories", [])

    async def memorize(self, content: str, tags: list[str] = None):
        """记忆：保存新的长期记忆"""
        await self._astrbot_server.call_tool(
            "memory_save", {"content": content, "tags": tags or []}
        )

    async def consult_knowledge(self, query: str) -> list:
        """查阅：查询知识库"""
        result = await self._astrbot_server.call_tool(
            "knowledge_query", {"query": query}
        )
        return result.get("results", [])

    async def use_skill(self, skill_name: str, params: dict = None) -> dict:
        """使用技能"""
        result = await self._astrbot_server.call_tool(
            "skill_execute", {"skill_name": skill_name, "params": params or {}}
        )
        return result.get("result")

    async def call_tool(self, tool_name: str, arguments: dict) -> dict:
        """调用底层工具"""
        result = await self._astrbot_server.call_tool(
            "tool_call", {"tool_name": tool_name, "arguments": arguments}
        )
        return result.get("result")

    async def speak(self, platform: str, target: str, message: str):
        """通过 AstrBot 平台发送消息"""
        await self._astrbot_server.call_tool(
            "send_message",
            {"platform": platform, "target": target, "message": message}
        )
```

---

## 推理引擎改造

MaiBot 的 `reasoning_engine.py` 需要改造，将记忆/工具调用路由到 AstrBot：

```python
# 改造后的推理流程 (伪代码)

class FusionReasoningEngine(MaisakaReasoningEngine):

    def __init__(self, astrbot_bridge: AstrBotBridge, **kwargs):
        super().__init__(**kwargs)
        self.astrbot = astrbot_bridge

    async def reason(self, message: SessionMessage) -> ChatResponse:

        # 1. 【MaiBot 灵魂】判断是否要回复
        should_reply = self._evaluate_should_reply(message)
        if not should_reply:
            return ChatResponse.no_reply()

        # 2. 【AstrBot 能力】检索相关记忆
        memories = await self.astrbot.recall_memory(
            query=message.text, top_k=5
        )

        # 3. 【AstrBot 能力】查询知识库 (如果需要)
        knowledge = []
        if self._needs_knowledge(message):
            knowledge = await self.astrbot.consult_knowledge(message.text)

        # 4. 【MaiBot 灵魂】构建人格化的 prompt
        prompt = self._build_persona_prompt(
            message=message,
            memories=memories,      # AstrBot 提供的记忆
            knowledge=knowledge,     # AstrBot 提供的知识
            chat_context=message.context,
        )

        # 5. 【MaiBot 灵魂】生成回复 (带人格、语气、表情)
        response = await self._generate_with_persona(prompt)

        # 6. 【AstrBot 能力】如果回复需要执行技能/工具
        if response.needs_tool:
            tool_result = await self.astrbot.call_tool(
                response.tool_name, response.tool_args
            )
            response = await self._integrate_tool_result(response, tool_result)

        # 7. 【MaiBot 灵魂】后处理 (加表情、调整语气)
        response = self._apply_mood_and_emoji(response)

        # 8. 【AstrBot 能力】保存对话记忆
        await self.astrbot.memorize(
            content=f"用户说: {message.text}\n我回复了: {response.text}",
            tags=["conversation", message.session_id]
        )

        return response
```

---

## 共享的 Soul 配置

两个系统共用同一份人格配置文件：

```yaml
# soul.yaml — MaiBot 和 AstrBot 共享的"灵魂"配置

persona:
  name: "麦麦"
  identity: "一个活泼的群聊伙伴，不是助手"
  traits:
    - "说话随意自然，像真人聊天"
    - "会犯错，不追求完美"
    - "有自己的情绪和偏好"
    - "懂梗，会接话"
  speaking_style:
    - "不用 markdown 格式"
    - "回复长短不一，有时一个词，有时一段话"
    - "会用表情包和颜文字"
    - "模仿群友的说话方式"

emotional_model:
  base_mood: "cheerful"
  mood_decay_rate: 0.1
  empathy_weight: 0.8
  vibe_sensitivity: 0.7

memory_policy:
  auto_memorize: true
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

timing:
  min_silence_before_speak: 30s    # 群聊安静多久后才主动说话
  max_consecutive_replies: 3       # 最多连续回复几条
  vibe_check_interval: 10s         # 氛围检测间隔
  boredom_threshold: 300s          # 无聊阈值

skills_from_astrbot:
  enabled: true
  auto_discover: true              # 自动发现 AstrBot 新安装的技能
  preferred:
    - web_search                   # 搜索
    - image_gen                    # 生图
    - code_exec                    # 代码执行
```

---

## 部署架构

```
┌─────────────────────────────────────────────┐
│                 Docker Compose               │
│                                              │
│  ┌────────────────────┐  MCP   ┌──────────┐│
│  │     AstrBot         │◄──────►│ MaiBot   ││
│  │                     │  SSE   │          ││
│  │  - 平台适配器        │        │ - 人格   ││
│  │  - 记忆 (SQLite/PG) │        │ - 推理   ││
│  │  - 知识库 (向量DB)   │        │ - 学习   ││
│  │  - Skills           │        │ - 氛围   ││
│  │  - 插件             │        │ - 表情   ││
│  │  - MCP Server       │        │          ││
│  │  - WebUI            │        │          ││
│  └────────┬───────────┘        └──────────┘│
│           │                                 │
│  ┌────────┴────────────────────────────────┐│
│  │         共享配置 volume                   ││
│  │         soul.yaml                        ││
│  └─────────────────────────────────────────┘│
└─────────────────────────────────────────────┘
           │
           ▼
    QQ / 微信 / TG / Discord
```

### docker-compose.yml

```yaml
version: "3.8"

services:
  astrbot:
    image: soulter/astrbot:latest
    ports:
      - "6185:6185"      # WebUI
      - "6196:6196"      # MCP Server
    volumes:
      - astrbot_data:/AstrBot/data
      - ./soul.yaml:/AstrBot/data/soul.yaml:ro
    environment:
      - MCP_SERVER_ENABLED=true
      - MCP_SERVER_PORT=6196

  maibot:
    image: maibot/maisaka:latest
    depends_on:
      - astrbot
    volumes:
      - maibot_data:/MaiBot/data
      - ./soul.yaml:/MaiBot/data/soul.yaml:ro
    environment:
      - ASTRBOT_MCP_URL=http://astrbot:6196
      - SOUL_CONFIG=/MaiBot/data/soul.yaml

volumes:
  astrbot_data:
  maibot_data:
```

---

## 实现路线图

### Phase 1: MCP 桥接基础 (1-2 周)
- [ ] 在 AstrBot 中开发 `astrbot_mcp_server` Star 插件
- [ ] 暴露 memory、knowledge、tools 三个核心 MCP 工具
- [ ] 在 MaiBot 中开发 `AstrBotBridge` 连接层
- [ ] 验证基本的 MCP 调用链路

### Phase 2: 灵魂融合 (2-3 周)
- [ ] 设计共享的 `soul.yaml` 配置格式
- [ ] 改造 MaiBot 推理引擎，集成 AstrBot 的记忆/知识检索
- [ ] 实现对话后的自动记忆保存
- [ ] 保持 MaiBot 的人格 prompt 和情感系统不变

### Phase 3: 能力扩展 (2-3 周)
- [ ] 将 AstrBot 的 Skills 系统暴露给 MaiBot
- [ ] MaiBot 可以按需调用 AstrBot 的 1000+ 插件
- [ ] 实现技能的自动发现和推荐
- [ ] 知识库 RAG 增强对话质量

### Phase 4: 多平台上线 (1-2 周)
- [ ] 通过 AstrBot 将 MaiBot 的人格扩展到所有支持的平台
- [ ] 各平台的消息统一路由到 MaiBot 处理
- [ ] WebUI 统一管理

### Phase 5: 深度优化 (持续)
- [ ] 记忆系统的向量化升级
- [ ] 人格一致性在多平台间的保持
- [ ] 性能优化 (MCP 调用缓存)
- [ ] 用户画像的跨平台同步

---

## 关键挑战与对策

| 挑战 | 对策 |
|------|------|
| MCP 调用延迟影响对话自然度 | 缓存热点记忆，异步预加载 |
| MaiBot GPL-3.0 与 AstrBot License 兼容性 | 桥接层独立模块，接口清晰隔离 |
| 两个人格系统冲突 (AstrBot 也有 persona) | 禁用 AstrBot 内置人格，完全用 MaiBot 的 |
| 多平台语境差异 (QQ vs Telegram) | 平台特征作为 MaiBot 推理的额外输入 |
| 数据库 schema 不兼容 | 各自维护自己的存储，通过 MCP API 抽象 |
