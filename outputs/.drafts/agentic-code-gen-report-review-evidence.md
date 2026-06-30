# Evidence Notes — Agentic AI for Code Generation review

Sources inspected:
- `report/main.tex` (1190 lines), `report/references.bib` (277 lines)
- `report/archive/multi-agent-frameworks.tex` (archived section)
- Web verification (2026-06-30)

## Structural observations (from main.tex)
- L55: `\date{April 1, 2025}` — but body cites/plots 2026 data (L497 "early 2026", fig comments "Feb 2026 Claude Opus 4.6", agentmarketcap2026 dated 2026). ANACHRONISM.
- L63: abstract is `% TODO: Write abstract` — EMPTY.
- §6 Discussion (L1157+) and §7 Conclusion (L1180+): placeholder prose only ("Recurring design patterns...", "Summarize key findings..."). Known WIP per CLAUDE.md.
- L75 "four active research fronts", L1160 "across all four topics", L1182 "across the four research fronts": STALE — multi-agent section archived, leaving 3 survey fronts (E2E, Planning/Search, Execution Feedback). Discussion is cross-cutting, not a 4th front.
- L140 "three subsequent sections" is CORRECT (§3,4,5).
- L506 "four other repositories" is legitimate (Django + 4 others).
- references.bib contains uncited entries (qian2024chatdev, hong2024metagpt, huang2023agentcoder, islam2024mapcoder, wang2024agentsse, dong2025surveycodegen, xia2025livesweagent). With `plain` bibstyle, uncited refs do not render — harmless but dead. Surveys wang2024agentsse & dong2025surveycodegen exist but are NOT cited in the "Background and Related Surveys" paragraph (L72-75), which is itself placeholder.

## Quantitative claim verification
1. SWE-agent 12.47% Full / 18.00% Lite, $1.21, 12 median steps — consistent with yang2024sweagent. Not re-fetched; internal consistency with table OK.
2. Reflexion 91% pass@1 HumanEval vs GPT-4 80% — matches shinn2023reflexion abstract (known).
3. Self-debug Spider +2–3% no tests, MBPP up to +12% with tests — consistent with chen2024selfdebug.
4. **D4C — VERIFIED** (arxiv.org/html/2404.08877v1): "D4C can repair 180 bugs correctly in Defects4J, with each patch being sampled only 10 times. This surpasses the SOTA." Report's "+10% over perfect FL, 90% fewer samples" is consistent with paper claims. OK.
   - Caveat: bib title is "Aligning LLMs for FL-free Program Repair", author "Xu, Hao and others" — actual paper title is "Aligning the Objective of LLM-based Program Repair". Author list in bib is a guess ("and others"). MINOR bib accuracy issue.
5. **Devin 13.86% — VERIFIED provenance** (cognition.ai/blog/swe-bench-technical-report): evaluated on a randomly chosen 25% of the **Full** test set (570/2,294), resolved 79 → 13.86%. Report plots this on the Full line with comment "(570 subset, extrapolated)" — placement CORRECT. Text L490 "Devin 13.9%" on Full consistent.
   - MINOR: fig comment labels it "Mar 2024 Devin" but x=2024.33 (~late Apr). Cosmetic.
   - NOTE: some secondary sources (sitepoint) wrongly call it Lite; report follows the authoritative Cognition Full-subset framing. Good.
6. **Claude Opus 4.6 — EXISTS** (multiple 2026 sources): Opus 4.5/4.6/4.7 released through 2026. Fig point "Feb 2026 Claude Opus 4.6" 62.7% Lite is plausible; Lite-specific number not independently confirmed (web-sourced via agentmarketcap2026/dissecting2025).
7. **SWE-bench Pro Opus 4.5 ~46% — VERIFIED** (labs.scale.com, sqmagazine): Opus 4.5 = 45.9% on SWE-bench Pro standardized. Report L498 "~46%" CORRECT. Opus 4.5 = 80.9% on Verified (vendor).
8. dataku Full numbers (Claude Opus 4 71.2%, Devin 68.3%; Devin 48.2% Verified) — single independent blog (dataku2025swebench); report appropriately flags as "independent analysis". Provenance is a blog, not peer-reviewed; treat as weak evidence (report already hedges).

## Figure/table consistency
- Table tab:e2e-systems numbers match in-text (SWE-agent 18/12.5/$1.21; Agentless 32/$0.70; ACR 19% pass@1, 26% pass@3, $0.43; OpenHands Lite 26.0). Consistent.
- fig:swebench-trajectory: mixes best-in-class across heterogeneous scaffolds/models; legend distinguishes Full/Lite/Verified-easy/hard. Caption cites dissecting2025, dataku2025swebench, agentmarketcap2026. Acceptable for a survey trend chart but data points are leaderboard-compiled (not all primary). Verified easy/hard points sourced from ganhotra2025difficulty.
- tab:swebench-splits (Full 2294 / Lite 300 / Verified 500) — correct.

## Net assessment inputs
- Strong, well-cited §3–§5; figures are a differentiator. Main defects are completion-state (abstract, §6/§7) and consistency hygiene (date, "four"), not factual errors. Quantitative spot-checks largely passed.
