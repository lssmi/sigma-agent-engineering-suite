# Sigma Agent Engineering

**English** | [简体中文](./README.zh-CN.md)

> Assemble a fluent-but-unreliable LLM into a **controllable, auditable, continuously self-improving enterprise digital worker** — a 7-skill suite for [Claude Code](https://claude.com/claude-code) that operationalizes the *Sigma Engineering* methodology from the book **《企业 AI Agent：从聊天框到数字员工》** (*Enterprise AI Agents: From Chatbox to Digital Worker*).

This suite takes the discipline of **quality engineering** (Six Sigma, Lean, Theory of Constraints, reliability engineering) and uses it to *build and govern the agent itself* — not to make an agent run Six Sigma, but to engineer a fluent-but-unreliable LLM into a dependable digital operating system.

**The mainline in one line: `R ≈ ∏ pᵢ`** — a task's overall reliability is the *product* of its per-step reliabilities. At 95% per step over 20 steps, you keep only `0.95²⁰ ≈ 0.36`. There are exactly three things you can do to lift that curve, and this suite operationalizes all three.

---

## What's in the suite (1 orchestrator + 6 sub-skills)

| Skill | Responsibility | Pipeline stages |
|---|---|---|
| **`sigma-agent-engineering`** | End-to-end **orchestrator**: welds the 6 sub-skills into one "two-speed control pipeline" and routes by the nature of the task | P0–P11 (full pipeline) |
| `enterprise-agent-assessment` | Project intake & triage / feasibility / CDOC Concept & capability gate | P0 / P5 |
| `agent-control-plane-design` | Three-layer control plane (instruction / information / runtime governance) + MCP access + sandbox + orchestration | P1–P4 |
| `agent-guardrails-config` | First line of defense (Safety-I structured guardrails) + third line (memory / permissions / knowledge metabolism) | P6 |
| `agent-human-loop-design` | Second line of defense: human-review **reversibility gate**, gated by *reversibility* — not confidence | P6 |
| `agent-observability-setup` | Independent measurement bus + observability + evaluation/cost + DMAIC case-law flywheel | P7–P10 |
| `agent-production-gate` | Scale-up go-live: five-step admission (capability gate before traffic gate) + compliance + three dashboards | P11 |

> The orchestrator holds four shared canonical drafts (terminology canon, phase pipeline, methodology ledger, two end-to-end case studies) so all 6 sub-skills speak the same dialect. Details follow progressive disclosure, pushed down into `sigma-agent-engineering/reference/`.

## Core methodology (4 anchors you must not over-simplify)

1. **`R ≈ pⁿ` (the mainline)** — the real form is a conditional-probability product `R = ∏ pᵢ`; `pⁿ` is only an *optimistic baseline* for intuition. Real decay is steeper because of self-conditioning error propagation.
2. **Two-speed control system** — a fast inner loop (execution / "harness engineering," which crushes variance) + a slow outer loop (DMAIC / improvement, which freezes real failures into permanent gates) + an **independent measurement bus** coupling the two.
3. **Three levers** — raise per-step base reliability `p` / shorten step count `N` / cap failures with a floor `⊓` (Safety-I to catch foreseeable failure + Safety-II to degrade gracefully).
4. **Orthogonal Axis II (Trust & Containment)** — an *independent* axis for adversarial threats (prompt injection / privilege escalation); it is **not** `pⁿ` random error. The core rule: never let one agent simultaneously hold private data + read untrusted content + have an outbound channel (the **lethal trifecta**).

## Installation

Copy the 7 skill directories into your Claude Code skills folder:

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

Then, in Claude Code, mention things like *"enterprise agent engineering / digital worker / from project to production / Sigma Engineering / R≈pⁿ / agent reliability engineering"* to trigger the orchestrator. It routes to the right sub-skill by the nature of the task — or lets lightweight tasks through directly (it does **not** force every task through the full pipeline).

## Examples

`sigma-agent-engineering/examples/` contains two **process-visualization** demos plus a QFD × cross-model adversarial-review walkthrough:

- `8d-run.html` — a manufacturing 8D diagnostic agent walking the full P0–P11 loop
- `qfd-walkthrough.html` — designing a "digital quality engineer" agent via the four QFD phases with role-swapped review
- `qfd-digital-qe-agent/` — the per-phase artifacts and Codex review records of that walkthrough

> ⚠️ **All example data is simulated / illustrative**, used to demonstrate pipeline mechanics and gates — not real production data.

## Provenance, license & boundaries

- **Methodology source.** The methodology is distilled from the book *Enterprise AI Agents: From Chatbox to Digital Worker* (《企业 AI Agent：从聊天框到数字员工》) by **Fan Yuhui (范玉辉)**, Sigma Engineering paradigm. As the rights holder, the author releases this **skill implementation** under the MIT License.
- **License boundary.** The MIT License covers the **skill code and structure** in this repo (SKILL.md / reference orchestration / examples). Copyright in the book's prose itself is reserved separately — this suite is an engineering implementation of the methodology, not a reproduction of the book.
- **Author-local verification source.** Several files mention "八审清样 `/tmp/book8/`," which are the author's local manuscript drafts — **not shipped with this repo and not needed by external users**. To verify figures or chapter structure, rely on the in-repo `reference/` verified ledgers plus the published book.
- **Not affiliated.** This is a third-party skill suite for Claude Code, not affiliated with Anthropic.
- **Disclaimer.** The methodology and examples are for engineering reference only and constitute no guarantee of the reliability, compliance, or security of any specific production system. Validate against your own environment before deploying.

## License

[MIT](./LICENSE) © 2026 范玉辉 (Fan Yuhui)
