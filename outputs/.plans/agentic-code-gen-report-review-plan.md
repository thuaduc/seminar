# Review Plan — Agentic AI for Code Generation (Seminar Report)

## Artifact
- Identifier: `report/main.tex` (+ `report/references.bib`) — local LaTeX source
- Type: TUM SS26 seminar survey report, twocolumn, ~1190 lines
- Framing: "vibe coding pipeline" Intent→Plan→Generate→Execute→Refine
- Status per CLAUDE.md: §1 intro placeholder-ish, §2–§5 drafted, §6–§7 placeholders, abstract empty

## Review criteria
- Novelty / contribution clarity (survey framing vs prior surveys)
- Empirical rigor of cited numbers (SWE-bench scores, costs, pass@1)
- Baselines / fairness of comparisons
- Reproducibility (sources for every quantitative claim, leaderboard provenance)
- Claims validity (anachronisms, fabricated-looking model names, future-dated sources)
- Figures/tables (correctness, internal consistency, captions vs data)
- Metrics definitions (pass@1 vs pass@3, Full/Lite/Verified)
- Related work coverage (surveys cited?)
- Writing quality / internal consistency (abstract, date, "four topics" leftover)

## Verification checks needed
1. SWE-agent 12.47% Full / 18.00% Lite, $1.21 — vs paper
2. Reflexion 91% pass@1 HumanEval — vs paper
3. Self-debugging Spider 2–3%, MBPP up to 12% — vs paper
4. D4C: 180 bugs, 10 samples, +10% over perfect FL, 90% fewer samples — vs paper
5. Devin 13.86% SWE-bench provenance + subset claim
6. dataku Full numbers (Claude Opus 4 71.2%, Devin 68.3%) provenance
7. "Claude Opus 4.6" at 2026.17 in fig data — does this model exist?
8. SWE-bench Pro Claude Opus 4.5 ~46% — provenance
9. Date "April 1, 2025" vs literature/sources dated through 2026 (anachronism)
10. "four topics/fronts" leftover after multi-agent section archived
11. Empty abstract; §6/§7 placeholder-only
12. Uncited bib entries / missing survey citations in intro

## Deliverables
- Evidence notes: outputs/.drafts/agentic-code-gen-report-review-evidence.md
- Final review: outputs/agentic-code-gen-report-review.md
- Improvements applied directly to report/main.tex (abstract, consistency fixes, citations)
