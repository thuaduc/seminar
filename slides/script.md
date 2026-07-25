# Speaker script — Agentic AI for Code Generation

## How we write code has changed

Manual: you write every line. 
Autocomplete: you accept or reject suggestions, still reading every line. 
One-shot: you describe a function, the model writes it, but nothing checks it. 
Agentic: you state an intent, the system edits files, runs tests, reads failures, tries again.


## Real progress

April 2024, first agentic scaffolds: eighteen percent. M
arch 2025: sixty-five. 
April 2026: eighty-one.

Massive improvement in two years. 
For most of that period the models were not trained to be agents. 
The gain came from the model, but not only the model: 
- the scaffolding — the system around it


## What this talk shows

Three questions. 
- How do the systems work? 
- How do we measure them? 
- What are the challenges?

## What is an autonomous coding agent?

A system that takes a natural-language request and autonomously produces a working code change. 
It navigates files, edits, executes, and revises without step-by-step human guidance.

Three properties. 
- Autonomous — it decides what to do next. 
- Grounded in execution — it runs tests, reads errors, acts on them. 
- Repository-scale — it works across files in a real codebase, not on isolated functions.

## How the loop is controlled

- Orchestration — who decides what happens next? 
- The model, a hard-coded script, or a search algorithm?

- Interface and context — what can the agent see and do? 
- A purpose-built command set, or a raw sandbox?

These are not stages. 
They cut across every stage — they decide whether the loop runs at all and what it has access to.


## Orchestration: how control flows

The agentic loop. 
- The model observes the environment — file contents, test output, error traces. 
- It reasons about what it sees. 
- Then it acts through a tool call, and the cycle repeats until the model decides it is done.

The fixed pipeline. 
- The sequence is hard-coded: localise the fault, generate a repair, validate with tests. 
- There is no loop and no decision about what to do next. 
- The scaffold is in control.

Tree search. 
- Multiple trajectories are explored in parallel, each scored and pruned by a value function. 
- It is the agentic loop with backtracking — when a path looks bad, it abandons it and tries another.

This is the choice that decides who is in control — the model, the scaffold, or a search algorithm.

## What the agent sees and does

Two designs for how the agent interacts with the codebase. 
- ACI — a purpose-built interface. 
- The agent views files through a scrollable window, edits with linter-guarded commands, searches with summarised results. 
- The scaffold controls what the model sees and rejects bad actions before they run.

- Code as action — the model writes Python or bash in a sandbox. 
- Full shell access, no guardrails.

The ACI ablation is the cleanest evidence that the scaffold matters. 
- Same model, same benchmark. 
- Drop the linter, minus three points. 
- Raw search instead of summarised, minus six. 
- Whole files instead of a window, minus five.

Context is managed, not logged. 
- Compress old observations, search the syntax tree coarse to fine, retrieve by embedding.

## Interface and context

Two designs. 
- ACI — a purpose-built interface with a windowed viewer, linter-guarded edits, summarised search. 
- Code as action — the model writes Python or bash in a sandbox.

ACI ablation, same model, on Lite: 
- drop the linter, minus three points. 
- Raw search instead of summarised, minus six. 
- Whole files instead of a window, minus five. 
- That is the interface alone.

Context is managed, not logged. 
- Compress old observations, search the syntax tree coarse to fine, retrieve by embedding over a skeleton.

## The agent loop

Nearly all systems do the same five things.

- Intent capture — the agent reads the issue or specification. 
- This is where it decides what is being asked. 
- It happens once, at the start, and is never revisited.

- Planning — the agent decides where in the codebase to act. 
- Which files, which functions. 
- Not how to write the patch, but where the problem lives.

- Code generation — the agent writes or edits source code. 
- A patch, a new function, a refactored block.

- Execution — the agent runs what it wrote. 
- Tests, linters, the program itself. 
- This is where the environment can say no.

- Refinement — the agent reads the execution result and decides what to change. 
- It either regenerates the patch or replans from an earlier point.

## Agentic techniques: per-stage

Four dimensions. 
- Planning — where to change the code. 
- Generation — writing the patch, one candidate or many. 
- Execution — running it and reading the result. 
- Refinement — how the agent recovers from failures.

## Plan: finding the right code

Planning here is about where the code is, not how to write it. 
You choose one function among tens of thousands, and the symbol often does not appear in the issue text.

Add fault localisation to AutoCodeRover: 
- nineteen to twenty-two percent on Lite, 
- seven new instances solved, 
- nothing changed about patch generation.

## Plan shape

- Chain of thought — one linear path, a wrong first step gets elaborated. 
- Tree of thoughts — expand, score, prune. 
- Graph of thoughts — merge branches, revisit states.

The scorer is the load-bearing part, not the branching. 
What is a half-finished patch worth before any test has run?

SWE-Search builds an LLM value function inside MCTS — twenty-three percent relative improvement. 
But it calls a model at every node, which is why cheap pipelines still win.

## Plan: localisation

Hierarchical narrowing. 
- Agentless: files, then classes and functions, then edit locations. 
- AutoCodeRover: AST-backed search APIs, coarse to fine. 
- Specification sketches: interfaces, pseudocode, assertions.

The useful plans name concrete program elements you can check against the repository.

## Plan: the function boundary

D4C reports that perfect fault localisation makes repair worse. 
Both sides are right.

- Above the function boundary — which function among tens of thousands? 
- Localisation is the task. 
- Below it — ranking lines within a known-faulty function fights the model's infilling prior.

D4C repairs whole functions: 
- a hundred and eighty of four hundred and thirty-seven Defects4J bugs, 
- beating systems with perfect localisation by ten percent.

## Plan: the Agentless paradox

- Plan-then-generate: inspectable, brittle. 
- Interleaved: adaptive, hard to audit. 
- Generate-then-refine: first output as hypothesis.

Agentless scores highest and plans least. 
The claim is not that deliberation helps. 
Grounding helps — Agentless gets it structurally, hard-coding the narrowing an agentic loop rediscovers each time.

## Generate: writing the patch

Three approaches. 
- Infilling — the model completes a span given surrounding context, works well within a single function. 
- Diff generation — produce a unified diff, smaller output but fragile if the context is stale. 
- Sample N and pick one — generate many candidates, rank by test results or a critic.

Two correct patches can share no token. 
You cannot compare them as strings. 
Ranking must use execution or AST normalisation.

## Execute: running the code

This is where the environment can say no. 
- The test suite, the linter, the type checker — all external to the model.

Error traces tell the agent what failed and where. 
But the test suite is a sample of the intended behaviour, not the behaviour itself. 
A patch can pass every test and still be wrong.

## Refinement: iterative self-repair

Three mechanisms that close the loop by re-reading the model's own output. 
- Self-Refine: the model critiques itself — plateaus. 
- Reflexion: verbal critique across attempts with self-written tests — inherits what those tests miss. 
- Self-debugging: the model explains its own code — explains the bug as intended.

## Refinement: tests and trained critics

Three that bring in something external. 
- D4C: failing tests plus the whole function. 
- Agentless: regression plus generated reproduction tests. 
- Best-of-N with a critic: rollouts ranked by a trained model.

All run in per-instance sandboxes — for reproducibility first, containment second.

## Refinement: when does the loop stop?

All tests pass — but that smuggles ground truth into the stopping condition.

Diminishing returns — another repair round is often worse than a fresh sample at fixed budget.

Iterative repair and best-of-N are the same lever: extra inference to cut variance. 
- OpenHands: sixty percent on Verified with one rollout, sixty-six with five reranked by a critic.

Hold on to that middle column from the refinement tables.

## Five systems compared

Five systems side by side — how each handles orchestration, interface, planning, and refinement.

## SWE-bench Verified leaderboard

Latest numbers from the SWE-bench website, all run with mini-SWE-agent v2 for apples-to-apples comparison. 
February 2026.

## SWE-bench

Merged pull requests that close an issue and touch the test suite. 
Twenty-three hundred from twelve Python repositories.

- The test filter gives a grading oracle. 
- The issue filter gives the intent.

Task: repository before the fix, plus the issue text. 
Produce a patch. 
- Fail-to-pass tests must flip, 
- pass-to-pass must not regress.

Resolve rate: both sets pass, first patch.

## Measuring generated code

Not string similarity — two correct patches may share no token.

- Functional correctness: run it. 
- Pass-at-k: probability one of k samples passes. 
- Resolve rate: pass-at-one at repository scale. 
- Cost: dollars, calls, wall-clock — decouples from resolve rate.

Every metric scores the end of the pipeline. 
Plan quality, localisation, refinement efficiency: unmeasured.

## The five splits

- Full: everything. 
- Lite: three hundred easy single-file, some leak the fix. 
- Verified: five hundred human-screened. 
- Multilingual: nine languages. 
- Pro: long-horizon, multi-file, held-out repos.

Verified is the most curated, not the hardest. 
Resolve rates are not comparable across splits.

## What the numbers show

Nine systems on Verified, split by annotator difficulty.

Easy and medium are ninety-one percent of Verified. 
The headline mostly reports sub-hour work.

The hard tier stalled near twenty percent. 
A system can gain ten overall points while solving no new hard task.

Union column: 
- easy and medium are an ensembling problem — different systems solve different instances. 
- Hard is not — over half defeats every system at once.

## Can we trust the numbers?

Verified against Pro, same model weights. 
Everything drops twenty points or more.

Read the floor, not the spread — only Opus 4.5 uses the standardised harness. 
The others self-report. 
The one measured most strictly drops furthest.

- Contamination: OpenAI dropped Verified in 2026 — models reproduce gold patches from the task ID alone.
- Reward hacking: closing test leaks cuts scores by double digits.

## Challenge: intent capture

The stage with no feedback edge. 
The agent guesses and commits.

- Issue reports leave out edge cases. 
- Tests only check what they exercise — a wrong guess about an untested invariant scores as a success. 
- No surveyed system asks a clarifying question.

Clarification mechanisms exist and work. 
The gap is the evaluation: SWE-bench filters out ambiguity by construction. 
The agent is never rewarded for asking.

## Challenge: plan

The repository does not fit in context. 
The agent must choose what to read, and that choice is most of the problem.

- Hard tier stuck at twenty percent against easy at eighty. 
- Of the forty-five hard instances, twenty-five need several files — only nine are solved by anyone.

Is the cause retrieval or reasoning? 
Nobody has run the experiment. 
- Hand it the gold file set. 
- If the hard tier moves, retrieval. 
- If not, only training will.

## Challenge: generation

Two correct patches can share no token. 
Two near-identical patches can differ in meaning. 
You cannot score generated code against a reference.

The dominant quality lever — sample N and rerank — multiplies cost directly. 
Resolve rate decouples from compute. 
The practical frontier is accuracy per dollar.

## Challenge: execution

The test suite is a sample of the intended behaviour, not the behaviour itself. 
A patch can pass every test and still be wrong.

- Reward hacking: closing test-leakage channels cuts scores by double digits. 
- Agents partly learn the grader. 
- Contamination: models reproduce gold patches from the task ID alone. 
- OpenAI dropped Verified in 2026.

"All tests pass" smuggles ground truth into the stopping condition. 
Any self-critique inside that loop looks more effective than it is.

## Challenge: refinement

Real fixes touch several files over long sequences of steps.

Another repair round drops fast in value. 
- At fixed budget, often worse than a fresh sample. 
- Self-critique alone can only reorganise what the model believes — measured going negative.

Pro shows the same ceiling as the hard tier. 
Long-horizon, multi-file tasks do not yield to more iterations.

Iterative repair and best-of-N are the same lever — extra inference to cut variance. 
The question is which allocation buys more correctness.

## What it adds up to

Everything that works puts something outside the model in a position to contradict it.

- Oracle — test runner, compiler, human — can surprise the model. 
- Refinement against it reliably improves code.

- Proxy — trained critic, generated test — carries information but cannot surprise. 
- No new information enters the loop.

- Self-critique — same distribution as the generator. 
- Can only reorganise what the model believes. 
- Measured going negative on reasoning tasks.

The two weak stages are the two with no oracle. 
- Intent — the oracle is a human, nobody asks. 
- Validation — the oracle is a test suite a wrong patch can pass.

## Takeaways

- Agentic beats one-shot because it can run things, not because it reasons more.
- One number does not suffice. Difficulty, contamination and cost belong next to any resolve rate.
- Verification is the bottleneck. In a randomised trial, developers were nineteen percent slower with AI tools — believing they were twenty percent faster.

From autocomplete to autonomous issue resolution: crossed. 
From impressive to dependable: a question about oracles.
