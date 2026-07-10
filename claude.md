# Agentic AI for Code Generation — Seminar Report

TUM SS26 seminar report surveying autonomous AI agents for code generation.

## Framing

The report is organised around a five-stage **agent pipeline**: Intent → Plan → Generate → Execute → Refine.

The pipeline is a lens, not an architecture. Its argumentative job is to make one question askable of every system at once: *at each stage, what can tell the model it is wrong?* Read as a control system, intent capture is the uncontrolled input, generation is the plant, execution is the oracle, and planning and refinement are the control layer.

The central thesis: planning and refinement form a control layer over generation, but only where a signal the generator did not produce can contradict it. Refinement mechanisms sort onto a three-position axis — **authoritative oracle** (test runner, compiler, human; can surprise the model) / **learned or generated proxy** (trained critic, generated tests; carries information but cannot surprise) / **same-distribution self-critique** (can only reorganise what the model believes). The pipeline's two weak stages are the two with no oracle: intent capture, and test-based validation.

## Structure (report/main.tex)

1. **Introduction** — Motivation, background, scope
2. **The Agent Pipeline** — Five stages, control roles, mapping to sections
3. **End-to-End Autonomous SE Agents** — SWE-agent, Agentless, AutoCodeRover, OpenHands, Devin
4. **Planning & Search Strategies** — Intent capture, plan representations, search, plan–generate interface
5. **Execution Feedback & Iterative Refinement** — Signals, mechanisms, limits of self-correction, fault localisation, convergence
6. **Discussion** — Consolidated patterns, evaluation landscape, context bottleneck, open challenges, SE-practice implications
7. **Conclusion**

Out of scope: editor autocompletion, code search, and multi-agent *role-play* frameworks (ChatDev, MetaGPT). Multi-agent *search/decomposition* pipelines (SWE-Search) are in scope and treated under §4.

## Files

- `report/main.tex` — Main LaTeX source (TUM template, twocolumn)
- `report/references.bib` — Bibliography
- `report/tum/` — TUM class files and resources

## Constraints

- **Body must stay under 15 pages**, excluding references. It currently ends on p15 with references starting p15; there is almost no slack. Any addition needs a corresponding cut.
- Do **not** reintroduce the term "vibe coding" — it was deliberately stripped throughout.
- The security/trustworthiness material was deliberately removed as off-thesis. The `veracode2025genai`, `susvibes2025`, and `securitydebt2025` bib entries were deleted with it (recoverable from git history).

## Typesetting notes

- `\KOMAoptions{parskip=half}` gives paragraph separation; `\parindent` is 0. Without this, consecutive body paragraphs fuse.
- `\paragraph` beforeskip is redeclared to `0pt plus 0.3ex minus 0.1ex` — KOMA's 3.25ex default stacks on `\parskip` and opens a double gap before every run-in heading. `afterskip` must stay negative to keep headings run-in.
- `xurl` is loaded so long bibliography URLs break; without it there are 4 overfull hboxes.

## Workflow

- When asked to write report content, edit the LaTeX source only; do not rebuild the PDF unless explicitly requested.
- Keep the bibliography free of orphans: every entry cited, every citation resolved.

## Status

Drafted in full and building clean (0 errors, 0 overfull boxes, 0 undefined references; 51 bib entries, all cited).

Figures: pipeline overview (§2), plan shapes CoT/ToT/GoT (§4), SWE-bench difficulty-stratified trajectory (§3), Verified-vs-Pro gap (§6). Tables: system comparison (§3), SWE-bench splits (§3), loop-closure axis (§5).

A survey-taxonomy figure was cut for space; it is recoverable from git history if the page budget ever loosens.
