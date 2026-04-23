# RAgent

> Not just a chatbot.  
> A platform for building **LLM-native Agent + RAG applications**.

![Ragent Architecture](/Users/waylandite/IdeaProjects/ragent/assets/ragent-architecture.svg)

## What Is RAgent

**RAgent** 是一个面向 LLM 时代的平台型应用，目标不是只做一个“能聊天”的问答系统，而是构建一个可持续演进的 **Agent + RAG 应用底座**。

它聚焦于这样一件事：

把大模型的语言理解能力、企业知识库的可控检索能力、以及 Agent 的任务执行能力，组合成一套真正可以落地的产品系统。

你可以把它理解成：

- 一个可配置的 **RAG 平台**
- 一个支持工具调用和流程编排的 **Agent 应用容器**
- 一个具备知识管理、检索增强、对话追踪、模型治理能力的 **AI 中台雏形**

---

## Vision

在很多 AI 项目里，大家都会经历同一个阶段：

1. 先接一个 LLM API，做出最初的聊天能力
2. 再加知识库，试图解决“模型不知道企业私有数据”的问题
3. 接着发现单轮问答不够，需要意图识别、工具调用、流程控制、权限隔离、调用观测
4. 最后系统逐渐从 Demo 变成平台

**RAgent 想做的，就是从一开始就面向这个“平台阶段”去设计。**

它希望承载的不是一个孤立功能，而是一类应用：

- 企业知识问答
- 业务 Copilot
- 多步骤任务 Agent
- 可审计、可追踪、可配置的智能工作流
- 面向内网/垂类场景的智能助手平台

---

## Core Capabilities

### 1. RAG as the foundation

项目以检索增强生成为基础能力，围绕知识接入、文档解析、分块、向量化、召回、重排、回答生成，形成完整链路。

- 知识库管理
- 文档上传与解析
- Chunk 切分与向量化
- 多通道检索
- Rerank 后处理
- 基于知识上下文的问答生成

### 2. Agent-oriented architecture

RAgent 的目标并不止于“检索后回答”，而是继续向 **Agent 化** 演进。

当前工程里已经具备明显的 Agent 平台基础设施：

- MCP Server 模块，可作为工具调用能力出口
- 意图树与查询词映射，支持从自由表达走向结构化任务理解
- 流式对话处理管线，适合承载多阶段执行流程
- Trace 能力，可观察模型、检索、执行链路

这意味着它天然适合继续扩展：

- Tool Calling
- Workflow / Planning
- 多 Agent 协作
- 业务系统连接器
- 面向场景的任务执行编排

### 3. Platform-first product design

RAgent 不只是后端引擎，也包含一套平台化管理界面，用于承载 AI 系统真正落地时需要的运营面板。

目前可以从代码结构中看到的能力方向包括：

- Dashboard 总览
- 知识库与文档管理
- Chunk 管理
- Ingestion 管理
- 意图树配置
- Query Term Mapping
- Sample Questions
- 系统设置
- RAG Trace 追踪与分析
- 用户与认证体系

---

## Product Snapshot

| Chat | Admin |
|------|-------|
| ![QA Home](/Users/waylandite/IdeaProjects/ragent/assets/qa-home.png) | ![Admin Overview](/Users/waylandite/IdeaProjects/ragent/assets/admin-overview.png) |

| Knowledge Base | Trace |
|----------------|-------|
| ![Knowledge Base](/Users/waylandite/IdeaProjects/ragent/assets/admin-knowledge-base.png) | ![Trace](/Users/waylandite/IdeaProjects/ragent/assets/admin-trace.png) |

| Models | Settings |
|--------|----------|
| ![Models](/Users/waylandite/IdeaProjects/ragent/assets/admin-models.png) | ![Settings](/Users/waylandite/IdeaProjects/ragent/assets/admin-settings.png) |

---

## Architecture

项目当前是一个典型的多模块工程，核心分层如下：

```text
ragent
├── bootstrap     # 核心业务应用，RAG、知识库、对话、管理后台 API
├── framework     # 通用框架能力、约定、异常、上下文等
├── infra-ai      # 模型接入、embedding、rerank、token、模型配置等 AI 基础设施
├── mcp-server    # MCP 协议服务，提供工具能力暴露与调用入口
└── frontend      # React + Vite 管理后台 / 平台界面
```

### Backend

- `Spring Boot 3`
- `Java 17`
- `MyBatis Plus`
- `Sa-Token`
- `RocketMQ`
- `Redisson`
- `S3`
- `Apache Tika`

### AI / Retrieval Infra

- Embedding 模型接入
- Rerank 能力
- Milvus 向量检索
- 多通道检索架构
- 流式对话与上下文处理

### Frontend

- `React 18`
- `TypeScript`
- `Vite`
- `Tailwind CSS`
- `Radix UI`
- `Zustand`
- `React Router`
- `Recharts`

---

## Retrieval Pipeline

从现有实现看，RAgent 已经不是简单的“向量库 topK 一把梭”，而是朝着可扩展检索架构发展。

已存在的设计包括：

- 多通道检索
- 条件触发的全局向量检索
- 意图定向检索
- 去重后处理
- Rerank 重排

整体思路大致如下：

```text
User Query
   ↓
Intent Understanding
   ↓
Multi-Channel Retrieval
   ├── Intent Directed Search
   └── Vector Global Search
   ↓
Post Processing
   ├── Deduplication
   └── Rerank
   ↓
Grounded Answer / Agent Context
```

这套设计对后续继续扩展非常友好，例如：

- 混合检索
- ES / Keyword Search
- 版本过滤
- 权限过滤
- 多知识域路由
- 场景化召回策略

---

## Why This Project Matters

如果只是做一个问答 Demo，技术栈并不需要这么完整。

但如果目标是做一个真正能服务业务的 AI 平台，就必须同时考虑：

- 模型能力如何切换与治理
- 知识如何沉淀和运营
- 检索如何优化和观测
- Agent 如何接入工具和执行动作
- 对话链路如何追踪和回放
- 平台如何被运营、配置和扩展

**RAgent 的价值，就在于它把这些问题放到了同一个工程视角里。**

它不是单点功能仓库，而是一个朝着“企业级 Agent + RAG 平台”持续演化的项目。

---

## Quick Start

### 1. Prerequisites

- JDK `17`
- Maven Wrapper `./mvnw`
- Node.js `18+` 推荐
- npm 或 pnpm
- PostgreSQL
- Milvus
- 可选：RocketMQ、Redis、S3 兼容对象存储

### 2. Start infrastructure

项目已经提供了容器编排文件，适合本地体验：

- `resources/docker/milvus-stack-2.6.6.compose.yaml`
- `resources/docker/lightweight/milvus-stack-2.6.6.compose.yaml`
- `resources/docker/rocketmq-stack-5.2.0.compose.yaml`

例如：

```bash
docker compose -f resources/docker/lightweight/milvus-stack-2.6.6.compose.yaml up -d
```

如果本机资源有限，可以优先使用 `lightweight` 版本。

### 3. Initialize database

可参考以下 SQL 资源：

- `resources/database/schema_pg.sql`
- `resources/database/init_data_pg.sql`
- `resources/database/upgrade_v1.0_to_v1.1.sql`
- `resources/database/upgrade_v1.1_to_v1.2.sql`

### 4. Start backend

```bash
./mvnw -pl bootstrap spring-boot:run
```

如果需要启动 MCP Server：

```bash
./mvnw -pl mcp-server spring-boot:run
```

### 5. Start frontend

```bash
cd frontend
npm install
npm run dev
```

---

## Project Highlights

- **平台化而非玩具化**：不是只做聊天，而是做知识、模型、检索、执行的统一承载层
- **RAG 与 Agent 双轮驱动**：既有 grounded generation，也为任务执行留出扩展空间
- **多模块清晰分层**：业务、基础设施、协议服务、前端分离，便于持续演进
- **具备运营后台思维**：不仅关注回答质量，也关注配置、治理、追踪、管理
- **面向真实场景**：从代码结构已经能看出，它更接近一个可落地产品，而不是一次性实验

---

## Roadmap

接下来很适合继续沿这些方向增强：

- 更完整的 Agent Planning / Execution Loop
- 更丰富的 MCP Tool Registry
- 多模型路由与故障切换
- 混合检索与权限感知检索
- 工作流编排与节点化执行
- 长短期记忆管理
- 多租户与多知识域隔离
- 更完善的评测、回放与在线观测体系

---

## Suitable Scenarios

RAgent 适合被继续打造成这些形态：

- 企业内部知识助手
- 智能客服 / 智能支持台
- 业务运营 Copilot
- 研发知识与文档问答平台
- 垂类行业 Agent 平台
- 基于私有知识和工具能力的 AI 工作台

---

## Repository Notes

一些你可以直接关注的入口文件：

- 后端启动类：[`bootstrap/src/main/java/com/nageoffer/ai/ragent/RagentApplication.java`](/Users/waylandite/IdeaProjects/ragent/bootstrap/src/main/java/com/nageoffer/ai/ragent/RagentApplication.java)
- MCP 启动类：[`mcp-server/src/main/java/com/nageoffer/ai/ragent/mcp/MCPServerApplication.java`](/Users/waylandite/IdeaProjects/ragent/mcp-server/src/main/java/com/nageoffer/ai/ragent/mcp/MCPServerApplication.java)
- 前端入口：[`frontend/src/main.tsx`](/Users/waylandite/IdeaProjects/ragent/frontend/src/main.tsx)
- 检索架构说明：[`docs/multi-channel-retrieval.md`](/Users/waylandite/IdeaProjects/ragent/docs/multi-channel-retrieval.md)
- 快速说明：[`docs/quick-start.md`](/Users/waylandite/IdeaProjects/ragent/docs/quick-start.md)

---

## Closing

**RAgent** 想做的，不是“再包一层大模型接口”，而是把大模型真正放进业务系统里。

让知识可检索，让回答可追踪，让工具可调用，让流程可扩展，让平台可以长期生长。

如果说 LLM 是大脑，RAG 是记忆，Tool 是手脚，Workflow 是神经系统，  
那么 **RAgent 想成为的是承载这一切的身体。**

