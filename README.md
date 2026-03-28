# ⚛️ Samsara Engine
### The continuous reliability platform for AI agents

[![CI](https://github.com/anandin/samsara-engine/actions/workflows/ci.yml/badge.svg)](https://github.com/anandin/samsara-engine/actions)
[![License: MIT](https://img.shields.io/badge/License-MIT-purple.svg)](https://opensource.org/licenses/MIT)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)

> *"You can build a working AI agent in a day. Making it reliable is the hard part."*
> — Every AI developer, every day

Samsara Engine is the missing reliability layer for AI agents. It gives you the same feedback loop that software has — CI/CD, regression testing, structured memory, production monitoring — adapted for the reality of LLM-based agents.

## The Problem

```
Software Dev:  git commit → CI runs → test fails → fix → ship
AI Agent Dev:   build agent → demo works → production breaks → debug for 3 days → pray
```

Six pain points every AI developer knows:
1. **Repeatability** — same input, different tool path, different result
2. **Memory failures** — context windows don't fix what memory should do
3. **Debugging black boxes** — wrong answer, no trace, no idea which step failed
4. **No evaluation framework** — agents need trajectory scoring, not just output scoring
5. **No regression safety net** — a fix for one thing breaks another silently
6. **Multi-agent hype** — most "multi-agent systems" are one broken loop with extra steps

## The Solution: The Iteration Loop

```
Week N:
  1. Evaluate agents against test suite
  2. Ship passing changes to production  
  3. Monitor live traffic
  4. Convert production failures → test cases
  5. Return to Step 1

Week N+1: Better. Always.
```

The compounding effect is the product. A failure found in week 1 becomes a regression test by week 2. By week 10, your agent has been through 10 rounds of systematic failure → fix → coverage expansion.

## What You Get

| Feature | What It Does |
|---------|-------------|
| **🐕 Fidelity Testing** | CI/CD for agents. Generate test cases, score trajectories, gate deploys on pass rate |
| **🧠 Structured Memory** | Episodic + semantic + procedural + working memory. Not a chat history — a mind |
| **🏃 Step Traces** | Every tool call, every LLM decision, every context retrieval — captured and annotated |
| **🚨 Production → Test** | One click: production failure → permanent regression test |
| **📊 Reliability Dashboard** | Agent health scores, failure mode distribution, cost per task |
| **🔁 Iteration Loop** | Weekly eval cycles. Compounding coverage. Systematic reliability |

## Quick Start

```bash
pip install samsara-engine
```

**Wrap a LangGraph agent:**
```python
from samsara import Samsara

with Samsara("my-support-agent") as samsara:
    result = agent_graph.invoke({"task": user_input})
    
# View the trace
# samsara trace --session-id <latest>
```

**Run fidelity tests:**
```bash
samsara eval --agent my-support-agent --suite customer-support-defaults
```

**Get a health score:**
```bash
samsara health --agent my-support-agent
```

## Supported Frameworks

| Framework | Support | How |
|-----------|---------|-----|
| LangGraph | ✅ Week 1 | Middleware |
| CrewAI | ✅ Week 5 | Crew callbacks |
| OpenAI Agents SDK | ✅ Week 5 | Decorator |
| MCP-native agents | ✅ Week 5 | Protocol proxy |
| Custom / any | ✅ Always | Webhook-style trace upload |

## Architecture

```
Agent (any framework)
    │
    ▼
Samsara Instrumentor
    │  ← captures every step
    │
    ▼
Samsara Cloud API
    ├── Trace Engine     ← step-by-step execution capture
    ├── Memory Store     ← episodic + semantic + procedural + working
    ├── Eval Harness    ← trajectory scoring + regression gate
    └── Dashboard       ← health scores + alerts + trends
    │
    ▼
CI/CD (GitHub Actions, Jenkins, GitLab)
```

## Why "Samsara"?

In Buddhist philosophy, **samsara** is the cycle of death and rebirth — the endless loop of suffering caused by ignorance of how things truly are.

The AI agent development cycle is samsara:
- Build → break → debug → rebuild → break again → repeat

The path out is awareness + practice. Samsara Engine provides:
- **Awareness** — traces show you exactly what's happening
- **Practice** — eval suites + regression testing = systematic improvement
- **Liberation** — an agent you can trust in production

## Status

🟡 **Pre-launch** — PRD complete, engineering starts soon

See the full spec: [PROBLEM.md](PROBLEM.md) · [SOLUTION.md](SOLUTION.md) · [ROADMAP.md](ROADMAP.md)

## Contributing

Issues, PRs, and test suite templates welcome. This is an open project — the reliability layer for AI agents should be built by the community that needs it.

```
git clone https://github.com/anandin/samsara-engine
cd samsara-engine
pip install -e .
```

## License

MIT — use it, break it, make it reliable, ship it.
