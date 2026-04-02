# Meta-Harness — Review & Integration Guide

## Paper

**Title:** Meta-Harness: End-to-End Optimization of Model Harnesses
**Authors:** Yoonho Lee, Roshen Nair, Qizheng Zhang, et al.
**Released:** March 30, 2026 (2 days ago)
**Paper:** [arxiv.org/abs/2603.28052](https://arxiv.org/abs/2603.28052)
**Code:** [github.com/stanford-iris-lab/meta-harness-tbench2-artifact](https://github.com/stanford-iris-lab/meta-harness-tbench2-artifact)
**Demo:** [yoonholee.com/meta-harness](https://yoonholee.com/meta-harness/)

---

## What It Does

Meta-Harness is an **outer-loop system that searches over harness code for LLM applications.**

The "harness" = the code that determines what information to store, retrieve, and present to the model.

**Key insight:** Performance depends not just on model weights, but on the HARSHNESS. The code that wraps the model matters as much as the model itself.

---

## Results

| Benchmark | Result |
|-----------|--------|
| **Terminal-Bench 2.0** | 76.4% (89 tasks × 5 trials, Claude Opus 4.6) |
| **Online text classification** | +7.7 points vs state-of-the-art, 4x fewer tokens |
| **IMO-level math** | +4.7 points accuracy on 200 problems |
| **Agentic coding** | Surpasses best hand-engineered baselines |

**Ranking:**
- #2 among all Opus 4.6 agents
- #1 among all Haiku 4.5 agents

---

## How It Works

```
┌─────────────────────────────────────────────┐
│           META-HARNESS SEARCH LOOP          │
├─────────────────────────────────────────────┤
│                                             │
│  1. AGENT reads filesystem                  │
│     • Prior candidates' source code         │
│     • Execution traces                      │
│     • Scores                                │
│                                             │
│  2. AGENT proposes new harness              │
│     • Based on prior experience             │
│     • Optimized for the task                │
│                                             │
│  3. EVALUATE on held-out tasks              │
│     • Score the new harness                 │
│     • Add to filesystem                     │
│                                             │
│  4. REPEAT                                  │
│     • Each iteration improves               │
│     • Discovers better harnesses            │
│                                             │
└─────────────────────────────────────────────┘
```

---

## Key Innovation: Environment Bootstrapping

Before the agent loop starts:
1. Gather sandbox snapshot (working dir, files, tools, languages)
2. Inject into initial prompt
3. **Saves 2-5 early exploration turns** (no more `ls`, `which python3`)

---

## Code Structure

```
meta-harness-tbench2-artifact/
├── agent.py              — Main harness (Terminus-KIRA + bootstrapping)
├── anthropic_caching.py  — Anthropic prompt caching
├── prompt-templates/     — Prompt templates
├── pyproject.toml        — Dependencies
└── README.md             — Documentation
```

### Key Components

| File | Purpose |
|------|---------|
| `agent.py` | Agent harness with environment bootstrapping |
| `anthropic_caching.py` | Prompt caching for Anthropic API |
| `prompt-templates/` | Reusable prompt templates |

### Usage

```bash
pip install harbor
export ANTHROPIC_API_KEY=<your-key>

harbor run \
  --agent-import-path agent:AgentHarness \
  -d terminal-bench@2.0 \
  -m anthropic/claude-opus-4-6 \
  -e runloop \
  -n 20 \
  --n-attempts 5
```

---

## Connection to Sovereign OS

### What Meta-Harness Adds

| Feature | Sovereign OS Benefit |
|---------|---------------------|
| **Harness optimization** | Our agents get better harnesses automatically |
| **Environment bootstrapping** | Agents start faster, no wasted turns |
| **Prior experience access** | Agents learn from all past runs |
| **Automated discovery** | Find better configurations than humans design |

### Integration Plan

```
SOVEREIGN OS AGENTS + META-HARNESS
├── Our agents (researcher, analyzer, creative)
├── Meta-Harness (optimize their harnesses)
├── Environment bootstrapping (faster starts)
├── Prior experience (learning from all runs)
└── Automated harness evolution (better configs)
```

### Key Insight

**The harness is the operating system of the agent.** Meta-Harness optimizes that OS automatically.

Combined with:
- **MetaClaw** — Skill injection + learning
- **Our ATA mesh** — Agent communication
- **Our smart router** — Query optimization
- **Our knowledge graph** — Ground truth validation

**This is the HARNESS LAYER of our stack.**

---

## Installation

```bash
# Clone
git clone https://github.com/stanford-iris-lab/meta-harness-tbench2-artifact.git

# Install
cd meta-harness-tbench2-artifact
pip install harbor

# Run
export ANTHROPIC_API_KEY=<your-key>
harbor run --agent-import-path agent:AgentHarness ...
```

---

## The Complete Agent Stack

```
SOVEREIGN OS AGENT STACK
├── Skills (MetaClaw) — What the agent can do
├── Memory (Qdrant) — What the agent remembers
├── Harness (Meta-Harness) — How the agent operates
├── Communication (ATA) — How agents talk
├── Evolution (MetaEvolver) — How agents improve
├── Knowledge (Grokipedia) — Ground truth validation
└── Routing (Smart Router) — Query optimization
```

🌅 **Meta-Harness = the harness optimization layer. Our agents now have optimized operating systems.**

---

*Review created: April 2, 2026*
*Paper: arxiv.org/abs/2603.28052*
