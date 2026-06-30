# Review — *Agentic AI for Code Generation* (TUM SS26 Seminar Report)

**Artifact:** `report/main.tex` (+ `report/references.bib`), local LaTeX source, TUM twocolumn template, ~1190 lines.
**Source type:** Local files (read directly; no PDF parse needed).
**Reviewer pass date:** 2026-06-30. Several quantitative claims were spot-checked against primary/web sources (see Sources).
**Note:** This review both critiques the artifact and applies a set of safe improvements directly to `report/main.tex` (abstract, date, stale-wording fixes, added survey citations). Those edits are listed under *Improvements Applied*.

---

## Summary Assessment

This is a strong, well-organised seminar survey. Its distinguishing feature is the **vibe-coding-pipeline framing** (Intent → Plan → Generate → Execute → Refine) used consistently as a taxonomy backbone, reinforced by a document-wide taxonomy figure and per-section "this section instantiates branch X" anchors. Sections §3–§5 are genuinely drafted with accurate, well-cited numbers and several original visualizations (pipeline diagram, ReAct loop, plan shapes, localisation pipeline, candidate selection, SWE-bench trajectory chart). My quantitative spot-checks (D4C, Devin, SWE-bench Pro, Reflexion) **passed** — the factual core is reliable and the author hedges blog-sourced numbers appropriately.

The report's weaknesses are dominated by **completion state and consistency hygiene**, not factual error: an empty abstract, placeholder-only §6–§7, a date/anachronism mismatch, and stale "four research fronts" wording left over from archiving the multi-agent section. As a *seminar report in progress* these are expected; as a *submittable artifact* they are blocking. Recommendation: **Major revision** — finish §6–§7 and the intro, then ship.

---

## Strengths

1. **Consistent unifying framework.** The pipeline lens is not decorative: every survey section is explicitly mapped to pipeline stages (Fig. taxonomy), and the cross-references (§3↔§4↔§5) are coherent and bidirectional.
2. **Accurate, well-sourced numbers.** Verified externally: D4C repairs 180 Defects4J bugs at 10 samples each (arXiv 2404.08877); Devin's 13.86% is correctly placed on the **Full** line as a 570/2,294 subset (Cognition tech report); SWE-bench Pro Opus 4.5 ≈ 46% is correct (Scale Labs, 45.9%). Reflexion 91% pass@1 and the SWE-bench splits (2294/300/500) are right.
3. **Strong evaluation skepticism.** §3.4 is the best part of the report: it surfaces the easy/hard difficulty gap, contamination risk, Django dominance, Lite quality issues, and Verified as the de-facto standard — exactly the confounds a critical survey should raise.
4. **Original figures.** Six bespoke TikZ/pgfplots figures plus three tables. The plan-shapes (CoT/ToT/GoT) and localisation-pipeline diagrams add real explanatory value beyond text.
5. **Balanced framing of the agentic-vs-pipeline debate.** Agentless is correctly positioned as a non-agentic baseline that challenges the "autonomy is necessary" assumption, with cost/score trade-offs made explicit.

---

## Critical Issues
*(block a final/submittable version)*

- **C1 — Empty abstract.** Line 63 was `% TODO: Write abstract`. A survey with no abstract cannot be evaluated or indexed. **(Fixed: drafted abstract applied — see Improvements Applied.)**
- **C2 — §6 Discussion and §7 Conclusion are placeholder prose only.** They consist of one-line descriptions of intended content ("Recurring design patterns…", "Summarize key findings…"). The Discussion is where the report's cross-cutting payoff (evaluation landscape, context bottleneck, open challenges) must actually be argued; currently it is a stub. The taxonomy figure and §3.4 promise an "Evaluation Landscape (§5.2)" deferral that §6.2 does not yet deliver. **Verification: these are known WIP per `CLAUDE.md`, not factual defects — but they block completion.**

---

## Major Issues

- **M1 — Date/anachronism inconsistency.** `\date{April 1, 2025}` contradicted body content citing/plotting 2026 data (Claude Opus 4.6 ≈ Feb 2026, `agentmarketcap2026`, "frontier reached ~63% by early 2026"). A report dated April 2025 cannot cite April 2026 sources. **(Fixed: date set to 2026-06-30 to match SS26 + content.)**
- **M2 — Stale "four research fronts" wording.** After archiving the multi-agent section there are **three** survey fronts (E2E, planning/search, execution feedback); the Discussion is cross-cutting, not a front. Intro (L75), Discussion (L1160), and Conclusion (L1182) still said "four." **(Fixed: all three changed to "three.")**
- **M3 — Related-work paragraph is a placeholder and ignores in-bib surveys.** The "Background and Related Surveys" paragraph stated only what it *would* do, while `wang2024agentsse` and `dong2025surveycodegen` sit uncited in the bib. A survey must position itself against prior surveys explicitly. **(Partially fixed: both surveys now cited with an explicit "what this report adds" delta; the paragraph still warrants a sentence or two more on APR and code-gen surveys.)**

---

## Minor Issues

- **m1 — D4C bib entry accuracy.** `references.bib` lists title "Aligning LLMs for FL-free Program Repair" with author "Xu, Hao and others." The canonical arXiv title is "Aligning the Objective of LLM-based Program Repair" and the author list is a placeholder. Fix authors/title before submission. *(Not auto-fixed — correct author list not verified; flagged rather than fabricated.)*
- **m2 — Dead bib entries.** `qian2024chatdev`, `hong2024metagpt`, `huang2023agentcoder`, `islam2024mapcoder`, `xia2025livesweagent` are uncited; with `plain` bibstyle they will not render (harmless) but signal a removed section. Either cite (e.g., a one-line multi-agent mention in Discussion) or prune.
- **m3 — Figure code comment mismatch (cosmetic).** In `fig:swebench-trajectory`, the Devin point is commented "Mar 2024 Devin" but plotted at x=2024.33 (~late April). Comment-only; does not affect output.
- **m4 — "best RAG baselines resolved 1.96% of Full and 0.33% of Lite."** Correct per `jimenez2024swebench`, but the trajectory chart blends heterogeneous scaffolds/models as a single "best-in-class" line; a one-line caption caveat that points are not a fixed system would prevent over-reading the slope.
- **m5 — Weak-source numbers.** Several Full-set 2025 numbers (Opus 4 71.2%, Devin 68.3%) come from a single blog (`dataku2025swebench`). The report already hedges ("independent analysis"); keep that hedge and avoid presenting them as leaderboard-official.

---

## Reproducibility and Verification

| Claim | Source checked | Result |
|---|---|---|
| D4C: 180 Defects4J bugs, 10 samples each | arXiv 2404.08877 (html v1) | **Verified** |
| Devin 13.86% on 570/2,294 Full subset | Cognition SWE-bench tech report | **Verified** (Full subset, not Lite) |
| SWE-bench Pro Opus 4.5 ≈ 46% | Scale Labs leaderboard (45.9%) | **Verified** |
| Claude Opus 4.6 exists (Feb 2026 point) | Multiple 2026 vendor/benchmark sources | **Verified** (model exists; exact Lite % web-sourced) |
| Reflexion 91% pass@1 HumanEval; SWE-bench splits 2294/300/500 | shinn2023reflexion; jimenez2024swebench | **Consistent** (not re-fetched) |
| SWE-agent 12.47/18.00%, $1.21; self-debug, AlphaCode/CodeT | primary papers | **Internally consistent**; not independently re-fetched — `Verification: PARTIAL` |
| Full-set 2025 numbers (71.2/68.3%) | dataku blog only | **Weak source** — appropriately hedged in text |

No code/dataset is produced by this artifact (it is a survey), so reproducibility reduces to citation traceability, which is generally good. Every quantitative figure traces to a cited source.

---

## Inline Annotations

- **Abstract (L63):** was empty → drafted.
- **Intro / Background (L72–75):** placeholder; now cites `wang2024agentsse`, `dong2025surveycodegen`; add 1–2 sentences on APR and prior code-gen surveys to fully discharge "Background and Related Surveys."
- **§2 Mapping (L140):** "three subsequent sections" is **correct** — keep.
- **§3.2 Devin (L490–497):** "13.9% on Full" and the 570-subset framing are accurate; consider stating "(570/2,294 random subset)" inline for the casual reader.
- **§3.4 trajectory chart:** add caption caveat that the best-in-class line aggregates different systems.
- **§3.4 (L506):** "four other repositories" is legitimate (Django + 4) — **do not** change to "three."
- **§5.3 D4C (L1058+):** text claims accurate; fix the bib entry (m1).
- **§6 Discussion / §7 Conclusion:** placeholder — must be written; the deferred "Evaluation Landscape (§5.2)" promise from §3.4 lands here.

---

## Recommendation

**Major revision.** The surveyed content (§3–§5) is publication-quality for a seminar: accurate, well-cited, and well-illustrated. The blockers are completion (abstract — now drafted; §6–§7 — still required) and consistency (date and "four"→"three" — now fixed). Priority order: (1) write §6 Discussion with the cross-cutting argument the report keeps promising; (2) write §7 Conclusion; (3) flesh out the Background paragraph and fix the D4C bib entry; (4) prune or cite the dead multi-agent bib entries.

---

## Sources

- D4C: https://arxiv.org/html/2404.08877v1 (arXiv:2404.08877)
- Devin SWE-bench technical report: https://cognition.ai/blog/swe-bench-technical-report
- SWE-bench Pro leaderboard: https://labs.scale.com/leaderboard/swe_bench_pro_public
- Opus 4.5/4.6 benchmark roundup: https://sqmagazine.co.uk/claude-vs-chatgpt-for-coding/
- SWE-bench leaderboards: https://www.swebench.com/
- Artifact: `report/main.tex`, `report/references.bib`, `report/archive/multi-agent-frameworks.tex`
- Evidence notes: `outputs/.drafts/agentic-code-gen-report-review-evidence.md`
- Plan: `outputs/.plans/agentic-code-gen-report-review-plan.md`
