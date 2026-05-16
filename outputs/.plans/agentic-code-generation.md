# Comparison Plan: Agentic Code Generation

## Objective
Find and compare papers from top ML/NLP/SE conferences on agentic AI for code generation — LLM agents that plan, write, execute, debug, test, and refine code in realistic software-engineering environments.

## Sources to Search
- **ML/AI conferences**: ICML, NeurIPS, ICLR
- **NLP conferences**: ACL, EMNLP, NAACL
- **SE conferences**: ICSE, FSE, ASE

## Search Strategy
- Use alpha_search (semantic + keyword) for academic papers
- Use web_search for recent conference proceedings (2023-2025)
- Filter for papers explicitly about agentic/multi-step code generation, not just single-pass code LLMs

## Dimensions to Evaluate
1. **Agent architecture** — planning, tool use, feedback loops
2. **Capabilities covered** — which of {plan, write, execute, debug, test, refine}
3. **Benchmarks used** — SWE-bench, HumanEval, MBPP, CodeContests, etc.
4. **Key results** — quantitative performance claims
5. **Venue & year**
6. **Evidence type** — empirical, ablation, case study
7. **Caveats/limitations**

## Expected Output
- `papers/` — individual paper notes (title, venue, year, arXiv link, summary)
- `outputs/agentic-code-generation-comparison.md` — comparison matrix + synthesis
- Charts if quantitative metrics allow comparison

## Workflow
1. Parallel search across venue clusters (ML, NLP, SE)
2. Deduplicate and filter for relevance
3. Read top papers for claim extraction
4. Build comparison matrix
5. Verify sources and add citations
