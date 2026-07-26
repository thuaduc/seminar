# Speaker script — Agentic AI for Code Generation

## How we write code has changed

Manual: you write every line. 
Autocomplete: you accept or reject suggestions, still reading every line. 
One-shot: you describe a function, the model writes it, but nothing checks it. 
Agentic: you state an intent, the system edits files, runs tests, reads failures, tries again.


## Real progress

April 2024, first agentic scaffolds: eighteen percent. 
March 2025: sixty-five. 
April 2026: eighty-one. 

Massive improvement in two years. 
Two factors:
- Model improvement. Models were not trained to be agents.
- Agent system improvement


## What this talk shows

Three questions. 
- How do the systems work? 
- How do we measure them? 
- What are the challenges?

## A few terms, up front

Five words you'll keep hearing — quick definitions before I use them. 
Scaffold: the non-model code around it — tools, prompts, control flow. 
AST: a parsed structure of the code — classes, functions, calls, not raw text. 
Harness: the environment a result is run and graded in. 
Oracle: something outside the model that can check it — a test suite, a compiler, a human. 
Critic: a separate model, trained to score candidate outputs.

## What is an agentic coding system?


A system that takes a natural-language request and autonomously produces a working code change. 
It navigates files, edits, executes, and revises without step-by-step human guidance.

Three properties. 
- Autonomous — it decides what to do next. 
- Grounded in execution — it runs tests, reads errors, acts on them. 
- Repository-scale — it works across files in a real codebase, not on isolated functions.

## The agent loop

Nearly all systems do the same five things.

- Intent capture — the agent reads the issue or specification. 
- This is where it decides what is being asked. 
- It happens once, at the start, and is never revisited.

- Planning — the agent decides where in the codebase to act. 
- Which files, which functions. 
- What is the problem and how to solve it.

- Code generation — the agent writes or edits source code. 
- A patch, a new function, a refactored block.

- Execution — the agent runs what it wrote. 
- Tests, linters, the program itself. 

- Refinement — the agent reads the execution result and decides what to change. 
- It either regenerates the patch or replans from an earlier point.

## How the loop is controlled

The five stages are what happens. Two controls decide how the loop runs —
and each acts on a different part of the loop you just saw.

Orchestration is the arrows — it decides the next step.
The model deciding, or a hard-coded script? That is what decides whether
refinement ever loops back, or the run just stops.

Interface and context is inside each box — what the agent can see and do at a
stage: a purpose-built command set, or a raw sandbox.

So: orchestration controls the flow between stages, interface controls what
happens within one. Neither is a stage itself — both cut across all five.


## Orchestration

Two ways to run the same five stages — the difference is who picks the next step.

The agentic loop: the model decides. 
The fixed pipeline: the scaffold decides. 

An example of each next.

## Orchestration: agentic loop

The model is in control — this is the ReAct loop: Thought, Action, Observation. 
- Thought — it reasons about what it sees. 
- Action — it acts through a tool call. 
- Observation — it reads back the result: file contents, test output, error traces. 
- The cycle repeats until the model decides it is done.

Read a file, run tests, edit — any tool, any order, as many rounds as it takes. 
Nothing outside the model fixes the sequence.

## Orchestration: fixed pipeline

The scaffold is in control. 
- The developer writes the sequence in advance: localise the faulty code, repair it, validate against the tests. 
- The model fills each step, but it never decides what comes next — the scaffold does.

On failure it stops or moves on — no arrow goes back, it does not retry. 
Cheaper and predictable, but no recovery.

## Interface and Context

Two common designs for how the agent interacts with the codebase. 
- ACI — a purpose-built interface. 
- The agent views files through a scrollable window, edits with linter-guarded commands, searches with summarised results. 
- The scaffold controls what the model sees and rejects bad actions before they run.

- Code as action — the model writes Python or bash in a sandbox. 
- Full shell access, no guardrails.

Context is managed, not logged. 
- Compress old observations, search the syntax tree coarse to fine, retrieve by embedding.

## Interface: side by side

Same four operations, side by side — viewing, editing, searching, executing.

The ACI ablation is the cleanest evidence that the scaffold matters, not the model. 
- Same model, same benchmark, on Lite. 
- Drop the linter, minus three points. 
- Raw search instead of summarised, minus six. 
- Whole files instead of a window, minus five.

That's the two axes.

## Back to the five stages

Same diagram as before, one stage lit up. Orchestration and interface cut across all five — now we go through them one at a time, starting where the model has the least to lean on: planning.

## Plan: finding the right code

Planning makes two commitments: where to change the code, and what the change should be. 
Where — you choose one function among tens of thousands, and the symbol often does not appear in the issue text. 
What — a signature, an interface, an invariant, stated concretely enough that the repository can check it. 
Both are guesses; the section is mostly about the first, because that is the one with no pre-training prior to lean on.

Add fault localisation to AutoCodeRover: 
- nineteen to twenty-two percent on Lite, 
- seven new instances solved, 
- nothing changed about patch generation.

## Plan shape
    
How does the agent explore possible plans? Before any code is written, it searches the space of candidate plans — a spectrum from no search to explicit search.

- Chain of thought — the model thinks step by step in one straight line. 
- If the first step is wrong, every step after it builds on that mistake. 
- There is no way to go back.

- Tree of thoughts — instead of one path, the model generates several options at each step. 
- A scorer picks the most promising branch and drops the rest. 
- This is where it starts to look like search.

- Graph of thoughts — like a tree, but branches can merge back together. 
- The model can revisit an earlier state or combine ideas from two different paths.

The key question is what scores each branch? 
- A half-finished patch has not been tested yet. 
- There is no compiler output, no test result — nothing external to judge it. 
- So the scorer is usually the model itself, guessing whether the path looks right.

SWE-Search builds an LLM value function inside Monte Carlo tree search — twenty-three percent relative improvement. 
But it calls a model at every node, which makes it expensive. 
That is why cheap fixed pipelines still win on cost.

Hold that gap: the scorer is the model judging its own guess — same distribution, it cannot surprise itself. So what could ground it instead? We'll get there — first, what these shapes actually look like in real agents.

## Plan: Chain of Thought

Start from the problem the plan is for — a one-line bug report: the app crashes on checkout with an empty cart.
It names no file and no fix. Planning has to decide where the bug is and what to change.

Chain of thought is the simplest shape: observe, think, act, in one straight line — the ReAct loop from orchestration.
Watch that every action here is just looking — grep, open. No edit yet. The output of planning is not a fix, it is a plan: where and what.
Thought: an empty cart, probably a divide-by-zero in the total. Action: grep the total function. Observation: sum of prices over length of items. Thought: length is zero when empty. Action: open it to confirm. Observation: no guard. Plan — where: total at cart.py line 12; what: guard the empty cart, return zero. Then generation writes the edit, later.
One straight line. If that first thought is wrong — the crash is really in the receipt — it just keeps going. What catches it is not a better shape; it is the tests failing later, which is a different loop.

## Plan: Tree of Thoughts

In practice no agent grows a tree of half-thoughts. The tree lives one level up: run the whole ReAct agent several times, get several complete patches, score them. 
Run 1 passes two of three tests, run 2 all three, run 3 none — pick run 2. 
The scorer is tests or a trained critic, not the model's own hunch. OpenHands goes from sixty to sixty-six percent with five runs reranked. SWE-Search wraps it in Monte Carlo tree search with a learned value — plus twenty-three percent, but a model call at every node.

## Plan: Graph of Thoughts

Merging and revisiting is demonstrated on puzzles — Game-of-24, sorting — almost never on repository bugs. 
In real agents you see the two ends: one chain, or many chains scored. The closest thing to revisit is Reflexion — write a note after a failure, retry with it — a loose loop, not a graph. 
Treat the graph as the conceptual top of the spectrum, not something running in production.

## Plan: localisation

So how does it stop guessing? Point at real code. A tree lets the model try several guesses, but nothing has run yet — so it narrows the actual repository from broad to specific, and every step names code it can check. 
- Agentless: files, then classes and functions, then edit locations. 
- AutoCodeRover: AST-backed search APIs, coarse to fine.

The useful plans name concrete program elements you can check against the repository — not invented requirements.

## Localisation: Agentless

Narrowing without letting the model wander — three fixed steps, coarse to fine. 
- Rank files: from the repo layout and the issue, shortlist the files most likely involved. 
- Rank elements: inside those files, narrow to specific classes and functions. 
- Pin locations: inside those, the exact lines to edit.

Three prompts, no search, no tools. The narrowing is hard-coded into the scaffold — fixed, not discovered.

## Localisation: AutoCodeRover

Let the model navigate, but through the syntax tree — not by reading whole files. 
- Search APIs: search_class, search_method, search_method_in_class. 
- The agent calls them coarse to fine, pulling in just the code it needs. 
- AST-backed: structured, cheaper context than dumping whole files into the prompt.

The model chooses what to search — agentic, where Agentless is a fixed funnel.

Grounding is planning's job, done blind — nothing has run yet. Generation is next, and it finally produces something execution can check.

## Generate: turning it into code

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

Right or wrong, that verdict is what refinement works with next.

## Refinement: iterative self-repair

Execution just returned a verdict. What the agent does with it is the last dimension, and where it varies the most.

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

## SWE-bench

*Part two — how we measure.* Every number ahead scores one thing: did the final patch pass. Watch what that leaves invisible — plan quality, localisation, the work of refinement.

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

- Functional correctness: does it run and pass? The only thing that counts. 
- Pass-at-k: give it k tries, count it solved if any one passes. 
- Resolve rate: pass-at-one on a whole repo — solved on the first try. 
- Cost: dollars, calls, time — a higher score can cost much more.

Every metric scores the end of the pipeline. 
Plan quality, localisation, refinement efficiency: unmeasured.

## The five splits

Roughly two years of splits, oldest to newest. 
- Full, Oct 2023: everything. 
- Lite, Mar 2024: three hundred easy single-file, some leak the fix. 
- Verified, Aug 2024: five hundred human-screened. 
- Multilingual, May 2025: nine languages. 
- Pro, Sep 2025: long-horizon, multi-file, held-out repos.

Verified is the most curated, not the hardest. 
Resolve rates are not comparable across splits.

## How hard is each task?

Before the graph, one thing about the tiers.
When OpenAI built Verified, annotators estimated the fix time for each issue —
how long a skilled engineer would need to write the patch.
Three buckets: under fifteen minutes easy, up to an hour medium, over an hour hard.

Two things to hold onto.
It is estimated human effort — a proxy for how involved the change is, not a
clock on the agent.
And there were really four bins; one-to-four hours and four-plus collapse into
hard, because only three issues exceed four hours.

## The headline climbs

Same nine systems, April 24 to March 25 — four lines, all one dataset.
Blue is the overall rate: eighteen to sixty-five percent, SWE-agent to Augment.
Orange easy, green medium, red hard.
Watch the spread: easy is near eighty, hard is stuck at twenty.
The overall line is just the tier-weighted average — it hides that gap.
Next slide reads off the exact splits.

## What the numbers show

Nine systems on Verified, split by annotator difficulty.

Easy and medium are ninety-one percent of Verified. 
The headline mostly reports sub-hour work.
And this split is not just one blog — Epoch AI's 2025 analysis finds the same
composition independently, thirty-nine easy, fifty-two medium, nine hard.
The pie is that mix; the table is how each tier resolves.

The hard tier stalled near twenty percent. 
A system can gain ten overall points while solving no new hard task.

## Challenge: intent capture

*Part three — where it breaks.* The spine already told us where to look: the weak stages are the two with no oracle, nothing that can tell the model it is wrong. This is the first of them.

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

## Challenge: cost and verification

Two problems on the same axis. 

The dominant quality lever — sample N and rerank — multiplies cost directly. 
Resolve rate decouples from compute: spending more does not reorder the systems.

"All tests pass" smuggles ground truth into the stopping condition. 
The suite is a sample of intended behaviour, not the behaviour itself — a patch can pass every test and still be wrong.

Self-critique alone can only reorganise what the model already believes — measured going negative.

The practical frontier is not peak accuracy. 
It is accuracy per dollar, bounded by an oracle a wrong patch can still pass.

## Takeaways

- Agentic beats one-shot because it can run things, not because it reasons more.
- All three challenges are the same gap: nothing outside the model catches a wrong guess about intent, a wrong file, or a wrong fix.
- Verification is the bottleneck. In a randomised trial, developers were nineteen percent slower with AI tools — believing they were twenty percent faster.

From autocomplete to autonomous issue resolution: crossed. 
From impressive to dependable: still an open question.

## Backup: can we trust the numbers?

Verified against Pro, same model weights. 
Everything drops twenty points or more.

Read the floor, not the spread — only Opus 4.5 uses the standardised harness. 
The others self-report. 
The one measured most strictly drops furthest.

- Contamination: OpenAI dropped Verified in 2026 — models reproduce gold patches from the task ID alone.
- Reward hacking: closing test leaks cuts scores by double digits.

If someone asks why the tier table is from 2025: because that is where the
stratified data stops. By early 2026 the frontier had saturated Verified near
eighty percent and OpenAI retired it as contaminated, so nobody re-ran the
per-difficulty breakdown. The 2025 snapshot is the last clean read; the live
frontier signal has moved to Pro.

## Backup: surveyed systems on SWE-bench Verified

Best published result for each of the five surveyed systems on Verified, pulled from swebench.com. 
Devin's number is on a different, easier split, self-reported, and never reproduced — read it as context, not a comparison.
