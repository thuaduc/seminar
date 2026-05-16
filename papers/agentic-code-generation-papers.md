# Agentic AI for Code Generation — Paper Collection (2024–2025)

Papers from ICML / NeurIPS / ICLR / ACL / EMNLP / NAACL / ICSE / FSE / ASE on LLM agents that plan, write, execute, debug, test, and/or refine code in realistic software-engineering environments.

---

## ICLR

### 1. SWE-bench: Can Language Models Resolve Real-world GitHub Issues?
- **Venue:** ICLR 2024 (Conference)
- **Authors:** Carlos E. Jimenez et al.
- **Summary:** Introduces SWE-bench, an evaluation framework with 2,294 SE problems from real GitHub issues/PRs. Evaluates LLM ability to resolve real-world issues requiring long-context reasoning.
- **Benchmark:** SWE-bench
- **URL:** https://proceedings.iclr.cc/paper_files/paper/2024/hash/edac78c3e300629acfe6cbe9ca88fb84-Abstract-Conference.html
- **arXiv:** https://arxiv.org/abs/2310.06770

### 2. Teaching Large Language Models to Self-Debug
- **Venue:** ICLR 2024 (Conference)
- **Authors:** Xinyun Chen, Maxwell Lin, Nathanael Schärli, Denny Zhou
- **Summary:** Proposes Self-Debugging — teaching LLMs to debug predicted programs via rubber duck debugging, using execution results and code explanations without human feedback.
- **Capabilities:** Debug, Refine
- **URL:** https://proceedings.iclr.cc/paper_files/paper/2024/hash/2460396f2d0d421885997dd1612ac56b-Abstract-Conference.html
- **arXiv:** https://arxiv.org/abs/2304.05128

### 3. OpenHands: An Open Platform for AI Software Developers as Generalist Agents
- **Venue:** ICLR 2025 (Conference)
- **Authors:** Xingyao Wang et al.
- **Summary:** Platform for building AI agents that interact like human developers — writing code, using terminals, browsing web. Uses CodeAct paradigm. Achieves strong SWE-bench results.
- **Capabilities:** Plan, Write, Execute, Debug, Test, Refine
- **URL:** https://proceedings.iclr.cc/paper_files/paper/2025/hash/a4b6ad6b48850c0c331d1259fc66a69c-Abstract-Conference.html
- **arXiv:** https://arxiv.org/abs/2407.16741

### 4. SWE-Search: Enhancing Software Agents with Monte Carlo Tree Search and Iterative Refinement
- **Venue:** ICLR 2025 (Poster)
- **Authors:** Antonis Antoniades, Albert Örwall, Kexun Zhang, Yuxi Xie, Anirudh Goyal, William Wang
- **Summary:** Applies MCTS to software engineering agents for structured exploration of solution space with iterative refinement. Improves SWE-bench performance.
- **Capabilities:** Plan, Execute, Refine
- **URL:** https://openreview.net/forum?id=G7sIFXugTX

### 5. Planning in Natural Language Improves LLM Search for Code Generation
- **Venue:** ICLR 2025 (Conference)
- **Authors:** Evan Wang, Federico Cassano, Catherine Wu et al.
- **Summary:** Demonstrates that searching over natural language plans before code generation improves diversity and correctness. Addresses inference-time compute scaling.
- **Capabilities:** Plan, Write
- **URL:** https://proceedings.iclr.cc/paper_files/paper/2025/hash/071a637d41ea290ac4360818a8323f33-Abstract-Conference.html
- **arXiv:** https://arxiv.org/abs/2409.03733

### 6. SWE-bench Multimodal: Do AI Systems Generalize to Visual Coding Tasks?
- **Venue:** ICLR 2025 (Conference)
- **Authors:** John Yang et al.
- **Summary:** Extends SWE-bench to multimodal settings evaluating AI agents on visual/user-facing JavaScript bugs.
- **Capabilities:** Debug, Refine
- **URL:** https://openreview.net/pdf/5d4924b3ed846d3ff1985182b2a8851d10c4f3ef.pdf

### 7. HyperAgent: Generalist Software Engineering Agents to Solve Coding Tasks at Scale
- **Venue:** ICLR 2025 (Submission)
- **Authors:** Multiple
- **Summary:** Multi-agent system for generalist SE tasks at scale using LLMs.
- **Capabilities:** Plan, Write, Execute, Debug
- **URL:** https://openreview.net/forum?id=PZf4RsPMBG

### 8. Training Software Engineering Agents and Verifiers with SWE-Gym
- **Venue:** ICLR 2025 / ICML 2025
- **Authors:** Multiple
- **Summary:** First environment for training real-world SWE agents. Contains 2,438 Python task instances with executable environments and unit tests. Used to train agents achieving strong SWE-bench results.
- **Capabilities:** Write, Execute, Debug, Test
- **URL:** https://openreview.net/forum?id=Cq1BNvHx74

### 9. Execution-Guided Within-Prompt Search for Programming-by-Example
- **Venue:** ICLR 2025 (Conference)
- **Authors:** Verbruggen et al.
- **Summary:** Uses LLM as policy generating code lines, executing each and annotating with results, searching within a single expanding prompt.
- **Capabilities:** Write, Execute, Refine
- **URL:** https://proceedings.iclr.cc/paper_files/paper/2025/hash/98e967164ae2f6811b975d686dece3eb-Abstract-Conference.html

---

## NeurIPS

### 10. SWE-agent: Agent-Computer Interfaces Enable Automated Software Engineering
- **Venue:** NeurIPS 2024 (Poster)
- **Authors:** John Yang, Carlos E. Jimenez et al.
- **Summary:** Introduces agent-computer interfaces (ACIs) designed for LLM agents, enabling automated software engineering. Demonstrates significant performance improvements on SWE-bench via custom interfaces.
- **Capabilities:** Plan, Write, Execute, Debug
- **URL:** https://proceedings.neurips.cc/paper_files/paper/2024/hash/5a7c947568c1b1328ccc5230172e1e7c-Abstract-Conference.html

### 11. MAGIS: LLM-Based Multi-Agent Framework for GitHub Issue Resolution
- **Venue:** NeurIPS 2024 (Poster)
- **Authors:** Wei Tao et al.
- **Summary:** Four-agent framework (Manager, Repository Custodian, Developer, QA Engineer) for GitHub issue resolution. Leverages collaboration in planning and coding.
- **Capabilities:** Plan, Write, Debug, Test
- **URL:** https://openreview.net/forum?id=qevq3FZ63J
- **arXiv:** https://arxiv.org/abs/2403.17927

### 12. LeDex: Training LLMs to Better Self-Debug and Explain Code
- **Venue:** NeurIPS 2024 (Conference)
- **Authors:** Nan Jiang, Xiaopeng Li, Shiqi Wang et al.
- **Summary:** Training framework that improves self-debugging via chain of explanations on wrong code followed by refinement. Generates training data automatically.
- **Capabilities:** Debug, Refine
- **URL:** https://proceedings.neurips.cc/paper_files/paper/2024/file/3ea832724870c700f0a03c665572e2a9-Paper-Conference.pdf
- **arXiv:** https://arxiv.org/abs/2405.18649

### 13. Lessons Learned: A Multi-Agent Framework for Code LLMs to Learn and Improve
- **Venue:** NeurIPS 2025 (Poster)
- **Authors:** Yuanzhe Liu, Ryan Deng, Tim Kaler, Xuhao Chen, Charles Leiserson, Yao Ma, Jie Chen
- **Summary:** Multi-agent framework where LLMs with different specializations collaboratively solve coding problems by learning from each other's lessons.
- **Capabilities:** Write, Debug, Refine
- **URL:** https://openreview.net/forum?id=pY65QWWFlm

---

## ICML

### 14. CodeAct: Executable Code Actions Elicit Better LLM Agents
- **Venue:** ICML 2024 (Conference)
- **Authors:** Xingyao Wang et al.
- **Summary:** Proposes using executable Python code as the unified action space for LLM agents (instead of JSON/text). Improves agent performance by enabling composition, control flow, and data manipulation.
- **Capabilities:** Plan, Write, Execute
- **URL:** https://proceedings.mlr.press/v235/wang24h.html
- **arXiv:** https://arxiv.org/abs/2402.01030

### 15. Nemotron-CORTEXA: Enhancing LLM Agents for Software Engineering Tasks
- **Venue:** ICML 2025 (Conference)
- **Authors:** Sohrabizadeh et al.
- **Summary:** Enhances LLM agents for SE tasks (SWE-bench) via improved fault localization and solution diversity. >50% improvement over baseline.
- **Capabilities:** Plan, Write, Debug, Refine
- **URL:** https://proceedings.mlr.press/v267/sohrabizadeh25a.html

### 16. AuPair: Golden Example Pairs for Code Repair
- **Venue:** ICML 2025 (Poster)
- **Authors:** Aditi Mavalankar, Hassan Mansoor, Zita Marinho, Mariia Samsikova, Tom Schaul
- **Summary:** Scales inference-time compute for self-repair using golden example pairs. Given an initial flawed response, the LLM corrects its mistake guided by example repair pairs.
- **Capabilities:** Debug, Refine
- **URL:** https://icml.cc/virtual/2025/poster/45813

### 17. RLEF: Grounding Code LLMs in Execution Feedback with Reinforcement Learning
- **Venue:** ICML 2025 (Conference)
- **Authors:** Gehring et al.
- **Summary:** Uses RL to ground LLM agents in execution feedback for multi-step coding tasks. Agents learn from test results and error messages to improve iteratively.
- **Capabilities:** Write, Execute, Debug, Refine
- **URL:** https://mlanthology.org/icml/2025/gehring2025icml-rlef/
- **arXiv:** https://arxiv.org/abs/2410.02089

### 18. Multi-Turn Code Generation Through Single-Step Rewards (μCODE)
- **Venue:** ICML 2025 (Conference)
- **Authors:** Jain et al.
- **Summary:** Solves multi-turn code generation using single-step rewards, avoiding complex hierarchical RL. Simple and scalable approach to iterative refinement.
- **Capabilities:** Write, Execute, Refine
- **URL:** https://mlanthology.org/icml/2025/jain2025icml-multiturn/

---

## ACL

### 19. MapCoder: Multi-Agent Code Generation for Competitive Problem Solving
- **Venue:** ACL 2024 (Long Paper)
- **Authors:** Md. Ashraful Islam, Mohammed Eunus Ali, Md Rizwan Parvez
- **Summary:** Multi-agent framework with retrieval, planning, coding, and debugging agents. Uses dynamic tree traversal based on confidence scores of generated plans.
- **Capabilities:** Plan, Write, Debug
- **Benchmarks:** HumanEval, MBPP, APPS, CodeContests, xCodeEval
- **URL:** https://aclanthology.org/2024.acl-long.269/
- **arXiv:** https://arxiv.org/abs/2405.11403

### 20. LDB: Debug like a Human — A Large Language Model Debugger via Verifying Runtime Execution Step by Step
- **Venue:** ACL 2024 (Findings)
- **Authors:** Li Zhong, Zilong Wang, Jingbo Shang
- **Summary:** Debugging framework that verifies runtime execution step-by-step at block granularity, mimicking how human developers debug with breakpoints.
- **Capabilities:** Debug, Refine
- **URL:** https://aclanthology.org/2024.findings-acl.49/
- **arXiv:** https://arxiv.org/abs/2402.16906

### 21. Iterative Refinement of Project-Level Code Context for Precise Code Generation with Compiler Feedback
- **Venue:** ACL 2024 (Findings)
- **Authors:** Zhangqian Bi et al.
- **Summary:** Iteratively refines project-level code context using compiler feedback to improve code generation accuracy for project-specific APIs and structures.
- **Capabilities:** Write, Execute, Refine
- **URL:** https://aclanthology.org/2024.findings-acl.138/

### 22. Planning-Driven Programming: A Large Language Model Programming Workflow (LPW)
- **Venue:** ACL 2025 (Long Paper)
- **Authors:** Multiple
- **Summary:** LLM programming workflow designed to improve code generation accuracy through structured planning before code generation.
- **Capabilities:** Plan, Write
- **URL:** http://www.aclanthology.org/2025.acl-long.621/

### 23. Interactive and Expressive Code-Augmented Planning with Large Language Models
- **Venue:** ACL 2025 (Long Paper)
- **Authors:** Multiple
- **Summary:** Structures LLM outputs using control flow and code for complex planning tasks. Addresses error-prone nature of code-based approaches for ambiguous/unstructured problems.
- **Capabilities:** Plan, Write, Execute
- **URL:** http://aclanthology.org/2025.acl-long.994/

### 24. Tree-of-Code: A Self-Growing Tree Framework for End-to-End Code Generation and Execution
- **Venue:** ACL 2025 (Findings)
- **Authors:** Multiple
- **Summary:** Builds on CodeAct with a tree-structured approach to code generation and execution. Addresses greedy sequential generation issues.
- **Capabilities:** Plan, Write, Execute
- **URL:** http://www.aclanthology.org/2025.findings-acl.509/

### 25. ProjectEval: A Benchmark for Programming Agents Automated Evaluation on Project-Level Code Generation
- **Venue:** ACL 2025 (Findings)
- **Authors:** Multiple
- **Summary:** Benchmark for evaluating LLM agents on project-level code generation from user perspective with explainable results.
- **Capabilities:** Write, Test (evaluation focus)
- **URL:** https://aclanthology.org/2025.findings-acl.1036/

### 26. Qwen2.5-xCoder: Multi-Agent Collaboration for Multilingual Code Instruction Tuning
- **Venue:** ACL 2025 (Long Paper)
- **Authors:** Multiple
- **Summary:** Multi-agent collaboration for code instruction tuning across programming languages. Bridges knowledge transfer among different languages.
- **Capabilities:** Write, Refine
- **URL:** https://www.aclanthology.org/2025.acl-long.642/

### 27. CodePRM: Execution Feedback-enhanced Process Reward Model for Code Generation
- **Venue:** ACL 2025 (Findings)
- **Authors:** Multiple
- **Summary:** Process reward model enhanced with execution feedback to supervise code generation thought process.
- **Capabilities:** Write, Execute, Refine
- **URL:** http://aclanthology.org/2025.findings-acl.428/

---

## EMNLP

### 28. CodeAgent: Autonomous Communicative Agents for Code Review
- **Venue:** EMNLP 2024 (Main Conference)
- **Authors:** Xunzhu Tang, Kisub Kim, Yewei Song et al.
- **Summary:** Multi-agent LLM system for code review that emulates collaborative human review process. Agents communicate to assess code quality.
- **Capabilities:** Plan, Debug, Test (review-focused)
- **URL:** http://aclanthology.org/2024.emnlp-main.632/

### 29. Instruct, Not Assist: LLM-based Multi-Turn Planning and Hierarchical Questioning for Socratic Code Debugging
- **Venue:** EMNLP 2024 (Findings)
- **Authors:** Priyanka Kargupta, Ishika Agarwal, Dilek Hakkani Tur, Jiawei Han
- **Summary:** Multi-turn planning with hierarchical questioning for code debugging. LLM acts as instructor rather than giving direct solutions.
- **Capabilities:** Plan, Debug
- **URL:** https://aclanthology.org/2024.findings-emnlp.553/

### 30. Debug Smarter, Not Harder: AI Agents for Error Resolution in Computational Notebooks
- **Venue:** EMNLP 2024 (Demo)
- **Authors:** Multiple
- **Summary:** Agentic system for error resolution in computational notebooks. Explores notebook environment interactively like a human user.
- **Capabilities:** Execute, Debug, Refine
- **URL:** https://aclanthology.org/anthology-files/pdf/emnlp/2024.emnlp-demo.38.pdf

---

## NAACL

### 31. CodeTree: Agent-guided Tree Search for Code Generation with Large Language Models
- **Venue:** NAACL 2025 (Long Paper)
- **Authors:** Multiple
- **Summary:** Framework using unified tree structure to explore different coding strategies, generate solutions, and iteratively debug. Addresses multi-stage planning, generating, and debugging.
- **Capabilities:** Plan, Write, Debug, Refine
- **URL:** http://www.aclanthology.org/2025.naacl-long.189/

### 32. CodeSim: Multi-Agent Code Generation and Problem Solving through Simulation-Driven Planning and Debugging
- **Venue:** NAACL 2025 (Findings)
- **Authors:** Md. Ashraful Islam, Mohammed Eunus Ali, Md Rizwan Parvez
- **Summary:** Multi-agent framework addressing planning, coding, and debugging through human-like visual simulation. Agents verify algorithms through simulation before finalizing.
- **Capabilities:** Plan, Write, Execute, Debug
- **URL:** https://aclanthology.org/2025.findings-naacl.285/
- **arXiv:** https://arxiv.org/abs/2502.05664

---

## ICSE

### 33. RepairAgent: An Autonomous, LLM-Based Agent for Program Repair
- **Venue:** ICSE 2025 (Research Track)
- **Authors:** Islem Bouzenia, Premkumar Devanbu, Michael Pradel
- **Summary:** First autonomous agent-based approach to program repair. LLM autonomously plans and executes repair strategies, unlike fixed-prompt or fixed-loop approaches.
- **Capabilities:** Plan, Debug, Refine
- **URL:** https://conf.researchr.org/details/icse-2025/icse-2025-research-track/160/RepairAgent-An-Autonomous-LLM-Based-Agent-for-Program-Repair
- **arXiv:** https://arxiv.org/abs/2403.17134

### 34. ROCODE: Integrating Backtracking Mechanism and Program Analysis in Large Language Models for Code Generation
- **Venue:** ICSE 2025 (Research Track)
- **Authors:** Multiple
- **Summary:** Integrates backtracking mechanism with program analysis into LLMs to overcome auto-regressive limitations in code generation.
- **Capabilities:** Write, Debug, Refine
- **URL:** https://conf.researchr.org/details/icse-2025/icse-2025-research-track/100/ROCODE-Integrating-Backtracking-Mechanism-and-Program-Analysis-in-Large-Language-Mod

### 35. LLM-Based Test-Driven Interactive Code Generation
- **Venue:** ICSE 2025 (Journal-first)
- **Authors:** Multiple
- **Summary:** Test-driven interactive code generation using LLMs. Combines test generation with code generation in an interactive loop.
- **Capabilities:** Write, Test, Refine
- **URL:** https://conf.researchr.org/details/icse-2025/icse-2025-journal-first-papers/82/LLM-Based-Test-Driven-Interactive-Code-Generation-User-Study-and-Empirical-Evaluatio

### 36. AutoCodeRover v2: Autonomous Program Improvement (ICSE 2025 extension)
- **Venue:** ICSE 2025 (related work; original at ISSTA 2024)
- **Authors:** Multiple (NUS)
- **Summary:** Project-structure-aware autonomous agent for program improvement. >50% improvement over v1 on full SWE-Bench (2294 issues). Uses code search and context retrieval.
- **Capabilities:** Plan, Write, Debug, Test
- **URL:** https://abhikrc.com/pdf/ICSE25.pdf
- **arXiv:** https://arxiv.org/abs/2404.05427

---

## FSE

### 37. Demystifying LLM-based Software Engineering Agents
- **Venue:** FSE 2025 (Research Papers)
- **Authors:** Chunqiu Steven Xia, Yinlin Deng, Soren Dunn, Lingming Zhang
- **Summary:** Large-scale empirical study of LLM-based SE agents. Analyzes autonomous agents for code synthesis, program repair, and test generation.
- **Capabilities:** Analysis of Plan, Write, Execute, Debug, Test, Refine
- **URL:** https://conf.researchr.org/details/fse-2025/fse-2025-research-papers/85/Demystifying-LLM-based-Software-Engineering-Agents
- **PDF:** https://lingming.cs.illinois.edu/publications/fse2025.pdf

### 38. ReproCopilot: LLM-Driven Failure Reproduction with Dynamic Refinement
- **Venue:** FSE 2025 (Research Papers)
- **Authors:** Multiple
- **Summary:** Uses program analysis + LLM to generate failure-reproducing tests with dynamic refinement loop.
- **Capabilities:** Execute, Debug, Test, Refine
- **URL:** https://conf.researchr.org/details/fse-2025/fse-2025-research-papers/114/ReproCopilot-LLM-Driven-Failure-Reproduction-with-Dynamic-Refinement

### 39. Alibaba LingmaAgent: Improving Automated Issue Resolution via Comprehensive Repository Exploration
- **Venue:** FSE 2025 (Industry Papers)
- **Authors:** Multiple (Alibaba Cloud)
- **Summary:** Industrial agent for automated issue resolution that comprehensively explores software repositories. Deployed in TONGYI Lingma IDE assistant.
- **Capabilities:** Plan, Write, Debug, Refine
- **URL:** https://conf.researchr.org/details/fse-2025/fse-2025-industry-papers/21/Alibaba-LingmaAgent-Improving-Automated-Issue-Resolution-via-Comprehensive-Repositor

### 40. CXXCrafter: An LLM-Based Agent for Automated C/C++ Open Source Software Building
- **Venue:** FSE 2025 (Research Papers)
- **Authors:** Multiple
- **Summary:** Agent for automating the complex build process of C/C++ projects, handling configuration and dependency resolution.
- **Capabilities:** Plan, Execute
- **URL:** https://conf.researchr.org/details/fse-2025/fse-2025-research-papers/78/CXXCrafter-An-LLM-Based-Agent-for-Automated-C-C-Open-Source-Software-Building

---

## ASE

### 41. ALMAS: An Autonomous LLM-based Multi-Agent Software Engineering Framework
- **Venue:** ASE 2025 (Workshop: MAS-GAIN)
- **Authors:** Vali Tawosi, Keshav Ramani, Salwa Alamir, Xiaomo Liu
- **Summary:** Multi-agent framework for software development automation including code implementation and maintenance.
- **Capabilities:** Plan, Write, Debug
- **URL:** https://conf.researchr.org/details/ase-2025/mas-gain-2025-papers/3/ALMAS-an-Autonomous-LLM-based-Multi-Agent-Software-Engineering-Framework

### 42. Empirical Study of LLM-based SE Agent Trajectories
- **Venue:** ASE 2025 (Research)
- **Authors:** Multiple (Software Lab)
- **Summary:** Large-scale empirical study of thought-action-result trajectories of RepairAgent, SWE-agent, and other SOTA agents. Analyzes how agents reason, invoke tools, and refine solutions.
- **Capabilities:** Analysis of agent behaviors
- **URL:** https://software-lab.org/publications/ase2025_trajectories.pdf

### 43. Test-Driven Development and LLM-based Code Generation
- **Venue:** ASE 2024 (Research Papers)
- **Authors:** Noble Saji Mathews, Mei Nagappan
- **Summary:** Studies how LLMs can be integrated into test-driven development workflows for code generation.
- **Capabilities:** Write, Test
- **URL:** https://conf.researchr.org/details/ase-2024/ase-2024-research/127/Test-Driven-Development-and-LLM-based-Code-Generation

### 44. Trae Agent: SOTA Open-source AI Coding Agent for SWE-bench
- **Venue:** ASE 2025 (Workshop: AgenticSE)
- **Authors:** Multiple
- **Summary:** Achieved 75.2% on SWE-bench Verified. Demonstrates autonomous debugging, implementing fixes, and navigating complex codebases.
- **Capabilities:** Plan, Write, Execute, Debug, Refine
- **URL:** https://conf.researchr.org/details/ase-2025/agenticse-2025-papers/9/Trae-Agent-SOTA-Open-source-AI-Coding-Agent-for-SWE-bench

---

## Additional Relevant Papers (2024+, preprints with venue associations)

### 45. AgentCoder: Multi-Agent-based Code Generation with Iterative Testing and Optimisation
- **Venue:** Preprint (2024, under review)
- **Authors:** Dong Huang, Qingwen Bu, Jie M. Zhang, Michael Luck, Heming Cui
- **Summary:** Multi-agent system with programmer, test designer, and test executor agents. Iterative testing and optimization loop.
- **Capabilities:** Write, Test, Debug, Refine
- **arXiv:** https://arxiv.org/abs/2312.13010

### 46. CodeR: Issue Resolving with Multi-Agent and Task Graphs
- **Venue:** Preprint (2024)
- **Authors:** Multiple
- **Summary:** Multi-agent framework with pre-defined task graphs for GitHub issue resolution. Structures the resolution process into explicit workflow stages.
- **Capabilities:** Plan, Write, Debug, Test
- **arXiv:** https://arxiv.org/abs/2406.01304

### 47. SEIDR: Fully Autonomous Programming using Iterative Multi-Agent Debugging
- **Venue:** ACM TELO 2025
- **Authors:** Anastasiia Grishina, Vadim Liventsev, Aki Härmä, Leon Moonen
- **Summary:** Synthesize, Execute, Instruct, Debug, and Repair (SEIDR) framework for fully autonomous multi-agent programming.
- **Capabilities:** Write, Execute, Debug, Refine
- **arXiv:** https://arxiv.org/abs/2503.07693

### 48. ARCS: Agentic Retrieval-Augmented Code Synthesis with Iterative Refinement
- **Venue:** Preprint (2025)
- **Authors:** Multiple
- **Summary:** Synthesize–execute–repair loop with retrieval-augmented generation. Reduces hallucination via retrieval-before-generation design.
- **Capabilities:** Write, Execute, Debug, Refine
- **arXiv:** https://arxiv.org/abs/2504.20434

### 49. RGD: Multi-LLM Based Agent Debugger via Refinement and Generation Guidance
- **Venue:** Preprint (2024)
- **Authors:** Multiple
- **Summary:** Multi-LLM debugger using refinement and generation guidance for code accuracy.
- **Capabilities:** Debug, Refine
- **arXiv:** https://arxiv.org/abs/2410.01242

### 50. A Self-Improving Coding Agent
- **Venue:** ICLR 2025 (Workshop Oral)
- **Authors:** Maxime Robeyns, Martin Szummer, Laurence Aitchison
- **Summary:** Agent that self-improves its coding ability through autonomous practice without human supervision.
- **Capabilities:** Write, Execute, Debug, Refine
- **URL:** https://iclr.cc/virtual/2025/10000470

---

## Summary by Conference

| Conference | # Papers Found | Years |
|------------|---------------|-------|
| ICLR | 9 | 2024–2025 |
| NeurIPS | 4 | 2024–2025 |
| ICML | 5 | 2024–2025 |
| ACL | 9 | 2024–2025 |
| EMNLP | 3 | 2024 |
| NAACL | 2 | 2025 |
| ICSE | 4 | 2024–2025 |
| FSE | 4 | 2025 |
| ASE | 4 | 2024–2025 |

## Capability Coverage

| Capability | Papers covering it |
|------------|-------------------|
| **Plan** | 1,3,4,5,7,11,14,15,19,22,23,24,29,31,32,33,36,37,39,40,41,44,46 |
| **Write** | 3,5,8,9,11,13,14,15,17,18,19,21,22,23,24,25,26,27,31,32,34,35,36,37,39,41,43,44,45,46,47,48,50 |
| **Execute** | 3,8,9,14,17,18,21,23,24,27,30,32,40,44,47,48,50 |
| **Debug** | 2,3,4,6,7,8,10,11,12,13,15,16,17,19,20,28,29,30,31,32,33,34,36,37,38,39,41,42,44,45,46,47,48,49,50 |
| **Test** | 3,8,11,25,28,35,36,38,43,44,45,46 |
| **Refine** | 2,3,4,5,6,9,12,13,15,16,17,18,21,26,27,30,31,33,34,35,37,38,39,44,45,47,48,49,50 |
