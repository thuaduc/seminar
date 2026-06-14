# Agentic AI for Code Generation — Seminar Report

TUM SS26 seminar report surveying autonomous AI agents for code generation.

## Framing

The report is organised around a **vibe coding pipeline**: Intent → Plan → Generate → Execute → Refine. Each main section zooms into a stage of this pipeline.

## Structure (report/main.tex)

1. **Introduction** — Motivation, background, scope
2. **The Vibe Coding Pipeline** — Pipeline definition, mapping to sections
3. **End-to-End Autonomous SE Agents** — Full-pipeline systems (SWE-agent, Agentless, AutoCodeRover, OpenHands, Devin) *(written)*
4. **Planning & Search Strategies** — Task decomposition, search over programs
5. **Multi-Agent Frameworks** — Role-based collaboration for generation & validation
6. **Execution Feedback & Iterative Refinement** — Feedback loops, self-repair, convergence
7. **Discussion** — Patterns/trends, evaluation landscape, context bottleneck, open challenges
8. **Conclusion**

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
- §5 (Multi-Agent Frameworks): drafted with citations and comparison table
- §6–§8: structure and placeholder descriptions only
