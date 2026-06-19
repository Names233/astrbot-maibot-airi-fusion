# Digital Genesis — AstrBot × MaiBot × AIRI × Hermes × MemPalace 五合一融合架构

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

```
┌──────────────────────────────────────────────────────────────────┐
│                          用户                                    │
│           QQ / 微信 / Telegram / Discord / 直播 / Web            │
└───────────────────────┬──────────────────────────────────────────┘
                        │
                        ▼
┌──────────────────────────────────────────────────────────────────┐
│                AstrBot (大脑层 / Decision Hub)                    │
│                                                                   │
│   决策中枢 │ 平台路由 │ 任务调度 │ Skills │ 插件生态 │ MCP Hub   │
│                                                                   │
│   判断任务类型 → 路由到合适的 Agent                                │
│   ├── 闲聊/情感    → MaiBot (灵魂)                               │
│   ├── 视觉/语音    → AIRI (化身)                                  │
│   ├── 技术执行     → Hermes (双手)                                │
│   └── 所有路径     → MemPalace (记忆)                             │
└───────┬──────────────┬──────────────┬────────────────────────────┘
        │              │              │
        ▼              ▼              ▼
┌──────────────┐ ┌──────────────┐ ┌──────────────────────────────┐
│  MaiBot      │ │    AIRI      │ │         Hermes               │
│  (灵魂层)    │ │  (化身层)    │ │        (执行层)              │
│              │ │              │ │                               │
│ 推理引擎     │ │ Live2D/VRM   │ │ Terminal │ File │ Web Search │
│ 人格系统     │ │ TTS/STT      │ │ Code Exec │ Deploy │ Ops    │
│ 情感模型     │ │ 口型同步     │ │ 子任务委派 │ 项目管理        │
│ 氛围感知     │ │ 游戏Agent    │ │                               │
│ 表达学习     │ │ 多端渲染     │ │ MCP Server (7 tools)         │
└──────────────┘ └──────────────┘ └──────────────────────────────┘

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

## 一、五个项目各自的能力

### 1.1 AstrBot — 大脑层 (Decision Hub)

仓库: https://github.com/AstrBotDevs/AstrBot
Star: 34.9k | Python 3.10+ | License: AGPL-3.0 | 版本: v4.25.5

AstrBot 是整个数字生命的**大脑和决策中枢**。它不只是聊天机器人平台，而是一个完整的 Agent 运行时，负责理解用户意图、判断任务类型、路由到合适的执行者。

| 模块 | 职责 |
|------|------|
| **决策中枢** | 分析用户意图，判断任务类型，选择执行路径 |
| **平台路由** | QQ/微信/TG/Discord/飞书/钉钉 等 18+ 平台消息收发 |
| **Sub-Agent 调度** | 通过 HandoffTool 将任务委托给 MaiBot / AIRI / Hermes |
| **MCP Hub** | MCP 协议连接外部工具服务 (MemPalace, Hermes 等) |
| **Skills** | 技能管理、注册、执行 |
| **插件生态** | 1000+ 插件的加载和管理 |
| **Agent Sandbox** | 隔离安全执行代码、Shell 调用 |
| **会话管理** | 多轮对话上下文管理、自动压缩 |

**AstrBot 的决策流程:**

```
用户消息进入
  │
  ├─ 判断: 闲聊/情感/氛围感知 → 调用 MaiBot (人格回复)
  │    └─ MaiBot 返回拟人化回复 → AstrBot 发送到平台
  │
  ├─ 判断: 需要视觉/语音/游戏 → 调用 AIRI (化身表现)
  │    └─ AIRI 执行动画/语音/游戏操作
  │
  ├─ 判断: 技术任务(代码/部署/运维/调研) → 调用 Hermes (执行)
  │    └─ Hermes 执行完毕 → 通过 AstrBot 发送结果到平台
  │
  └─ 所有路径 → MemPalace (读写记忆、更新知识图谱)
```

### 1.2 MaiBot — 灵魂层 (Personality)

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

### 1.3 AIRI — 化身层 (Embodiment)

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

### 1.4 Hermes — 执行层 (Hands)

仓库: https://github.com/NousResearch/hermes-agent
文档: https://hermes-agent.nousresearch.com/docs
License: Custom

Hermes 是数字生命的**双手**——负责实际的技术操作。当 AstrBot 判断某个任务需要代码编写、服务器部署、文件操作、网络调研等技术执行时，会将任务委托给 Hermes。

| 模块 | 职责 |
|------|------|
| Terminal | 执行 Shell 命令、管理后台进程 |
| File Operations | 读写文件、搜索、补丁 |
| Web Search | 互联网搜索、网页内容提取 |
| Code Execution | Python 代码执行、脚本运行 |
| Browser | 网页交互、截图、表单填写 |
| Sub-Agent Delegation | 将复杂任务委派给子 Agent |
| Skill System | 可复用的技能库（持久化过程记忆） |
| Memory | 跨会话持久记忆 |
| Cron/Scheduling | 定时任务调度 |

**Hermes 暴露的 MCP 工具:**

| MCP 工具 | 功能 |
|----------|------|
| `hermes_terminal` | 执行 shell 命令，支持前台/后台、超时、工作目录 |
| `hermes_file_read` | 读取文件内容，支持分页 |
| `hermes_file_write` | 写入文件，自动创建父目录 |
| `hermes_search_files` | 搜索文件内容或按名称查找文件 |
| `hermes_web_search` | 互联网搜索 |
| `hermes_web_extract` | 提取网页/PDF 内容为 Markdown |
| `hermes_code_exec` | 执行 Python 代码，可调用其他工具 |
| `hermes_delegate` | 将子任务委派给独立的子 Agent |

### 1.5 MemPalace — 记忆层 (Memory Palace)

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

---

## 二、Hermes 接入架构

### 2.1 接入方式: MCP 工具层 + HandoffTool 调度层

Hermes 的接入采用**方案 c: 双层架构**：

```
┌─────────────────────────────────────────────────────────┐
│                    AstrBot (大脑)                        │
│                                                         │
│   SubAgentOrchestrator                                  │
│     └── HandoffTool: transfer_to_hermes                 │
│           ├── description: "技术执行任务委托给Hermes"     │
│           ├── system_prompt: "你是技术执行者..."          │
│           ├── tools: ["hermes_*"]                        │
│           └── background_task: true                      │
│                                                         │
│   FunctionToolManager                                   │
│     └── MCP Client → Hermes MCP Server                  │
│           ├── hermes_terminal                            │
│           ├── hermes_file_read                           │
│           ├── hermes_file_write                          │
│           ├── hermes_search_files                        │
│           ├── hermes_web_search                          │
│           ├── hermes_web_extract                         │
│           ├── hermes_code_exec                           │
│           └── hermes_delegate                            │
└─────────────────────────────────────────────────────────┘
                        │
                        │ MCP (stdio/SSE)
                        ▼
┌─────────────────────────────────────────────────────────┐
│                    Hermes (执行者)                        │
│                                                         │
│   MCP Server                                            │
│     ├── 暴露 8 个工具给 AstrBot                          │
│     ├── 内部调用 Hermes 完整工具链                        │
│     └── 结果返回给 AstrBot                               │
│                                                         │
│   反向调用                                              │
│     └── Hermes → MCP → AstrBot.send_message()           │
│         (执行完毕后通过 AstrBot 发送结果到聊天平台)       │
└─────────────────────────────────────────────────────────┘
```

**为什么用方案 c:**

| 层 | 作用 | 优势 |
|----|------|------|
| MCP 工具层 | Hermes 暴露工具接口 | 标准协议、安全白名单、进程隔离 |
| HandoffTool 调度层 | AstrBot 将 Hermes 注册为 Sub-Agent | 保持 Agent 自主性、支持多步推理、background_task |

### 2.2 AstrBot 端注册配置

```yaml
# AstrBot Sub-Agent 配置

subagent:
  agents:
    - name: "hermes"
      enabled: true
      public_description: "技术执行专家：代码编写、服务器部署、运维操作、网络调研、文件管理。当用户需要实际执行技术任务时委托给 Hermes。"
      system_prompt: |
        你是 Hermes，数字生命的技术执行者。
        你负责实际完成技术操作：编写代码、部署服务、管理文件、搜索信息。
        
        能力:
        - 执行任意 Shell 命令
        - 读写和搜索文件
        - 运行 Python 代码
        - 搜索互联网、提取网页内容
        - 将复杂任务委派给子 Agent
        
        工作方式:
        1. 接收任务描述
        2. 规划执行步骤
        3. 使用工具逐步完成
        4. 返回执行结果
        
        注意: 你执行完任务后，结果会通过 AstrBot 发送给用户。
      tools:
        - "hermes_terminal"
        - "hermes_file_read"
        - "hermes_file_write"
        - "hermes_search_files"
        - "hermes_web_search"
        - "hermes_web_extract"
        - "hermes_code_exec"
        - "hermes_delegate"
```

### 2.3 Hermes MCP Server 配置

```python
# hermes_mcp_server.py
# Hermes 作为 MCP Server，暴露工具给 AstrBot

from mcp import Server
from mcp.types import Tool, TextContent

server = Server("hermes")

@server.tool()
async def hermes_terminal(command: str, workdir: str = None, timeout: int = 180) -> str:
    """执行 Shell 命令"""
    # 调用 Hermes 内部 terminal 工具
    result = await hermes_internal.terminal(command, workdir=workdir, timeout=timeout)
    return result["output"]

@server.tool()
async def hermes_file_read(path: str, offset: int = 1, limit: int = 500) -> str:
    """读取文件内容"""
    result = await hermes_internal.read_file(path, offset=offset, limit=limit)
    return result["content"]

@server.tool()
async def hermes_file_write(path: str, content: str) -> str:
    """写入文件"""
    result = await hermes_internal.write_file(path, content)
    return f"Written to {path}"

@server.tool()
async def hermes_search_files(pattern: str, path: str = ".", target: str = "content") -> str:
    """搜索文件内容或名称"""
    result = await hermes_internal.search_files(pattern, path=path, target=target)
    return str(result["matches"])

@server.tool()
async def hermes_web_search(query: str, limit: int = 5) -> str:
    """搜索互联网"""
    result = await hermes_internal.web_search(query, limit=limit)
    return str(result["data"]["web"])

@server.tool()
async def hermes_web_extract(urls: list[str]) -> str:
    """提取网页内容"""
    result = await hermes_internal.web_extract(urls)
    return str(result["results"])

@server.tool()
async def hermes_code_exec(code: str) -> str:
    """执行 Python 代码"""
    result = await hermes_internal.execute_code(code)
    return result["output"]

@server.tool()
async def hermes_delegate(goal: str, context: str = "") -> str:
    """将子任务委派给独立子 Agent"""
    result = await hermes_internal.delegate_task(goal=goal, context=context)
    return result["summary"]

# 反向调用: Hermes → AstrBot
@server.tool()
async def hermes_reply_to_chat(message: str, platform: str, target: str) -> str:
    """通过 AstrBot 发送消息到聊天平台"""
    result = await astrbot_mcp_client.call_tool("send_message", {
        "platform": platform,
        "target": target,
        "message": message
    })
    return "sent"
```

---

## 三、五系统 MCP 通信拓扑

```
                    ┌─────────────────┐
                    │   MemPalace     │
                    │   MCP Server    │
                    │   (29 tools)    │
                    └────────┬────────┘
                             │ MCP (stdio/SSE)
         ┌───────────┬───────┼───────┬───────────┐
         ▼           ▼       ▼       ▼           ▼
    ┌─────────┐ ┌────────┐ ┌─────┐ ┌─────────┐ ┌─────────┐
    │ AstrBot │ │ MaiBot │ │AIRI │ │ Hermes  │ │         │
    │ MCP Hub │ │MCP Hub │ │MCP  │ │ MCP Srv │ │         │
    └────┬────┘ └────────┘ └─────┘ └────┬────┘ └─────────┘
         │                               │
         │  AstrBot ──MCP──► Hermes      │
         │  (调用技术工具/委托任务)       │
         │                               │
         │  AstrBot ──MCP──► MaiBot      │
         │  (调用人格推理)               │
         │                               │
         │  AstrBot ──MCP──► AIRI        │
         │  (调用化身表现)               │
         │                               │
         │  Hermes ──MCP──► AstrBot      │
         │  (发送消息到平台)             │
```

**MCP 连接关系:**

```
AstrBot  ──MCP──► MemPalace    (记忆读写)
AstrBot  ──MCP──► MaiBot       (人格推理)
AstrBot  ──MCP──► AIRI         (化身表现)
AstrBot  ──MCP──► Hermes       (技术执行)
MaiBot   ──MCP──► MemPalace    (记忆读写)
AIRI     ──MCP──► MemPalace    (记忆读写)
Hermes   ──MCP──► MemPalace    (记忆读写)
Hermes   ──MCP──► AstrBot      (发送消息到平台)
```

每个系统都直连 MemPalace，不经过中间层，最小化延迟。AstrBot 作为中枢，是唯一同时连接所有其他系统的节点。

---

## 四、数据流示例

### 场景 1: 群聊 + 记忆增强

```
用户在QQ群: "推荐个Python框架"
  → AstrBot QQ适配器接收
  → AstrBot 决策中枢判断: 闲聊/推荐 → 调用 MaiBot
  → MaiBot 推理引擎启动
  → MemPalace search("Python框架", wing="users/用户ID")
    → 返回: 用户3个月前说过在学 FastAPI，上周说觉得 Django 太重
  → MemPalace kg_query("用户")
    → 返回: {用户, 擅长, Python}, {用户, 偏好, 轻量框架}
  → MaiBot 人格系统生成: "你不是在用 FastAPI 嘛，挺好的呀，还要别的吗？"
  → MemPalace add_drawer(对话记录, wing="users/用户ID", room="conversations")
  → AstrBot 发回QQ群
```

### 场景 2: 技术任务 + Hermes 执行 (NEW)

```
用户在Telegram: "帮我在服务器上部署一个 FastAPI 服务"
  → AstrBot Telegram适配器接收
  → AstrBot 决策中枢判断: 技术任务 → 调用 Hermes (background_task=true)
  → Hermes 接收任务: "在服务器上部署 FastAPI 服务"
  
  → Hermes 执行步骤:
    1. hermes_web_search("FastAPI deployment best practices")
    2. hermes_terminal("ssh user@server 'uname -a'")
    3. hermes_file_write("main.py", fastapi_code)
    4. hermes_file_write("Dockerfile", docker_config)
    5. hermes_terminal("docker build -t fastapi-app .")
    6. hermes_terminal("docker run -d -p 8000:8000 fastapi-app")
    7. hermes_terminal("curl http://localhost:8000/health")
  
  → Hermes 通过 AstrBot 发送结果:
    "FastAPI 服务已部署完成 ✅
     - 地址: http://server:8000
     - 健康检查: 通过
     - Docker 容器: running"
  
  → MemPalace add_drawer("部署了FastAPI服务", wing="users/xxx", room="technical")
  → MemPalace kg_add("用户", "使用", "FastAPI")
```

### 场景 3: 直播 + 游戏 + 记忆

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

### 场景 4: 跨平台记忆连续 + 技术执行

```
# 周一 QQ 群
用户: "我最近在准备面试，想练算法"
  → AstrBot 调用 MaiBot → MemPalace add_drawer + kg_add
  
# 周三 Telegram
用户: "帮我写个 LRU Cache 的 Python 实现，加上测试"
  → AstrBot 决策中枢判断: 技术任务 → 调用 Hermes
  → MemPalace search("用户技术背景", wing="users/xxx")
    → 返回: 用户擅长 Python，在学 Rust，目标是后端岗位
  → Hermes 执行:
    1. hermes_file_write("lru_cache.py", lru_implementation)
    2. hermes_file_write("test_lru_cache.py", test_cases)
    3. hermes_terminal("python -m pytest test_lru_cache.py -v")
    4. hermes_code_exec("验证边界情况")
  → Hermes 通过 AstrBot 发送:
    "LRU Cache 实现完成 ✅
     - lru_cache.py: O(1) get/put，用 OrderedDict
     - test_lru_cache.py: 12 个测试用例全部通过
     - 包含边界情况: 容量为0、重复key、访问顺序更新"
  → AstrBot (MaiBot 人格化包装): "给你写好了，测试全过！面试加油 💪"
```

---

## 五、共享 Soul 配置

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

  decision:           # → AstrBot 使用
    platforms: [qq, telegram, discord]
    plugins: { auto_discover: true }
    subagents:
      - name: "hermes"
        trigger: "技术任务、代码、部署、运维、调研、文件操作"
      - name: "maisaka"
        trigger: "闲聊、情感、氛围感知"

  execution:          # → Hermes 使用
    mcp_tools: [terminal, file_read, file_write, search_files, web_search, web_extract, code_exec, delegate]
    default_workdir: "/workspace"
    sandbox: true
    max_concurrent_tasks: 3

memory:               # → MemPalace 使用
  palace_path: "/data/palace"
  backend: "pgvector"
  embedding_model: "embeddinggemma-300m"

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
    hermes:
      rooms: [diary, task_history, deployments, code_snippets]

  knowledge_graph:
    enabled: true
    auto_extract: true

  mining:
    auto_save: true
    dedup_threshold: 0.9
```

---

## 六、部署架构

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
      HERMES_MCP_URL: "http://hermes:9090"

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

  hermes:
    image: nousresearch/hermes-agent:latest
    depends_on: [astrbot, mempalace]
    ports:
      - "9090:9090"
    volumes:
      - hermes_data:/root/.hermes
      - /var/run/docker.sock:/var/run/docker.sock  # 容器管理
      - ./soul.yaml:/root/.hermes/soul.yaml:ro
    environment:
      ASTRBOT_MCP_URL: "http://astrbot:6196"
      MEMPALACE_MCP_URL: "http://mempalace:8080"
      HERMES_MCP_PORT: "9090"

volumes:
  pg_data:
  redis_data:
  palace_data:
  astrbot_data:
  maibot_data:
  airi_data:
  hermes_data:
```

---

## 七、License 兼容性

| 项目 | License | 兼容性 |
|------|---------|--------|
| AstrBot | AGPL-3.0 | ⚠️ 桥接层需独立模块，MCP 协议隔离 |
| MaiBot | GPL-3.0 | ⚠️ 桥接层需独立模块，MCP 协议隔离 |
| AIRI | MIT | ✅ |
| Hermes | Custom (Nous Research) | ✅ |
| MemPalace | MIT | ✅ |

MCP 协议是进程间通信，不要求代码层面的融合，各项目保持独立 license。

---

## 八、实现路线图

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
- [ ] 集成 AstrBot 决策中枢调度

### Phase 4: Hermes 接入 (2-3 周)
- [ ] Hermes MCP Server 实现 (暴露 8 个工具)
- [ ] AstrBot SubAgentOrchestrator 注册 Hermes
- [ ] HandoffTool 配置 (background_task=true)
- [ ] 反向调用: Hermes → AstrBot.send_message()
- [ ] 验证技术任务全流程: 用户指令 → AstrBot 决策 → Hermes 执行 → 结果回传
- [ ] Hermes 翼楼配置 (task_history, deployments, code_snippets)

### Phase 5: AIRI 接入 (3-4 周)
- [ ] AIRI 桥接 (MaiBot + AstrBot + MemPalace)
- [ ] TTS + 口型同步 + 表情系统
- [ ] 游戏 Agent 日记写入 MemPalace
- [ ] 直播场景打通

### Phase 6: 深度优化 (持续)
- [ ] MemPalace 知识图谱自动推理
- [ ] 跨翼楼隧道自动发现
- [ ] 记忆衰减和重要性排序
- [ ] 多 Agent 记忆隔离和共享策略
- [ ] AstrBot 决策路由优化 (基于任务类型自动选择最优 Agent)
- [ ] Hermes 执行结果自动写入知识图谱
