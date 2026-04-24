<div align="center">

# 🤖 AI Team — Turn Your Copilot Into a 9-Role Engineering Squad

**Stop treating Copilot / Cursor / Cline like a lone intern. Give your project a structured AI team with conventions, memory, and arbitration.**

中文 · [English](README.en.md) · [Quick Start](#-quick-start-30-seconds) · [Templates](#-three-templates-pick-one) · [Philosophy](#-design-philosophy)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](#-contributing)
[![Made for Copilot](https://img.shields.io/badge/Made%20for-Copilot%20%2F%20Cursor%20%2F%20Cline-blue)](#)

</div>

---

## 🧩 What problem does it solve?

Have you ever:

- ❌ Watched Copilot produce **a different code style every single time**?
- ❌ Said "continue" only to have the Agent **start from scratch**?
- ❌ Asked for a bug fix and gotten a **surprise architectural rewrite**?
- ❌ Realized that on a team, **every developer's AI output looks like a different project**?
- ❌ Tried to constrain AI by writing huge `copilot-instructions.md` files that **only got messier**?

**AI Team** gives your project a structured 9-role AI squad with **a single prompt**:

| Role | Responsibility | Prevents |
| --- | --- | --- |
| 🎯 Product | Turns vague requests into PRDs with acceptance criteria | Word-of-mouth requirements |
| 🛠 Dev | Enforces "Data → Logic → State → UI" layering | UI layer fetching directly, layer-skipping |
| 🧪 Test | Required test scenarios + coverage thresholds | Shipping without tests |
| ✅ Verify | Hard pre-delivery checklist | "Should be fine, push it" |
| ⚖ Arbitrate | A/B/C-type conflict resolution | "Both options seem fine" |
| 🧠 Memory | Persists task state, supports "continue/resume" | Context loss |
| 📈 Enhance | Post-task reflection + spec versioning | Frozen, dead specs |
| 🚦 Router | Loads only relevant roles per keyword | Context bloat & hallucination |
| 📜 Protocol | Values, authority order, global bans | Role conflicts |

---

## ⚡ Quick Start (30 seconds)

```bash
cd your-project
curl -O https://raw.githubusercontent.com/<your-username>/Agents/main/prompts/generate-ai-team.prompt.md
# Then paste the file's contents into Copilot Chat / Cursor / Cline (Agent mode + a top-tier model recommended)
```

The Agent will scan your project (`package.json`, `pubspec.yaml`, `pom.xml`, `go.mod`...), infer your architecture, naming conventions, and data flow, then generate:

```
.github/
├── copilot-instructions.md          ← Protocol layer: routing, values, bans
├── agents/                          ← 7 role specs tailored to your project
│   ├── product.md  dev.md  test.md  verify.md
│   └── arbitrate.md  memory.md  enhance.md
└── memory/
    ├── arbitration-log.md           ← Tracked decisions
    └── decisions.md                 ← Architectural decisions
```

After that, every Copilot interaction loads only the relevant roles, follows the spec, and produces predictable, auditable output.

---

## 🎁 Three Templates, Pick One

| You want | Use | Path |
| --- | --- | --- |
| 🚀 One-click generation | Main prompt | [`prompts/generate-ai-team.prompt.md`](prompts/generate-ai-team.prompt.md) |
| 📦 Single-file template | Single-file | [`templates/single-file/`](templates/single-file/) |
| 🧱 Modular (protocol / roles / generator separated) | Split | [`templates/split/`](templates/split/) |
| ✍️ Fully manual, no AI inference | Legacy | [`templates/legacy/`](templates/legacy/) |

---

## 💡 Design Philosophy

1. **Roles over monolith** — Routing table loads only relevant roles per task; no context bloat.
2. **Arbitration over freestyle** — Three conflict types (A/B/C). C-type forces human intervention.
3. **Memory over amnesia** — State snapshot in `.github/memory/`, resumable like an IDE reopen.
4. **Self-evolution** — Post-task reflection accumulates into a backlog of spec improvements.

For the full Chinese version with FAQ, advanced usage, and rationale, see [README.md](README.md).

---

## 🤝 Contributing

Issues and PRs are very welcome:

- 🌐 Add framework adapters (provide a sample project + inference rules)
- 🎨 Refine role templates
- 🐛 Report real cases where the Agent inferred wrong
- 📖 Translate docs to other languages

## 📜 License

MIT
