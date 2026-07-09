# Agentic AI for Code Generation — Seminar Report

TUM SS26 seminar report surveying autonomous AI agents for code generation.

## Framing

The report is organised around a **vibe coding pipeline**: Intent → Plan → Generate → Execute → Refine. Each main section zooms into a stage of this pipeline.

## Structure (report/main.tex)

1. **Introduction** — Motivation, background, scope
2. **The Vibe Coding Pipeline** — Pipeline definition, mapping to sections
3. **End-to-End Autonomous SE Agents** — Full-pipeline systems (SWE-agent, Agentless, AutoCodeRover, OpenHands, Devin) *(written)*
4. **Planning & Search Strategies** — Task decomposition, search over programs
5. **Execution Feedback & Iterative Refinement** — Feedback loops, self-repair, convergence
6. **Discussion** — Patterns/trends, evaluation landscape, context bottleneck, open challenges
7. **Conclusion**

*(Multi-Agent Frameworks section archived in `report/archive/multi-agent-frameworks.tex`.)*

## Files

- `report/main.tex` — Main LaTeX source (TUM template, twocolumn)
- `report/references.bib` — Bibliography
- `report/tum/` — TUM class files and resources

## Workflow

- When asked to write report content, edit the LaTeX source only; do not rebuild the PDF unless explicitly requested.

## Status

- §2 (Vibe Coding Pipeline): structure with pipeline overview figure
- §3 (End-to-End Agents): drafted with citations and comparison table
- §4 (Planning & Search Strategies): drafted with citations and 4 new visualizations (plan shapes, localisation pipeline, candidate selection, plan-generate table)
- §5 (Execution Feedback & Iterative Refinement): drafted with citations
- §6 (Discussion): drafted in full — consolidated patterns, evaluation landscape (SWE-bench Pro/Multilingual, reward hacking, SWE-Effi), context bottleneck, open challenges (long-horizon, ambiguity, cost, security), SE-practice implications; grounded in 2025–2026 sources
- §7 (Conclusion): drafted in full — synthesis across three fronts + future directions
- New 2025–2026 citations added to references.bib (SWE-bench Pro, Multilingual, Veracode GenAI security, SusVibes, SWE-Effi, context-management, reward-hacking)
- Multi-Agent Frameworks: archived (see `report/archive/multi-agent-frameworks.tex`)
