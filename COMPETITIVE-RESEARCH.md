# 🔬 Competitive Research — AI Agent Feedback Loop Landscape
### Samsara Engine · Build Decision Framework

*Research date: 2026-03-28 · Sources: GitHub, Reddit, documentation*

---

## The Landscape — Full Map

### Tier 1: Production Mature (5k+ stars)

#### mem0 — "The Memory Layer for AI Agents"
| | |
|---|---|
| **Stars** | 51.3k ⭐ · 5.7k forks |
| **License** | Apache 2.0 |
| **Last Release** | v1.0.8 (March 26, 2026) |
| **Contributors** | 100+ · YC-backed company |
| **What it does** | Universal memory layer for AI agents. Remembers user preferences, adapts to individual needs, continuously learns. |
| **Memory layers** | User memory + Session memory + Agent memory (3 layers) |
| **Retrieval** | Vector semantic search (LanceDB default, multiple backends) |
| **Framework support** | LangGraph, CrewAI, Vercel AI SDK, OpenAI — good |
| **Eval built-in** | ❌ No |
| **Agent self-participation** | ❌ No — agent is passive recipient of memory |
| **Multi-agent memory** | ❌ No |
| **What it's missing** | Episodic memory (event sequences), procedural memory (how-to), metacognition, self-correction feedback loop, trajectory scoring |
| **Fork difficulty** | Easy — well-structured, good docs, MIT compatible |

**Verdict:** Solves the storage layer. Does not solve the feedback loop. The memory is there but the agent can't interrogate it meaningfully, can't learn from it autonomously, and can't transfer knowledge to other agents. **Valid target for fork of the memory architecture only.**

---

#### deepeval — "Pytest for LLM Applications"
| | |
|---|---|
| **Stars** | 14.3k ⭐ · 1.3k forks |
| **License** | Apache 2.0 |
| **Releases** | v3.9.3 (December 2025) |
| **What it does** | Open-source eval framework for LLM apps. 20+ metrics (G-Eval, RAG, hallucination, agentic). Pytest-like interface. |
| **Eval approach** | LLM-as-judge + NLP models running locally |
| **Agent trajectory scoring** | ⚠️ Partial — has "agentic metrics" but treats agents as black-box LLMs with tools |
| **Framework support** | OpenAI, LangChain, LangGraph, CrewAI, PydanticAI, Anthropic, AWS AgentCore, LlamaIndex |
| **CI/CD integration** | ✅ Yes — pytest + GitHub Actions |
| **Synthetic data generation** | ✅ Yes |
| **What it's missing** | Step-level trajectory scoring (tools + decisions + downstream compounding), agent self-participation in eval, production-to-test conversion, memory integration |
| **Fork difficulty** | Medium — large codebase, well-structured |

**Verdict:** The eval framework closest to what we need. But built for LLM apps, not agents. Doesn't understand trajectories as first-class citizens. No memory. No self-correction. **Valid target for fork of eval harness + CI integration only.**

---

#### coze-loop — "AI Agent Optimization Platform"
| | |
|---|---|
| **Stars** | 5.4k ⭐ · 743 forks |
| **License** | Apache 2.0 (part of ByteDance/Coze) |
| **Language** | Go (68%) + TypeScript (27%) |
| **What it does** | Full lifecycle management: prompt dev → debugging → eval → monitoring. Visual playground. Observability. |
| **Eval approach** | Multi-dimensional automated eval (accuracy, compliance, conciseness) |
| **Observability** | ✅ Full trace recording from input → prompt parsing → model → tool execution |
| **Framework** | Eino framework (Cloudwego) — not LangGraph/CrewAI |
| **What it's missing** | Agent self-participation, memory, multi-agent knowledge transfer, open ecosystem (tied to Coze/ByteDance) |
| **Fork difficulty** | Hard — Go codebase, tied to Eino framework, Coze ecosystem dependency |

**Verdict:** Strong observability layer. But Coze proprietary — won't build on this. Move on.

---

### Tier 2: Emerging (500–5k stars)

#### EvoAgentX — "Self-Evolving Agent Framework"
| | |
|---|---|
| **Stars** | 2.7k ⭐ · 226 forks |
| **License** | MIT |
| **Release** | v0.1.0 (September 2025) |
| **What it does** | Self-evolving agents via 4 optimization algorithms (TextGrad, MIPRO, AFlow, EvoPrompt). Multi-agent workflow auto-generation. Built-in eval. |
| **Self-evolution method** | Prompt + workflow optimization via evolutionary algorithms — NOT memory-based learning |
| **Memory** | Short-term + long-term (generic, no layer taxonomy) |
| **What it's missing** | Episodic/procedural/semantic layer distinction, agent self-reflection, cross-agent knowledge transfer, human-in-loop for prod agents |
| **Fork difficulty** | Medium — Python, clean architecture |

**Verdict:** Closest conceptually to Samsara's evolution vision. But self-evolution = algorithmic prompt optimization, not cognitive memory loop. **Interesting architecture to study, not to fork.**

---

#### trulens — "Evaluation and Tracking for LLM Experiments"
| | |
|---|---|
| **Stars** | 3.2k ⭐ |
| **License** | Apache 2.0 |
| **What it does** | Eval + tracking for LLM experiments. Ground in ground truth + LLM-as-judge. LangChain + LlamaIndex + custom |
| **Agent-aware** | Partial — records full call chain, scores outputs |
| **What it's missing** | Trajectory-level eval, agent self-correction, memory, production-to-test conversion |
| **Fork difficulty** | Medium |

**Verdict:** eval ≠ feedback loop. Not the right foundation.

---

#### giskard-oss — "Open Source Evaluation for LLM Agents"
| | |
|---|---|
| **Stars** | 5.2k ⭐ |
| **License** | MPL 2.0 |
| **Focus** | Enterprise LLM eval + bias detection + regulatory compliance |
| **What it's missing** | Agent self-participation, trajectory scoring, feedback loops |
| **Fork difficulty** | Hard — enterprise focus, complex |

**Verdict:** Wrong segment. Enterprise eval ≠ agent cognitive infrastructure.

---

### Tier 3: Early / Interesting / Niche

#### agent-reliability-engineering — "SRE for AI Agents"
| | |
|---|---|
| **Stars** | 3 ⭐ · 0 forks |
| **License** | MIT |
| **Contributors** | 1 · 14 months production experience |
| **What it does** | Applies SRE principles to AI agents. 6 modules: evals, metrics (imp@ system), transfer, self-improve, config-versioning, patterns. |
| **Eval approach** | Manual scoring with 3 dimensions (success, autonomy, efficiency) on 1-5 scale |
| **Self-improve** | Agents analyze performance data → propose improvements → human approves |
| **Memory** | Config-versioned identity files (SOUL.md, IDENTITY.md concept) |
| **What it's missing** | No automated scoring, no codebase, only shell scripts + templates, 0 community |
| **Why it matters** | Philosophy is almost identical to Samsara's — 15-min weekly review cycle, human-in-loop, trend deltas |
| **Fork difficulty** | Trivial to fork — but too early/light to base anything on |

**Verdict:** Philosophy is right. Execution is a README. **Study this closely for the weekly iteration loop design.**

---

#### self-improving-agent (OpenClaw Skill)
| | |
|---|---|
| **Type** | OpenClaw agent skill |
| **License** | MIT |
| **Contributors** | OpenClaw community |
| **What it does** | Captures errors, corrections, feature requests into `.learnings/` markdown files. Pattern detection. Periodic review. Promotion to project memory files. |
| **Self-correction loop** | ✅ YES — agent triggers own correction capture |
| **Agent-native** | ✅ YES — the agent IS the one capturing, reviewing, promoting |
| **Memory** | File-based: ERRORS.md, LEARNINGS.md, FEATURE_REQUESTS.md |
| **Multi-agent** | ❌ No — per-agent learnings |
| **Eval harness** | ❌ No |
| **Production-to-test** | ❌ No |
| **Anand already running this** | ✅ YES |
| **Fork difficulty** | Easy — Python, file-based, MIT |

**Verdict:** This IS the Samsara core loop — running in Anand's environment right now. The only implementation that treats the agent as the primary actor in its own improvement. **The seed for the real product. Fork and productize this.**

---

#### agentcore-langfuse-continous-eval-loop
| | |
|---|---|
| **Stars** | 15 ⭐ · 6 forks |
| **License** | Apache 2.0 |
| **What it does** | AWS Bedrock AgentCore + Langfuse. Offline eval + Online monitoring. CI/CD pipeline. Golden dataset. User simulation. |
| **Eval approach** | Langfuse remote evaluators + human annotation queues |
| **What it's missing** | Everything outside AWS ecosystem. No memory. No agent self-participation. |
| **Fork difficulty** | Easy — Python, but AWS-locked |

**Verdict:** Not the right base. Too AWS-specific.

---

#### iris-eval/mcp-server
| | |
|---|---|
| **Stars** | 5 ⭐ |
| **What it does** | MCP server for agent eval. Score output quality, catch safety failures, enforce cost budgets. |
| **Why it's interesting** | MCP-native eval. The eval server speaks MCP protocol. |
| **What it's missing** | Everything except scoring + budgets. No memory, no self-correction, no trajectories. |
| **Fork difficulty** | Easy — small codebase |

**Verdict:** Interesting MCP integration pattern. Could be a component.

---

## The Gap Map — What No One Has Solved

```
                    HUMAN-IN-LOOP          AGENT-NATIVE
                    (external)            (self-directed)
                    ──────────            ─────────────
Memory              mem0 (passive)        self-improving
  (storage)                                agent (active)
                    coze-loop             ─────────────
Eval                deepeval
  (scoring)         trulens
                    giskard
                    ──────────            ─────────────
Feedback Loop       agent-reliability     self-improving
(production→test)   engineering           agent (running
                                         in Anand's env!)
                    ──────────            ─────────────
Knowledge           EvoAgentX             ❌ NONE
 Transfer           (algorithmic,
(multi-agent)        not memory-
                     based)
                    ──────────            ─────────────
Trajectory          ❌ PARTIAL            ❌ NONE
 Scoring
```

**The three gaps that define the space:**

**Gap 1: Agent-participatory feedback loop**
Every existing system evaluates agents from the outside. The self-improving agent skill is the only thing where the agent is an active participant in its own improvement. This is the conceptual breakthrough Anand named — treat the AI agent as persona first.

**Gap 2: Multi-layer memory for agents**
mem0 has vector storage + user/session/agent layers. No one has episodic (what happened, step by step), semantic (what do I know), procedural (how do I do X), and working (what am I thinking now) as first-class queryable layers. This is what Anand's Samskara project was exploring.

**Gap 3: Cross-agent knowledge transfer**
When Agent A (customer support) fails on a technical escalation and Agent B (technical support) solves it — Agent A should learn from Agent B's success without human mediation. Nothing exists for this.

---

## Fork / Build / Ignore Decision Matrix

| Repo | Fork? | Build on top? | Ignore? | Why |
|------|-------|---------------|--------|-----|
| **mem0** | ✅ Memory architecture only | ❌ | ❌ | 51k stars, proven infra, add episodic+procedural on top |
| **deepeval** | ✅ Eval harness + CI | ❌ | ❌ | 14k stars, solid infra, add trajectory scoring on top |
| **EvoAgentX** | ❌ | ✅ Study | ✅ | Self-evolution algorithms are interesting, not the right method |
| **agent-reliability-engineering** | ✅ Philosophy + weekly loop design | ✅ | ❌ | Right philosophy, tiny code — use as design reference |
| **self-improving-agent** | ✅ **YES — CORE SEED** | ✅ Extend | ❌ | Already running, agent-native, MIT, the actual concept |
| **coze-loop** | ❌ | ❌ | ✅ | ByteDance proprietary, Go/Eino locked |
| **trulens/giskard** | ❌ | ❌ | ✅ | Wrong segment — enterprise eval ≠ agent feedback |
| **iris-eval/mcp** | ❌ | ✅ MCP pattern | ✅ | Small, interesting MCP integration for eval protocol |
| **agentcore-langfuse-loop** | ❌ | ❌ | ✅ | AWS-locked |

---

## Recommended Path: Fork Two, Build One

### Fork 1: mem0 → Samsara Memory Architecture
**Why fork, not build from scratch:**
- 51k stars, production-tested, multiple vector backends, clean API
- Apache 2.0, already has LangGraph + CrewAI integrations
- Adding episodic + procedural + working memory ON TOP of their vector layer = 20% new code, 80% leverage
- Their user/session/agent 3-layer is a good foundation — we extend to 5-layer

**What to take:**
- Vector store infrastructure (LanceDB, PG, Qdrant, Chroma)
- Memory search/retrieval API
- Framework integration layer (LangGraph, CrewAI hooks)
- Python + TypeScript SDKs

**What to add:**
- Episodic memory: event sequences with tool calls, decisions, outcomes, timestamps
- Procedural memory: skill registry, method templates, how-to representations
- Metacognition layer: agent's model of its own capabilities and limitations
- Self-interrogation API: `memory.ask_about_self("have I handled X before?")`

---

### Fork 2: self-improving-agent → Samsara Core Feedback Loop
**Why fork, not build from scratch:**
- Already running in Anand's environment — has real-world usage
- The agent-native feedback loop concept is proven
- MIT license, simple Python implementation
- Pattern detection + error categorization + promotion workflow = exactly what we need

**What to take:**
- Error capture + categorization (ERRORS.md, LEARNINGS.md, FEATURE_REQUESTS.md)
- Pattern detection (LRN-YYYYMMDD-XXX format, deduplication)
- Periodic review + promotion workflow
- Multi-agent learning file structure

**What to add:**
- Convert file-based storage → proper database (SQLite → PostgreSQL)
- Add trajectory capture (what exact step failed, not just that it failed)
- Add production-to-test conversion (failure trace → TestCase automatically)
- Add cross-agent knowledge transfer (Agent A learns from Agent B)
- Add a human dashboard (web UI for reviewing + approving agent proposals)
- MCP server interface (any agent that speaks MCP can use the feedback loop)

---

### Build: Samsara Trajectory Scoring Engine
**Why not fork for this:**
- Doesn't exist in meaningful form in any of the repos
- deepeval has trajectory-adjacent metrics but treats agents as black boxes
- AgentClash has the right idea (step-by-step replays) but focuses on model comparison, not reliability scoring
- Need to build from scratch: step-level scoring, tool selection accuracy, downstream error propagation tracking

**What it scores:**
```
Plan quality:    Did the agent choose the right high-level approach?
Tool selection: Did it call the right tools in the right order?
Arg quality:    Were the tool inputs correct?
Path validity:   Did it take the shortest path or waste steps?
Recovery:        Did it detect and recover from errors?
Outcome:         Did it complete the task?
```

---

## The Self-Improving Agent — Anand's Version vs. Everything Else

Anand's OpenClaw self-improving agent skill is running in production right now. Every other repo in this landscape is designed for humans to evaluate agents. Anand's is designed for the agent to evaluate itself.

This is the conceptual breakthrough. Every other system asks: "how do we judge the agent?"
Anand's system asks: "how does the agent judge itself?"

The fork of the self-improving agent, combined with the mem0 memory architecture and a new trajectory scoring engine, gives you:
- A memory layer the agent actively queries and updates
- A feedback loop the agent runs on itself
- Trajectory scoring the agent understands
- Cross-agent knowledge transfer the agents manage autonomously
- Human dashboard for oversight, not control

**That's the product. That's Samsara Engine.**

---

## Implementation Order

### Phase 1 (Fork mem0 + Fork self-improving agent → productize)
1. Fork mem0 → add episodic + procedural memory layers
2. Fork self-improving agent → convert to proper DB + add MCP interface
3. Merge: memory layer + feedback loop + MCP server
4. Ship: Python SDK + MCP server + basic web dashboard

### Phase 2 (Build trajectory scoring)
5. Build trajectory scoring engine from scratch
6. Add to MCP server: agent can query "how did I score on this task?"
7. Add to memory: scores become memory entries

### Phase 3 (Multi-agent)
8. Add cross-agent knowledge transfer
9. Add agent capability profiling (what is each agent good at)
10. Add team dashboard

---

## What To Call This Product (Naming Note)

The existing repos are named:
- mem0 — memory storage
- deepeval — evaluation
- EvoAgentX — evolution
- coze-loop — optimization loop
- agent-reliability-engineering — SRE

None of them name the thing Anand named: **Samsara** — the endless cycle of death and rebirth applied to agent failure.

**The name is defensible.** No one else has claimed it. It captures the exact concept: agents break, debug, rebuild, break again — until awareness (of failure patterns) + practice (systematic evaluation) = liberation (reliable agents).

The self-improving agent skill is already the Samsara loop. We're just building the infrastructure to make it scale.
