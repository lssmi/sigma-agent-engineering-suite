# Sigma Agent Engineering — 企业 AI Agent 工程化 Skill 套件

[English](./README.md) | **简体中文**

> 把一个**会说话但不稳定的大模型**，按质量工程这门学科装配成**可控、可审计、运行中持续自我迭代**的企业级数字作业系统 —— 一个面向 [Claude Code](https://claude.com/claude-code) 的 7-skill 套件，落地专著《企业 AI Agent：从聊天框到数字员工》(Sigma Engineering 范式) 的方法论。

本套件把质量工程（六西格玛/精益/约束理论/可靠性工程）反过来用于**建造和治理 Agent 本身** —— 不是让 Agent 去跑六西格玛，而是用这门学科把一个会说话但不稳定的大模型，工程化成可靠的数字作业系统。

主线一句话：**`R ≈ ∏ pᵢ`** —— 任务整体可靠度是单步可靠度的连乘。每步 95%、跑 20 步，整体只剩约 `0.95²⁰ ≈ 0.36`。要把这条曲线抬起来，只有三件事可做，这套 skill 就是把这三件事工程化。

---

## 套件构成（1 主编排器 + 6 子 skill）

| Skill | 职责 | 管辖阶段 |
|---|---|---|
| **`sigma-agent-engineering`** | 端到端主编排器：把 6 个子 skill 焊成一条「两速控制流水线」，按任务本性分诊路由 | P0–P11 全流程 |
| `enterprise-agent-assessment` | 立项分诊 / 可行性评估 / CDOC Concept 与能力门 | P0 / P5 |
| `agent-control-plane-design` | 三层控制面（指令/信息/运行治理）+ MCP 接入 + 沙盒 + 编排 | P1–P4 |
| `agent-guardrails-config` | 第一道防线 Safety-I 结构化护栏 + 第三道防线记忆/权限/知识代谢 | P6 |
| `agent-human-loop-design` | 第二道防线：按**可逆性**（非置信度）放行的人工复核可逆门 | P6 |
| `agent-observability-setup` | 独立测量总线 + 可观测性 + 评估成本 + DMAIC 判例飞轮 | P7–P10 |
| `agent-production-gate` | 规模化生产准入：五步准入（能力门先于流量门）+ 合规 + 三组看板 | P11 |

> 主 skill 持有四份共享底稿（术语 canon、阶段流水线、方法论账本、两条贯穿案例），保证 6 个子 skill 引用同一套口径。细节走渐进式披露，下沉到 `sigma-agent-engineering/reference/`。

## 核心方法论（4 个不可简化的锚）

1. **`R ≈ pⁿ`（主线）** — 真实写法是条件概率连乘 `R = ∏ pᵢ`；`pⁿ` 只是建立直觉的乐观基线，真实衰减因 self-conditioning 更陡。
2. **两速控制系统** — 快内环（执行/驾驭工程，压方差）+ 慢外环（DMAIC/改进，固化永久门禁）+ 中间独立测量总线耦合。
3. **三手段** — 抬高单步可靠度 base p / 缩短步数 N / 给失败兜底封顶 ⊓（Safety-I + Safety-II）。
4. **正交轴 II（信任与遏制）** — 对付提示词注入/越权的独立轴，不是 `pⁿ` 随机误差；核心是别让一个 Agent 同时握私有数据 + 读不可信内容 + 对外发送（**致命三联**）。

## 安装

把这 7 个目录复制到你的 Claude Code skills 目录：

```bash
git clone https://github.com/lssmi/sigma-agent-engineering-suite.git
cp -R sigma-agent-engineering-suite/sigma-agent-engineering \
      sigma-agent-engineering-suite/enterprise-agent-assessment \
      sigma-agent-engineering-suite/agent-control-plane-design \
      sigma-agent-engineering-suite/agent-guardrails-config \
      sigma-agent-engineering-suite/agent-human-loop-design \
      sigma-agent-engineering-suite/agent-observability-setup \
      sigma-agent-engineering-suite/agent-production-gate \
      ~/.claude/skills/
```

之后在 Claude Code 里提到「企业 Agent 工程化 / 数字员工 / 从立项到生产 / Sigma Engineering / R≈pⁿ」等即可触发主编排器；它会按任务本性路由到对应子 skill，或直接放行轻量任务（不强制走全程）。

## 示例

`sigma-agent-engineering/examples/` 含两个**过程可视化**演示与一次 QFD×跨模型对抗评审走查：

- `8d-run.html` — 制造业 8D 诊断 Agent 沿 P0–P11 走一圈的过程图
- `qfd-walkthrough.html` — 用 QFD 设计「数字质量工程师 Agent」的四阶段 AB 角互换评审
- `qfd-digital-qe-agent/` — 上述走查的逐阶段产物与 Codex 评审记录

> ⚠️ **所有示例数据均为模拟/示例值**，用于演示流水线机制与门禁，非真实生产数据。

## 来源、授权与边界

- **方法论来源**：本套件的方法论提炼自专著《企业 AI Agent：从聊天框到数字员工》(范玉辉, Sigma Engineering 范式)。作者作为权利人，将本 **skill 实现**以 MIT 协议开源。
- **授权边界**：MIT 协议覆盖本仓的 **skill 代码与结构**（SKILL.md / reference 编排 / 示例）。书面专著正文本身的著作权另行保留——本套件是方法论的工程化落地，不是书的全文复制。
- **作者本地核验源**：仓内多处提到「八审清样 `/tmp/book8/`」是作者本机的书稿底稿，**未随本仓发布、外部用户也无需访问**。需要核对数值/章节时，以仓内 `reference/` 各文件已核验台账 + 正式出版书为准。
- **非官方关联**：本项目是面向 Claude Code 的第三方 skill 套件，与 Anthropic 无隶属关系。
- **免责**：方法论与示例仅供工程参考，不构成对任何具体生产系统可靠性、合规性或安全性的保证；落地请结合自身环境验证。

## License

[MIT](./LICENSE) © 2026 范玉辉 (Fan Yuhui)
