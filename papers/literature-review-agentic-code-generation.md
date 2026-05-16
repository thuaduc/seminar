# Literature Review: Agentic AI for Code Generation

> **Scope:** LLM agents that plan, write, execute, debug, test, and refine code in realistic software-engineering environments.  
> **Sources:** ICML, NeurIPS, ICLR, ACL, EMNLP, NAACL, ICSE, FSE, ASE (2024–2025)  
> **Total papers:** 50

---

## Table of Contents

1. [End-to-End Autonomous SE Agents](#1-end-to-end-autonomous-software-engineering-agents)
2. [Multi-Agent Frameworks for Code Generation](#2-multi-agent-frameworks-for-code-generation)
3. [Planning & Search Strategies for Code Synthesis](#3-planning--search-strategies-for-code-synthesis)
4. [Debugging & Self-Repair Agents](#4-debugging--self-repair-agents)
5. [Execution Feedback & Iterative Refinement](#5-execution-feedback--iterative-refinement)
6. [Training Agents: RL & Learning from Feedback](#6-training-agents-rl--learning-from-feedback)
7. [Testing & Test-Driven Approaches](#7-testing--test-driven-approaches)
8. [Benchmarks & Evaluation Frameworks](#8-benchmarks--evaluation-frameworks)
9. [Empirical Studies & Analysis](#9-empirical-studies--analysis)
10. [Industrial Deployments](#10-industrial-deployments)

---

## 1. End-to-End Autonomous Software Engineering Agents

These papers present complete agent systems that cover most or all stages of the coding lifecycle (plan → write → execute → debug → test → refine) in realistic repository-level settings.

| # | Paper | Venue | Key Contribution |
|---|-------|-------|-----------------|
| 3 | **OpenHands** | ICLR 2025 | Open platform for AI software developers; CodeAct-based generalist agents interacting via terminals, browsers, editors |
| 10 | **SWE-agent** | NeurIPS 2024 | Introduces Agent-Computer Interfaces (ACIs) tailored for LLM agents in SE; custom shell commands improve issue resolution |
| 7 | **HyperAgent** | ICLR 2025 | Multi-agent generalist system for diverse SE tasks at scale |
| 33 | **RepairAgent** | ICSE 2025 | First fully autonomous agent for program repair; plans and executes multi-step repair without fixed loops |
| 36 | **AutoCodeRover v2** | ICSE 2025 | Project-structure-aware agent; code search + context retrieval for autonomous program improvement on SWE-bench |
| 44 | **Trae Agent** | ASE 2025 | 75.2% on SWE-bench Verified; autonomous debugging, fix implementation, codebase navigation |
| 15 | **Nemotron-CORTEXA** | ICML 2025 | Improved localization + solution diversity for SE agents; >50% improvement over baselines on SWE-bench |
| 39 | **LingmaAgent** | FSE 2025 | Alibaba's industrial agent; comprehensive repository exploration for issue resolution |
| 47 | **SEIDR** | ACM TELO 2025 | Synthesize–Execute–Instruct–Debug–Repair pipeline for fully autonomous programming |

**Key themes:**
- All systems operate on **real GitHub repositories** (not toy benchmarks)
- Most adopt a **loop architecture**: localize → generate patch → validate → refine
- Agent-environment **interface design** is critical (SWE-agent's ACIs, OpenHands' CodeAct)
- Solution diversity and exploration strategies increasingly important (Nemotron-CORTEXA, SWE-Search)

---

## 2. Multi-Agent Frameworks for Code Generation

Papers where multiple specialized LLM agents collaborate, each handling a different role (planner, coder, debugger, tester, reviewer).

| # | Paper | Venue | Architecture |
|---|-------|-------|-------------|
| 11 | **MAGIS** | NeurIPS 2024 | 4 agents: Manager, Repository Custodian, Developer, QA Engineer |
| 19 | **MapCoder** | ACL 2024 | Retrieval agent → Planning agent → Coding agent → Debugging agent (dynamic traversal) |
| 32 | **CodeSim** | NAACL 2025 | Planning + Coding + Debugging agents with simulation-driven verification |
| 45 | **AgentCoder** | Preprint 2024 | Programmer + Test Designer + Test Executor agents in iterative loop |
| 46 | **CodeR** | Preprint 2024 | Multi-agent with pre-defined task graphs for structured issue resolution |
| 41 | **ALMAS** | ASE 2025 | Autonomous multi-agent SE framework for implementation and maintenance |
| 13 | **Lessons Learned** | NeurIPS 2025 | Agents with different specializations learn from each other's mistakes |
| 26 | **Qwen2.5-xCoder** | ACL 2025 | Multi-agent collaboration for multilingual code instruction tuning |
| 28 | **CodeAgent** | EMNLP 2024 | Multi-agent system for code review emulating human collaborative review |
| 49 | **RGD** | Preprint 2024 | Multi-LLM debugger with refinement and generation guidance |

**Key themes:**
- Role specialization consistently outperforms single-agent approaches
- Communication protocols between agents vary: structured task graphs (CodeR), natural language messages (MAGIS), confidence-score routing (MapCoder)
- Trade-off between coordination overhead and task decomposition benefit
- Most frameworks tested on competitive programming OR repository-level tasks, rarely both

---

## 3. Planning & Search Strategies for Code Synthesis

Papers focusing on how agents plan before coding, and how structured search over solution spaces improves generation quality.

| # | Paper | Venue | Strategy |
|---|-------|-------|----------|
| 5 | **Planning in Natural Language** | ICLR 2025 | NL plan search improves diversity → better inference-time scaling |
| 4 | **SWE-Search** | ICLR 2025 | Monte Carlo Tree Search over software agent actions with iterative refinement |
| 31 | **CodeTree** | NAACL 2025 | Unified tree structure exploring strategies, solutions, and debug paths |
| 22 | **LPW (Planning-Driven Programming)** | ACL 2025 | Structured planning workflow before generation |
| 23 | **Interactive Code-Augmented Planning** | ACL 2025 | Control flow and code-structured planning for complex long-horizon tasks |
| 24 | **Tree-of-Code** | ACL 2025 | Self-growing tree for end-to-end generation + execution (extends CodeAct) |
| 9 | **Execution-Guided Within-Prompt Search** | ICLR 2025 | Line-by-line generation with execution annotation, search within expanding prompt |
| 34 | **ROCODE** | ICSE 2025 | Backtracking mechanism + program analysis to overcome auto-regressive limitations |

**Key themes:**
- **Diversity is the bottleneck**: without planning, LLMs repeatedly sample similar incorrect solutions (Planning in NL, ICLR 2025)
- **Tree-based search** dominates: MCTS (SWE-Search), unified trees (CodeTree), self-growing trees (Tree-of-Code)
- Planning-then-code outperforms direct code generation across all benchmarks
- Backtracking mechanisms (ROCODE) address the fundamental limitation of auto-regressive generation

---

## 4. Debugging & Self-Repair Agents

Papers primarily addressing how LLM agents identify and fix errors in generated or existing code.

| # | Paper | Venue | Approach |
|---|-------|-------|----------|
| 2 | **Self-Debugging** | ICLR 2024 | Rubber duck debugging — LLM explains code, identifies bugs without human feedback |
| 12 | **LeDex** | NeurIPS 2024 | Training framework: chain of explanations → refinement; automated data generation |
| 20 | **LDB** | ACL 2024 | Step-by-step runtime execution verification at block granularity (like breakpoints) |
| 16 | **AuPair** | ICML 2025 | Golden example pairs guide self-repair; scales inference-time compute |
| 29 | **Instruct, Not Assist** | EMNLP 2024 | Socratic debugging via hierarchical questioning (pedagogical approach) |
| 30 | **Debug Smarter, Not Harder** | EMNLP 2024 | Agentic error resolution in computational notebooks via interactive exploration |
| 38 | **ReproCopilot** | FSE 2025 | Failure reproduction with dynamic refinement using program analysis + LLM |
| 48 | **ARCS** | Preprint 2025 | Synthesize–execute–repair loop with retrieval-augmented context |

**Key themes:**
- **Explanation before repair** is a recurring winning strategy (Self-Debugging, LeDex, LDB)
- Runtime execution information is underutilized — LDB shows step-by-step verification dramatically helps
- Self-repair can be improved at inference time without fine-tuning (AuPair, ARCS)
- Interactive/exploratory debugging (notebook agents) represents an emerging direction

---

## 5. Execution Feedback & Iterative Refinement

Papers addressing how agents use execution signals (compiler errors, test results, runtime traces) to iteratively improve code.

| # | Paper | Venue | Feedback Mechanism |
|---|-------|-------|-------------------|
| 17 | **RLEF** | ICML 2025 | RL grounding in execution feedback for multi-step tasks |
| 18 | **μCODE** | ICML 2025 | Multi-turn refinement via single-step rewards (avoids hierarchical RL) |
| 21 | **Iterative Refinement with Compiler Feedback** | ACL 2024 | Project-level context refinement using compiler errors |
| 27 | **CodePRM** | ACL 2025 | Process reward model enhanced with execution feedback |
| 14 | **CodeAct** | ICML 2024 | Executable Python code as unified action space — natural execution loop |
| 9 | **Execution-Guided Within-Prompt Search** | ICLR 2025 | Per-line execution annotation guides policy within a single prompt |
| 48 | **ARCS** | Preprint 2025 | Budgeted synthesize–execute–repair loop over frozen model |
| 50 | **Self-Improving Coding Agent** | ICLR 2025 WS | Autonomous self-improvement through practice cycles |

**Key themes:**
- Execution feedback is the **single most impactful signal** for iterative improvement
- Two paradigms: (a) training-time integration (RLEF, LeDex), (b) inference-time loops (ARCS, CodeAct)
- Process reward models (CodePRM) provide finer-grained supervision than outcome-only rewards
- Simple single-step reward shaping (μCODE) can match complex hierarchical RL
- Budget-aware approaches (ARCS) balance quality vs. compute cost

---

## 6. Training Agents: RL & Learning from Feedback

Papers focused on how to train or fine-tune LLMs specifically for agentic code generation behaviors.

| # | Paper | Venue | Training Approach |
|---|-------|-------|-------------------|
| 17 | **RLEF** | ICML 2025 | Reinforcement learning from execution feedback |
| 18 | **μCODE** | ICML 2025 | Single-step RL rewards for multi-turn generation |
| 12 | **LeDex** | NeurIPS 2024 | Automated pipeline for self-debugging training data |
| 8 | **SWE-Gym** | ICLR 2025 / ICML 2025 | Training environment with 2,438 real-world tasks + executable environments |
| 50 | **Self-Improving Coding Agent** | ICLR 2025 WS | Self-improvement without human supervision |
| 13 | **Lessons Learned** | NeurIPS 2025 | Agents learn from each other's mistakes collaboratively |
| 27 | **CodePRM** | ACL 2025 | Process reward model training with execution signals |

**Key themes:**
- **SWE-Gym** fills a critical gap: before 2025, there was no standard training environment for SE agents
- RL from execution feedback (RLEF) represents the frontier of agent training
- Self-play and self-improvement loops (Self-Improving Coding Agent) reduce reliance on human data
- Process rewards > outcome rewards for guiding multi-step reasoning

---

## 7. Testing & Test-Driven Approaches

Papers focusing on how agents generate, use, or leverage tests in the coding process.

| # | Paper | Venue | Approach |
|---|-------|-------|----------|
| 35 | **LLM-Based Test-Driven Interactive Code Generation** | ICSE 2025 | Interactive TDD loop: generate tests → generate code → verify |
| 43 | **Test-Driven Development and LLM-based Code Generation** | ASE 2024 | Studies LLM integration into TDD workflows |
| 38 | **ReproCopilot** | FSE 2025 | Automatic failure-reproducing test generation with dynamic refinement |
| 45 | **AgentCoder** | Preprint 2024 | Dedicated test designer agent in multi-agent loop |
| 28 | **CodeAgent** | EMNLP 2024 | Code review agents assessing quality (implicit testing) |
| 25 | **ProjectEval** | ACL 2025 | Benchmark for automated evaluation of agent-generated projects |

**Key themes:**
- Test generation is increasingly separated from code generation (AgentCoder's dedicated test designer)
- TDD workflows naturally complement agentic loops — tests provide execution feedback
- Failure reproduction (ReproCopilot) is an underexplored but high-impact application
- Project-level evaluation (ProjectEval) moves beyond function-level benchmarks

---

## 8. Benchmarks & Evaluation Frameworks

Papers introducing or extending evaluation infrastructure for agentic code generation.

| # | Paper | Venue | Benchmark |
|---|-------|-------|-----------|
| 1 | **SWE-bench** | ICLR 2024 | 2,294 real GitHub issues/PRs from popular Python repos |
| 6 | **SWE-bench Multimodal** | ICLR 2025 | Extends to visual/UI bugs in JavaScript |
| 8 | **SWE-Gym** | ICLR/ICML 2025 | 2,438 training tasks with executable environments |
| 25 | **ProjectEval** | ACL 2025 | Project-level code generation from user perspective |

**Key themes:**
- **SWE-bench** is the dominant benchmark (used by 15+ papers in this review)
- Evolution: function-level (HumanEval) → repository-level (SWE-bench) → multimodal (SWE-bench M)
- Training environments (SWE-Gym) lag behind evaluation benchmarks
- Need for diverse language/domain coverage (most benchmarks Python-only)

---

## 9. Empirical Studies & Analysis

Papers that study and analyze existing agentic systems rather than proposing new ones.

| # | Paper | Venue | Focus |
|---|-------|-------|-------|
| 37 | **Demystifying LLM-based SE Agents** | FSE 2025 | Large-scale analysis of autonomous agents for code synthesis, repair, test generation |
| 42 | **Empirical Study of Agent Trajectories** | ASE 2025 | Analyzes thought–action–result trajectories of RepairAgent, SWE-agent, etc. |

**Key themes:**
- Agents exhibit consistent failure patterns: over-editing, incorrect localization, premature termination
- Trajectory analysis reveals when agents "get stuck" and need better exploration strategies
- Empirical findings inform design of next-generation agents

---

## 10. Industrial Deployments

Papers from industry demonstrating real-world deployment of agentic coding systems.

| # | Paper | Venue | Company/Product |
|---|-------|-------|-----------------|
| 39 | **LingmaAgent** | FSE 2025 | Alibaba Cloud / TONGYI Lingma IDE |
| 44 | **Trae Agent** | ASE 2025 | ByteDance (top SWE-bench Verified) |
| 40 | **CXXCrafter** | FSE 2025 | Automated C/C++ build systems |

---

## Cross-Cutting Observations

### Architecture Patterns

```
┌─────────────────────────────────────────────────────┐
│            Dominant Agent Architecture               │
├─────────────────────────────────────────────────────┤
│                                                     │
│  ┌──────┐   ┌───────┐   ┌─────────┐   ┌────────┐  │
│  │ Plan │──▶│ Write │──▶│ Execute │──▶│ Verify │  │
│  └──────┘   └───────┘   └─────────┘   └────────┘  │
│      ▲                                     │        │
│      └─────────── Feedback Loop ───────────┘        │
│                                                     │
│  Variants:                                          │
│  • Single-agent loop (SWE-agent, RepairAgent)       │
│  • Multi-agent pipeline (MAGIS, MapCoder, CodeR)    │
│  • Tree-search (SWE-Search, CodeTree, MCTS)         │
│  • RL-trained loop (RLEF, μCODE)                    │
└─────────────────────────────────────────────────────┘
```

### Trends (2024 → 2025)

1. **2024:** Foundational agent architectures (SWE-agent, Self-Debugging, CodeAct) + benchmark establishment (SWE-bench)
2. **2025:** Scaling and optimization — MCTS search, RL training, multi-agent specialization, industrial deployment
3. **Emerging:** Self-improving agents, process reward models, multimodal SE, training environments

### Capability Maturity

| Capability | Maturity | Key Gap |
|-----------|----------|---------|
| **Write** | High | Project-level coherence still limited |
| **Debug** | High | Most studied; many competing approaches |
| **Plan** | Medium | NL planning shown effective but underexplored for complex SE |
| **Execute** | Medium | Environment interfaces improving rapidly |
| **Refine** | Medium | When to stop refining remains open |
| **Test** | Low | Test generation for agents is nascent; mostly uses existing tests |

### Open Problems

1. **Generalization beyond Python** — 90%+ of work targets Python repositories
2. **Long-horizon planning** — most agents handle 1-5 step tasks; real SE requires 10-50+ steps
3. **Multi-file coordination** — agents struggle with cross-file dependencies
4. **Knowing when to stop** — over-editing and premature termination are common failure modes
5. **Cost efficiency** — most SOTA agents require many LLM calls per task ($0.5–$5 per issue)
6. **Safety & correctness guarantees** — no formal verification of agent-generated patches

---

## Paper Index (Alphabetical by First Author/Title)

| ID | Short Title | Venue | Year | Topic Category |
|----|-------------|-------|------|----------------|
| 45 | AgentCoder | Preprint | 2024 | Multi-Agent |
| 41 | ALMAS | ASE 2025 WS | 2025 | Multi-Agent |
| 48 | ARCS | Preprint | 2025 | Execution Feedback |
| 16 | AuPair | ICML 2025 | 2025 | Debugging |
| 36 | AutoCodeRover v2 | ICSE 2025 | 2025 | End-to-End Agent |
| 14 | CodeAct | ICML 2024 | 2024 | Execution Feedback |
| 28 | CodeAgent | EMNLP 2024 | 2024 | Multi-Agent |
| 27 | CodePRM | ACL 2025 | 2025 | Training |
| 46 | CodeR | Preprint | 2024 | Multi-Agent |
| 32 | CodeSim | NAACL 2025 | 2025 | Multi-Agent |
| 31 | CodeTree | NAACL 2025 | 2025 | Planning & Search |
| 40 | CXXCrafter | FSE 2025 | 2025 | Industrial |
| 30 | Debug Smarter | EMNLP 2024 | 2024 | Debugging |
| 37 | Demystifying SE Agents | FSE 2025 | 2025 | Empirical Study |
| 42 | Empirical Trajectories | ASE 2025 | 2025 | Empirical Study |
| 9 | Exec-Guided Search | ICLR 2025 | 2025 | Planning & Search |
| 7 | HyperAgent | ICLR 2025 | 2025 | End-to-End Agent |
| 29 | Instruct Not Assist | EMNLP 2024 | 2024 | Debugging |
| 23 | Interactive Planning | ACL 2025 | 2025 | Planning & Search |
| 21 | Iterative Refinement | ACL 2024 | 2024 | Execution Feedback |
| 20 | LDB | ACL 2024 | 2024 | Debugging |
| 12 | LeDex | NeurIPS 2024 | 2024 | Debugging / Training |
| 13 | Lessons Learned | NeurIPS 2025 | 2025 | Multi-Agent / Training |
| 39 | LingmaAgent | FSE 2025 | 2025 | Industrial |
| 22 | LPW | ACL 2025 | 2025 | Planning & Search |
| 11 | MAGIS | NeurIPS 2024 | 2024 | Multi-Agent |
| 19 | MapCoder | ACL 2024 | 2024 | Multi-Agent |
| 18 | μCODE | ICML 2025 | 2025 | Training |
| 15 | Nemotron-CORTEXA | ICML 2025 | 2025 | End-to-End Agent |
| 3 | OpenHands | ICLR 2025 | 2025 | End-to-End Agent |
| 5 | Planning in NL | ICLR 2025 | 2025 | Planning & Search |
| 25 | ProjectEval | ACL 2025 | 2025 | Benchmark |
| 26 | Qwen2.5-xCoder | ACL 2025 | 2025 | Multi-Agent |
| 33 | RepairAgent | ICSE 2025 | 2025 | End-to-End Agent |
| 38 | ReproCopilot | FSE 2025 | 2025 | Testing |
| 49 | RGD | Preprint | 2024 | Debugging |
| 17 | RLEF | ICML 2025 | 2025 | Training |
| 34 | ROCODE | ICSE 2025 | 2025 | Planning & Search |
| 47 | SEIDR | ACM TELO 2025 | 2025 | End-to-End Agent |
| 2 | Self-Debugging | ICLR 2024 | 2024 | Debugging |
| 50 | Self-Improving Agent | ICLR 2025 WS | 2025 | Training |
| 1 | SWE-bench | ICLR 2024 | 2024 | Benchmark |
| 10 | SWE-agent | NeurIPS 2024 | 2024 | End-to-End Agent |
| 6 | SWE-bench Multimodal | ICLR 2025 | 2025 | Benchmark |
| 8 | SWE-Gym | ICLR/ICML 2025 | 2025 | Benchmark / Training |
| 4 | SWE-Search | ICLR 2025 | 2025 | Planning & Search |
| 43 | TDD + LLM | ASE 2024 | 2024 | Testing |
| 35 | Test-Driven Code Gen | ICSE 2025 | 2025 | Testing |
| 44 | Trae Agent | ASE 2025 WS | 2025 | End-to-End / Industrial |
| 24 | Tree-of-Code | ACL 2025 | 2025 | Planning & Search |

---

## Suggested Reading Order

For a seminar, the following reading order provides a coherent narrative:

### Foundation (read first)
1. **SWE-bench** [1] — the benchmark that defines the field
2. **SWE-agent** [10] — foundational agent architecture
3. **CodeAct** [14] — code-as-action paradigm
4. **Self-Debugging** [2] — seminal debugging approach

### Core Agent Systems
5. **OpenHands** [3] — most complete open platform
6. **RepairAgent** [33] — autonomous repair without fixed loops
7. **MapCoder** [19] — multi-agent competitive programming

### Advanced Techniques
8. **SWE-Search** [4] — MCTS for SE agents
9. **Planning in NL** [5] — why planning matters
10. **RLEF** [17] — training agents with RL

### Cutting Edge (2025)
11. **Nemotron-CORTEXA** [15] — SOTA on SWE-bench
12. **Demystifying SE Agents** [37] — what works and what doesn't
13. **SWE-Gym** [8] — training infrastructure

---

*Last updated: 2025-05-16*
