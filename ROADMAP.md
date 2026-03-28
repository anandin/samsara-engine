# 🗺️ SAMSARA ENGINE — Build Roadmap

## Philosophy

Ship the core loop first. Every feature must serve the iteration cycle:
- Does this help evaluate faster?
- Does this help trace failures more precisely?
- Does this close the production → test feedback loop?

If no to all three, it ships later.

---

## Phase 1: Samsara Core (Weeks 1–4)
**Goal: Prove the trace + eval loop works. One framework, end-to-end.**

### Week 1: Instrumentor + Trace Capture

**What:** Thin wrapper that captures every step of a LangGraph agent execution.

**Deliverables:**
- `SamsaraInstrumentor` class with `start()`, `stop()`, `trace()` methods
- LangGraph middleware: `with samsara.langgraph("agent-name"): graph.invoke(state)`
- Trace data model: step_type, tool_name, input, output, latency_ms, llm_decision, session_id
- Local trace viewer (CLI + simple web UI): `samsara trace --session-id abc123`
- Tests: 3 happy-path traces + 3 failure-mode traces

**What success looks like:**
- Developer can wrap any LangGraph agent in < 5 lines of code
- Every tool call and LLM decision appears in the trace within 100ms
- No performance degradation > 5% on agent execution

**Stack:** Python, LangGraph, SQLite (local trace store), FastAPI (trace server)

---

### Week 2: Eval Harness + Regression Gate

**What:** The thing that makes Samsara actually useful. Run test cases against an agent. Score the trajectory. Block bad deploys.

**Deliverables:**
- `TestCase` model: input_task, expected_tools[], expected_outcome, eval_rubric
- `TestSuite` model: collection of TestCases, metadata, last_run_at, pass_rate
- `EvalRunner`: executes agent against test case, captures trace, scores trajectory
- Scoring: tool selection accuracy (did it call the right tools in right order?) + outcome match
- `samsara eval --suite my-suite --agent my-agent` CLI command
- GitHub Actions integration: `samsara-action` that gates PRs on suite pass rate
- Local web UI: run eval, see pass/fail, drill into failures

**What success looks like:**
- 5 test cases in suite on day 1
- Running `samsara eval` shows pass/fail for each case with step-level diff
- GitHub Actions workflow blocks merge when pass rate < 100%

**Edge cases to handle:**
- Agent times out mid-execution → mark as FAIL, capture partial trace
- Agent calls wrong tool → step scored individually, overall FAIL
- Non-deterministic path but correct outcome → configurable: strict vs. relaxed scoring

---

### Week 3: Memory Layer (MVP)

**What:** Persistent episodic + semantic memory for agents. Not full architecture — just enough to prove the concept.

**Deliverables:**
- `MemoryStore` with four layers:
  - **Episodic**: store/retrieve session events (what happened, what tools were called, what outcomes occurred)
  - **Semantic**: entity store (things the agent knows about the world)
  - **Procedural**: skill registry (how to do X — prompt templates, method references)
  - **Working**: current context window (what's the agent thinking right now)
- Memory API: `memory.store(layer, content)`, `memory.query(query_text)`, `memory.recall(entity)`
- LangGraph memory integration: agent can call `memory_tool` like any other tool
- Memory versioning: every update to a semantic entry creates a new version with timestamp
- Memory persistence: SQLite → PostgreSQL when deployed (simple migration path)

**What success looks like:**
- Developer adds `memory.store("episodic", {"event": "user asked about refund policy", "outcome": "escalated to human"})` 
- In a future session, `memory.query("refund policy")` returns that event
- Agent uses memory as a tool, developer sees memory calls in trace

---

### Week 4: Production → Test Conversion + Dashboard

**Deliverables:**
- "Create test from failure" button in trace viewer
- One click: production failure trace → new TestCase saved to suite
- `SamsaraDashboard`: health scores per agent, failure mode distribution, suite trend over time
- Metrics tracked: task completion rate, mean steps to completion, regression pass rate, cost per task
- Alert configuration: Slack webhook / email when health score drops below threshold
- Deployment to production: `samsara deploy` → cloud-hosted dashboard + API

**What success looks like:**
- Engineer gets Slack alert: "Agent health score dropped from 91% to 54% in last hour"
- Opens dashboard, sees the exact step that failed
- Clicks "create test from this failure" → test case saved
- Fixes the bug → eval passes → health score recovers

---

## Phase 2: Multi-Framework + Collaboration (Weeks 5–8)

### Week 5–6: Framework Expansion

**What:** Support CrewAI, OpenAI Agents SDK, and MCP-native agents.

**Deliverables:**
- `CrewAIInstrumentor`: crew callback hook → trace events
- `OpenAIInstrumentor`: decorator-based trace for Agents SDK
- `MCPInstrumentor`: MCP protocol proxy that captures all tool calls
- Unified trace schema: all frameworks produce the same trace format
- Framework detection: auto-detect which framework agent uses, apply correct instrumentor

**What success looks like:**
- A CrewAI team can `pip install samsara-engine` and wrap their crew in < 10 lines
- Same dashboard, same eval harness, same memory layer
- Developer can mix framework: LangGraph supervisor + CrewAI sub-agents, all traced

---

### Week 7–8: Team Features

**Deliverables:**
- **Team workspaces**: shared test suites, shared memory stores, team-wide health dashboards
- **Role-based access**: owner, editor, viewer — who can run evals vs. who can just view
- **SSO**: Google + GitHub OAuth (Phase 2B: SAML for enterprise)
- **Shared eval templates**: " here's a pre-built suite for customer support agents" — one-click import
- **Collaboration in trace review**: comment on traces, tag teammates, assign action items
- **Audit log**: who changed what test case, when, why

---

## Phase 3: Intelligence + Scale (Weeks 9–16)

### Week 9–10: Intelligent Test Generation

**What:** Don't write test cases manually. Generate them from:

- **Prompt variants**: automatically generate edge-case inputs from a base prompt
- **Production failure clustering**: cluster similar failures, write one test that covers the cluster
- **Adversarial inputs**: red-team the agent automatically (fuzzing for prompts)
- **Regression oracle**: when a test fails, auto-suggest what likely caused it based on recent changes

**Stack:** GPT-4 class model for test generation + pattern matching on failure clusters

---

### Week 11–12: Agent Memory Architecture (Full)

**What:** The complete memory system. Make it genuinely useful, not just a proof of concept.

- **Cross-agent memory**: one agent's episodic memory can bootstrap another agent
- **Memory compaction**: older sessions compressed into semantic summaries
- **Memory editing UI**: human can view, edit, delete memory entries
- **Memory version diff**: see how an agent's understanding of something changed over time

---

### Week 13–14: Enterprise Infrastructure

**Deliverables:**
- **On-premise deployment**: Docker compose + Kubernetes Helm chart
- **SSO/SAML**: Okta, Azure AD, Google Workspace
- **Custom model fine-tuning**: use failure pattern data to fine-tune evaluation models
- **Multi-region deployment**: US, EU, APAC
- **SOC 2 Type II compliance**

---

### Week 15–16: The Platform Loop

**What:** Close the loop fully. The system gets smarter with every test run.

- **Failure prediction**: "this prompt change is likely to break these 3 test cases before you run them"
- **Eval optimization**: "your suite has 47 tests but 12 of them are redundant — merge them"
- **Competitive benchmarking**: "agents using Samsara score X% better on GAIA benchmark"
- **Marketplace**: community-shared test suites for common agent types (customer support, code review, research assistant)

---

## Architecture Decisions to Lock Early

### SQLite → PostgreSQL (not at start, but Day 1 schema design)
Trace data grows fast. Design schema for migration from day 1.

### OpenTelemetry for traces
Standard trace format means: any observability tool (Datadog, Honeycomb, Grafana) can ingest Samsara traces without lock-in.

### MCP-native from Week 1
MCP is becoming the USB-C of AI tools. Build for it from the start, not as an afterthought.

### API-first
Everything in the web UI must be achievable via API. Teams will want to script their own workflows.

---

## Metrics to Track Every Week

| Metric | Target |
|--------|--------|
| Test suites created | Growing |
| Avg. tests per suite | > 20 by week 8 |
| Time from failure to test | < 5 min |
| Production failures caught by regression | Increasing |
| Health score of agents on platform | > 80% average |
| Eval runs per week per team | > 3 |
| Memory entries stored | Growing |
| Community test suites shared | > 5 by week 12 |
