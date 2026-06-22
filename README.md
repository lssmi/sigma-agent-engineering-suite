<div align="center">

![Sigma Agent Engineering](./assets/hero.png)

**English** | [简体中文](./README.zh-CN.md)

[![license](https://img.shields.io/badge/license-MIT-0f766e)](./LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude_Code-skill_suite-d97757)](https://claude.com/claude-code)
![skills](https://img.shields.io/badge/skills-7-0f172a)
![methodology](https://img.shields.io/badge/Sigma_Engineering-R%E2%89%88%E2%88%8Fp%E1%B5%A2-334155)
![PRs](https://img.shields.io/badge/PRs-welcome-b45309)

**Assemble a fluent-but-unreliable LLM into a controllable, auditable, continuously self-improving enterprise digital worker.**
A 7-skill suite for [Claude Code](https://claude.com/claude-code) that operationalizes the *Sigma Engineering* methodology.

</div>

---

## The problem

An LLM that's fluent in a chat box is **not** a worker you can put on a production line. A real task isn't one clever answer — it's a 20-step chain, and **reliability is multiplied, not added**:

> ### `R ≈ ∏ pᵢ`  →  `0.95²⁰ ≈ 0.36`

95% per step across 20 steps, and only about **a third** of runs finish clean. Worse, the real decay is *steeper* than `pⁿ` because errors self-condition the chain downstream.

Most teams wire up the execution loop, ship a demo that works on stage… and then **production breaks, and there's no way to tell why.** That's not a prompt problem — it's a *reliability engineering* problem. This suite gives you the full pipeline to lift that curve, the way quality engineering lifts a manufacturing line.

## How it works — a two-speed control system

![The two-speed control pipeline](./assets/pipeline.png)

- **Fast inner loop (P1–P6)** — execution / *harness engineering*. Crush variance, gate every step. No learning, no reflection — just pass-or-stop.
- **Slow outer loop (P7–P11)** — improvement + scale-up: **P7–P10 DMAIC** (turn every real failure into a *permanent* gate, so the same bug can never ship twice) + **P11 production-readiness gate** (admission, not a DMAIC step).
- **Independent measurement bus (P7)** — the truth source that couples both loops, measured *independently* of what it measures (no self-fulfilling tests).
- **Case-law flywheel** — `Control → Define`: every fix becomes an asset the system carries forward.

You can do exactly three things to that `∏ pᵢ` curve: **raise per-step reliability**, **shorten the step count** (hand deterministic work to code), and **cap failures with a floor** (Safety-I to catch the foreseeable + Safety-II to degrade gracefully). The suite operationalizes all three.

## What's in the suite

A main **orchestrator** welds 6 focused sub-skills into one pipeline and routes by the nature of the task — it does **not** force every task through all 12 stages.

| Skill | Responsibility | Stages |
|---|---|---|
| **`sigma-agent-engineering`** | End-to-end **orchestrator** — the two-speed control pipeline + task triage | P0–P11 |
| `enterprise-agent-assessment` | Project intake & triage / feasibility / CDOC Concept & capability gate | P0 · P5 |
| `agent-control-plane-design` | Three-layer control plane (instruction / information / runtime) + MCP + sandbox + orchestration | P1–P4 |
| `agent-guardrails-config` | First line of defense (Safety-I structured guardrails) + third line (memory · permissions · knowledge metabolism) | P6 |
| `agent-human-loop-design` | Second line of defense — human review **gated by reversibility, not confidence** | P6 |
| `agent-observability-setup` | Independent measurement bus + observability + evaluation/cost + DMAIC case-law flywheel | P7–P10 |
| `agent-production-gate` | Scale-up go-live — five-step admission (capability gate *before* traffic gate) + compliance + dashboards | P11 |

## Core methodology (4 anchors)

1. **`R ≈ pⁿ`** — the real form is a conditional-probability product `R = ∏ pᵢ`; `pⁿ` is only an optimistic baseline.
2. **Two-speed control** — fast loop crushes variance, slow loop freezes failures into gates, measurement bus couples them.
3. **Three levers** — raise base `p` · shorten `N` · cap with a floor `⊓` (Safety-I + Safety-II).
4. **Orthogonal Axis II — Trust & Containment** — an independent axis for adversarial threats, *not* `pⁿ` random error. Never let one agent hold private data **+** read untrusted content **+** have an outbound channel (the **lethal trifecta**).

## Installation

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

Then mention things like *"enterprise agent engineering / digital worker / from project to production / Sigma Engineering / R≈pⁿ / agent reliability engineering"* in Claude Code to trigger the orchestrator.

## See it run

Two **process-visualization** walkthroughs ship in [`sigma-agent-engineering/examples/`](./sigma-agent-engineering/examples/):

**A manufacturing 8D diagnostic agent walking the full P0–P11 loop** ([`8d-run.html`](./sigma-agent-engineering/examples/8d-run.html))

![8D diagnostic agent run](./assets/example-8d.png)

**Designing a "digital quality engineer" agent with QFD + cross-model adversarial review** ([`qfd-walkthrough.html`](./sigma-agent-engineering/examples/qfd-walkthrough.html))

![QFD cross-model review walkthrough](./assets/example-qfd.png)

> ⚠️ All example data is **simulated / illustrative** — it demonstrates pipeline mechanics and gates, not real production data.

## Provenance, license & boundaries

- **Methodology source.** Distilled from the book *Enterprise AI Agents: From Chatbox to Digital Worker* (《企业 AI Agent：从聊天框到数字员工》) by **John Fan** (范玉辉 / Fan Yuhui), Sigma Engineering paradigm. As the author and rights holder, John Fan releases this **skill implementation** under the MIT License.
- **License boundary.** MIT covers the **skill code and structure** in this repo (SKILL.md / reference orchestration / examples). Copyright in the book's prose itself is reserved separately — this suite is an engineering implementation of the methodology, not a reproduction of the book.
- **Author-local verification source.** References to "八审清样 `/tmp/book8/`" are the author's local manuscript drafts — **not shipped and not needed**. To verify figures or chapters, use the in-repo `reference/` verified ledgers plus the published book.
- **Not affiliated** with Anthropic. **Disclaimer:** methodology and examples are for engineering reference only and guarantee nothing about any specific production system's reliability, compliance, or security. Validate against your own environment.

## License

[MIT](./LICENSE) © 2026 John Fan · x@ainewmeth
