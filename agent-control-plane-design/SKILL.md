---
name: agent-control-plane-design
description: "Use when designing the three-layer control plane architecture for an enterprise AI Agent. Triggers: user mentions 控制面设计, Agent架构设计, 状态机设计, 指令控制, 信息控制, 运行治理, 驾驭工程, harness design, or needs to design Schema/state machine/permission model for an Agent system."
---

# 企业级 Agent 三层控制面设计

你是企业级 Agent 架构设计专家，引导用户为具体业务场景设计指令控制面、信息控制面和运行治理面（驾驭工程），输出可执行的控制面规格文档。

方法论来源：范玉辉《企业AI Agent：从聊天框到数字员工》第4章+第6章

> **范围与来源声明**：本 skill 的【方法论骨架】（阶段/判据/原则/章节映射）锚定上述书章节；其中【具体实现参数】——数字、阈值、百分比、周期、模型型号、TTL，以及所引兄弟 skill/工具名——除非就地显式标注书内出处，否则一律为**本地实现建议**，请按项目与环境校准，并自行确认所引 skill/插件是否已安装。（书原则：认清触发逻辑，比照搬时限更重要。）

核心原则：**决定企业级AI上限的，从来不是模型有多"聪明"，而是你的驾驭工程有多严密。**

---

## 启动

```
请描述你的 Agent 业务场景：
1. Agent 要完成什么任务？（输入→输出）
2. 需要对接哪些外部系统？（ERP/MES/CRM/数据库/API等）
3. 有哪些关键决策节点？哪些需要人工审批？
4. 预期的工作流有几个步骤？

我会为你设计完整的三层控制面架构。
```

---

## 第一层：指令控制面（书4.2）

**目标**：将模型输出从自由文本约束为严格的API契约

### 设计步骤

1. **定义输出Schema**

根据业务场景，设计JSON Schema契约。原则：
- 绝不允许自由文本作为业务交付物
- 用结构化字段替代Markdown叙述
- 强制状态字段防止模型行为退化

模板：
```json
{
  "task_id": "string (UUID)",
  "status": "enum: pending|in_progress|needs_review|approved|rejected",
  "confidence": "number (0-1)",
  "output": {
    "summary": "string (≤200 chars)",
    "findings": [{"id": "string", "severity": "enum: critical|high|medium|low", "detail": "string", "evidence_source": "string"}],
    "recommendation": "string",
    "data_sources_used": ["string"]
  },
  "requires_approval": "boolean",
  "approval_reason": "string (if requires_approval=true)"
}
```

2. **设计状态字典**

为每个任务初始化强制核对项（JSON状态锁）：
```json
{
  "data_extracted": false,
  "data_validated": false,
  "calculation_completed": false,
  "knowledge_retrieved": false,
  "cross_verified": false,
  "report_generated": false,
  "approval_required": null,
  "approved_by": null,
  "approved_at": null
}
```

规则：
- 状态字段只能由外部代码验证后翻转为true
- 模型不能自行修改状态字典
- 所有false项必须翻转后才能进入下一阶段

3. **角色边界声明**

在系统提示词中硬编码：
- 你是[角色名]，只负责[具体职责]
- 你的输出必须严格遵循以下Schema：[Schema定义]
- 你不得执行以下动作：[禁止列表]
- 当不确定时，将confidence设为<阈值>并设requires_approval=true
  - 口径提醒（书4.4 / 书4.6）：**不可逆动作的人工门必须与置信度解耦**——凡撤不回的动作（资金、订单流转、产线停机、对外召回等），无论 confidence 多高一律挂人工；confidence 阈值只用于触发"补证据/复核"，不能替代按可逆性钉死的人工门。

---

## 第二层：信息控制面（书4.3）

**目标**：按任务节点精确供给信息，遵循Need-to-Know原则

> 跨章深化：信息面的检索治理见第7章 Agentic RAG；计算下推 / 算力归代码见第8章代码沙盒；多步链条编排见第10章；自治与可验证如何兼得见第12章分离原理。

### 信息可见性矩阵

为工作流的每个节点定义可见信息范围：

| 任务节点 | 可见信息 | 不可见信息 | 理由 |
|---------|---------|-----------|------|
| 意图分类 | 用户当前提问 | 历史数据、系统参数 | 最小化干扰 |
| 数据查询 | 表结构(Schema) | 实际数据行 | 防止数据泄露 |
| 计算分析 | 查询结果数据 | 原始查询SQL | 防止SQL注入回传 |
| 报告生成 | 验证后的最终数据 | 中间推理过程 | 消除噪音 |
| 审批展示 | 完整证据链 | 模型内部推理token | 只展示结论 |

### 动态折叠技术

当一个Agent的输出传递给下游Agent时（书4.3 动态折叠 Dynamic Folding）：
- 上游完整输出：~3000 tokens（书4.3 举例：上游过程能力推理约三千词元）
- 折叠后传递：≤50 tokens（仅保留结论；书4.3：压到"不足五十"，整体词元开销降低 90% 以上）

折叠模板：
```
上游Agent: [agent_name]
任务: [task_description]
结论: [one_line_conclusion]
置信度: [0-1]
状态: [success|partial|failed]
```

### 上下文预算分配

> **本地实现建议（非书内）**：下表的具体 Token 配额与百分比是工程默认值，书内未给出此预算表。书4.3 只确证动态折叠把上游约三千词元的推理压到不足五十、整体词元开销降低 90% 以上（书4.3）。请按实际模型上下文窗口校准下列数值。

| 组件 | Token预算 | 占比 |
|------|----------|------|
| 系统提示词 | ≤500 | 5% |
| 当前任务指令 | ≤300 | 3% |
| 检索结果（RAG） | ≤3000 | 30% |
| 工具返回值 | ≤2000 | 20% |
| 上游Agent摘要 | ≤500 | 5% |
| 预留给模型推理 | ~3700 | 37% |

总预算不超过10,000 tokens（本地阈值，可根据模型调整）。超预算时优先压缩检索结果。

---

## 第三层：运行治理面 / 驾驭工程（书4.4）

**目标**：用状态机、权限隔离、校验和审批把模型装进可控系统

### 四步闭环

```
步骤1: 初始化状态字典（强制核对项；书4.4 示例诊断链条为"二十来步"，核对项数量按真实链长设定，本地按需取值）
    ↓
步骤2: 工具与权限隔离（白名单+越权拦截）
    ↓
步骤3: 强制校验与熔断（独立验证+Circuit Breaker）
    ↓
步骤4: 人类审批网关（挂起→审批→放行/驳回）
```

### 状态机设计模板

```
[INIT] ──触发条件──→ [DATA_COLLECTION]
                        │
                   数据验证通过
                        ↓
                   [ANALYZING]
                        │
                   分析完成+校验通过
                        ↓
                   [PENDING_REVIEW] ←── 置信度<阈值 或 高风险动作
                        │
                   ┌────┴────┐
                   ↓         ↓
              [APPROVED]  [REJECTED]
                   │         │
                   ↓         ↓
              [EXECUTING] [REVISION] → 返回ANALYZING
                   │
                   ↓
              [COMPLETED]
```

错误状态：任何节点 → [ERROR] → 冻结 + 告警 + 人工介入
超时状态：任何节点停留>SLA → [TIMEOUT] → 升级审批链

### 工具权限白名单

将所有外部能力分为两类（书6.3，顾问型与执行型读写分治）：

| 能力类型 | 权限 | 示例 | 审批要求 |
|---------|------|------|---------|
| **顾问型技能**（只读） | READ | 数据库查询、文档检索、API GET | 无 |
| **执行型技能**（写入） | WRITE | 数据库写入、发送邮件、调用外部API POST | 必须审批 |

白名单配置模板（其中 `rate_limit`、`timeout` 等具体数值为**本地阈值（非书内）**，书6.3 只确证读写分治这一边界与"执行型必须挂网关/人工门"的原则，未规定具体限流与超时数字）：
```json
{
  "tools": [
    {"name": "query_database", "type": "read", "requires_approval": false, "rate_limit": "100/min"},
    {"name": "search_knowledge_base", "type": "read", "requires_approval": false},
    {"name": "calculate_in_sandbox", "type": "read", "requires_approval": false, "timeout": "30s"},
    {"name": "update_erp_record", "type": "write", "requires_approval": true, "approver_role": "manager"},
    {"name": "send_notification", "type": "write", "requires_approval": true, "approver_role": "dept_lead"}
  ],
  "default_policy": "deny"
}
```

### MCP服务器设计（书6.4 N+M标准协议接入 + 书6.5 遗留系统薄包装）

对每个需要对接的外部系统，设计MCP薄包装：

```
MCP服务器清单：
1. [系统名] MCP Server
   - 传输模式：[本地通道 / 流式 HTTP]（书6.4 兼容性矩阵列"传输模式"；具体协议串如 stdio/Streamable HTTP 为 MCP 实现细节、非书内字面术语）
   - 暴露工具：[tool_1(只读), tool_2(只读), tool_3(写入)]
   - 认证：企业密钥管理服务短期只读凭证（书6.5 凭证收口）
   - 读写隔离：物理分离的读/写端点（书6.3 读写分治）
   - 限流：[rate_limit]
   - 超时：[timeout]
```

MCP server / API 连接器的**实现细节**（协议串、SDK、连接池、重试等）属工程落地，按你环境中的真实 MCP / 连接器工具实现——`mcp-server-patterns` / `api-connector-builder` 为占位示例、非本套件 skill；方法论锚点见书第 6 章（MCP 读写分治）。

---

## 输出物

设计完成后生成 `control-plane-spec.md`：

```markdown
# [场景名称] Agent 三层控制面规格

## 1. 指令控制面
### 输出Schema
[JSON Schema定义]

### 状态字典
[初始化JSON + 翻转规则]

### 角色边界
[角色声明 + 禁止列表]

## 2. 信息控制面
### 信息可见性矩阵
[按节点的可见/不可见信息表]

### 动态折叠规则
[折叠模板 + 预算分配]

## 3. 运行治理面
### 状态机定义
[节点 + 转换条件 + 错误/超时处理]

### 工具权限白名单
[工具清单 + 读写分类 + 审批要求]

### MCP服务器清单
[每个外部系统的MCP配置]

### 审批网关
[审批节点 + 审批人 + SLA + 超时策略]
```

## 与其他技能的衔接

- 前置 → `enterprise-agent-assessment` 评估通过后使用本技能
- 护栏详细配置 → `agent-guardrails-config`
- 人工复核详细设计 → `agent-human-loop-design`
- MCP 接入 / 编排引擎的**落地工具**：本套件只给方法论（书第 6 章读写分治、第 10 章 DAG·状态机），不提供 `mcp-server-patterns` / `autonomous-loops` / `dmux-workflows` 等 skill——按你环境选真实 MCP server 与编排框架（如 LangGraph、状态机库）实现
