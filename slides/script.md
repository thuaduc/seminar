# Speaker script: Agentic AI for Code Generation

## How we write code has changed

Two columns: what the machine does, and what stays with you.
Manual: you write every line, and lean on the test suite to catch mistakes.
Autocomplete: you accept or reject each suggestion, line by line, and you stay the author.
One-shot: you describe a function, the model writes it, and you have to read and check all of it yourself.
Agentic: you state an intent, then review the outcome, while the system finds files, edits, and runs tests.


## Real progress

April 2024, the first agentic scaffolds: eighteen percent.
March 2025: sixty-five.
April 2026: eighty-one.

Massive improvement in two years.
Two things drove it:
- Better models. And note that none of them were trained to be agents.
- Better agent systems around those models.


## What this talk shows

Three questions.
- How do the systems work?
- How do we measure performance?
- What are the challenges?

## A few terms, up front

Six words you'll keep hearing, so let me define them before I use them.
Scaffold: the non-model code around the model, so tools, prompts, control flow.
Tool call: the model asking the scaffold to act, say read a file or run the tests, and getting the output back.
Patch: a description of a change, which lines in which files, not the changed code itself. It's what an agent actually submits.
Harness: the environment a result is run and graded in.
Oracle: something outside the model that can check it, like a test suite, a compiler, a human.
Critic: a separate model, trained to score candidate outputs.

## What is an agentic coding system?

A system that takes a natural-language request and autonomously produces a working code change.
It navigates files, edits, executes, and revises without step-by-step human guidance.
And it works across files in a repo, not on isolated functions.

## The agent loop

Nearly all systems do the same five things.

- Intent capture: the agent reads the issue or specification.
- This is where it decides what is being asked.
- It happens once, at the start, and is never revisited.

- Planning: the agent decides where in the codebase to act.
- Which files, which functions.
- What the problem is and how to solve it.

- Code generation: the agent writes or edits source code.
- A patch, a new function, a refactored block.

- Execution: the agent runs what it wrote.
- Tests, linters, the program itself.

- Refinement: the agent reads the execution result and decides what to change.
- It either regenerates the patch or replans from an earlier point.

Two controls decide how the loop runs.

Orchestration is the arrows. It decides which step comes next.

Interface and context sit inside each stage. They are the eyes, arms and legs of the agent: what it can see, and what it can do.

## Orchestration: agentic loop

The model is in control. This is the ReAct loop, an observe, think, act cycle.
- Observe: it reads the environment, so file contents, test output, error traces.
- Think: it reasons about the next step.
- Act: it takes an action through a tool call.
- The cycle repeats until the model decides it is done.

Read a file, run tests, edit: any tool, any order, as many rounds as it takes.
Nothing outside the model fixes the sequence.

## Orchestration: fixed pipeline

Here the scaffold is in control.
- The developer writes the sequence in advance: localise the faulty code, repair it, validate against the tests.
- The model fills in each step, but it never decides what comes next. The scaffold does.

On failure it stops or moves on. No arrow goes back, so it does not retry.
Cheaper and predictable, but no recovery.

## Interface and Context

Two common designs for how the agent interacts with the codebase.
- ACI, the agent-computer interface: a purpose-built interface.
- The agent views files through a scrollable window, edits with linter-guarded commands, searches with summarised results.
- The scaffold controls what the model sees, and rejects bad actions before they run.

- Code as action: the model writes Python or bash in a sandbox.
- Full shell access, no guardrails.

Context is managed, not logged.
- Compress old observations, search the syntax tree coarse to fine, retrieve by embedding.

## Interface: side by side

The same four operations, side by side: viewing, editing, searching, executing.

## Back to the five stages

Now we go through them one at a time, starting with plan.

## Plan: finding the right code

Planning makes two commitments: where to change the code, and what the change should be.
Where: you pick one function among tens of thousands, and the symbol you need often does not appear in the issue text at all.
What: a signature, an interface, an invariant, stated concretely enough that the repository can check it.


## Plan shape

How does the agent explore possible plans? Before any code is written it searches the space of candidate plans, and that runs on a spectrum from no search to explicit search.

- Chain of thought: the model thinks step by step in one straight line.
- If the first step is wrong, every step after it builds on that mistake.
- There is no way back.

- Tree of thoughts: instead of one path, the model generates several options at each step.
- A scorer picks the most promising branch and drops the rest.
- This is where it starts to look like search.

- Graph of thoughts: like a tree, but branches can merge back together.
- The model can revisit an earlier state, or combine ideas from two different paths.

## Plan: Chain of Thought

Start from the problem the plan is for, a one-line bug report: the app crashes on checkout with an empty cart.
It names no file and no fix. Planning has to decide where the bug is and what to change.

Chain of thought is the simplest shape: observe, think, act, in one straight line. It's the ReAct loop from orchestration.
Watch that every action here is just looking: grep, open. No edit yet. The output of planning is not a fix, it is a plan: where and what.
Thought: an empty cart, probably a divide-by-zero in the total. Action: grep the total function. Observation: sum of prices over length of items. Thought: length is zero when the cart is empty. Action: open it to confirm. Observation: no guard. Plan, where: total at cart.py line 12. What: guard the empty cart, return zero. Generation writes the edit later.
One straight line. If that first thought is wrong, say the crash is really in the receipt, it just keeps going. What catches it is not a better shape, it's the tests failing later, and that is a different loop.

## Plan: Tree of Thoughts

In practice no agent grows a tree of half-thoughts. The tree lives one level up: run the whole ReAct agent several times, get several complete patches, score them.
Run 1 passes two of three tests, run 2 all three, run 3 none. Pick run 2.
The scorer is tests or a trained critic, not the model's own hunch. OpenHands goes from sixty to sixty-six percent with five runs reranked. SWE-Search wraps it in Monte Carlo tree search with a learned value, worth plus twenty-three percent, but it costs a model call at every node.

## Plan: Graph of Thoughts

Merging and revisiting is demonstrated on puzzles, Game-of-24 and sorting, almost never on repository bugs.
In real agents you only see the two ends: one chain, or many chains scored. The closest thing to revisiting is Reflexion, which writes a note after a failure and retries with it. That's a loose loop, not a graph.
Treat the graph as the conceptual top of the spectrum, not something running in production.

Planning ends here, and all of it happens blind: where and what, chosen before anything runs. Generation is next, and it finally produces something execution can check. (The grounding mechanics for Agentless and AutoCodeRover are in the backup slides if the question comes up.)

## Generate: infilling

Now generation turns the plan into code. Same empty-cart example, three ways to write the edit.

First, infilling. The scaffold opens a gap and the model fills it. It reads the grey lines and writes only the orange ones, the two-line guard.

In its favour: this is the operation models are trained on most. The bulk of training is cutting a span out of a real file and asking the model to put it back, given what surrounds it. So it is the shape the model is strongest at, and the diff format on the next slide is not. It also can't land in the wrong place, because the scaffold picked the spot. Against it: one span at a time, in practice one function, and the surrounding code has to fit in the prompt.

## Generate: diff generation

Second, a diff: send only the change, not the whole file. Two wins. You can edit a huge file without re-writing it, and one patch can touch many places in many files. The cost is that the model now has to point to where the edit goes, and the surrounding context lines are what locate it. If it is off by even one line, the patch will not apply, before any test runs.

## Generate: sample N, pick one

Third, don't trust one shot. Write N patches and keep the one that passes. Here one wraps a try/except, one adds the guard, one returns zero always. The tests rank them: B passes three of three, so take B.
What you gain: one bad sample no longer sinks the task, and different candidates try different ideas. What you pay: picking needs an oracle. Two correct patches can share no token, so you can't compare them as text, and the model's own confidence doesn't count. You have to run them. And cost grows with N: calls, dollars, wall-clock.

## Execute: running the code

This is where the environment can say no. All three checks are external to the model.
- Test suite: fail-to-pass must flip, pass-to-pass must not regress.
- Linter or type checker: syntax and type errors, before tests even run.
- Error traces: what failed, and where.

Right or wrong, that verdict is what refinement works with next.

## Refinement: iterative self-repair

After execution returns a verdict, refinement decides what to do with it. 

These first three all work the same way: the model reads back its own output and tries to fix it.
- Self-Refine: the model critiques its own answer and rewrites it, with no outside signal.
- Reflexion: after a failed attempt it writes itself a short note on what went wrong, then retries with that note in context.
- Self-debugging: the model explains its own code back to itself, combine with test verdict.

The ceiling is simple: if a model could spot its own wrong code just by looking, it would have written it right the first time. 


## Refinement: tests and trained critics

Now something the model did not produce closes the loop, and that is the whole difference.
- D4C: it hands the failing tests and the whole function back to the model and asks for a fresh fix.
- Agentless validate: it keeps the project's own regression tests and adds generated ones to screen out bad patches.
- Best-of-N plus critic: it runs several patches to completion and lets a trained critic rank them.

External tests or critic can genuinely surprise the model. Generated tests cannot, because a misread issue just produces a test that confirms the misread. 

But they are also limited. Tests catch only the cases they run; the critic, only what it learned.

## Five systems compared

Five systems side by side: how each one handles orchestration, interface, and refinement, and where each is actually used.

## Where are these systems used?

The five we studied are mostly research artefacts: open-source, run mostly on SWE-bench. Blueprints, not products.

- SWE-agent, AutoCodeRover and Agentless live in papers and on the leaderboard.
- Two are shipped: OpenHands as an open platform from All Hands AI, Devin as Cognition's commercial cloud agent.
- And the tools developers actually reach for daily, so Claude Code, Cursor, Copilot, Codex, are productised descendants: the same observe, think, act loop, the same purpose-built interface, the same execution-grounded refinement.

Same ingredients, deployed as IDE assistants, autonomous PR bots, and CI jobs. So the patterns we just dissected are the patterns in the tools you already use.

## SWE-bench

Let's talk about benchmarks. There are a ton of them out there, but one of the first and the one most used by research papers is SWE-bench.

It's a suite of tasks. Each task is a real, merged pull request from a popular open-source project: Django, scikit-learn, matplotlib. One that both closes a GitHub issue and touches the test suite.
Nothing synthetic: a human reported the bug, a maintainer fixed it, the fix shipped.

Real codebases, hundreds of thousands of lines, with the maintainer's own patch as ground truth. Not toy problems.
The flip side is a monoculture: Python only, twelve projects, a limitation we come back to.

- The test filter gives you a grading oracle.
- The issue filter gives you the intent.

Task: the repository before the fix, plus the issue text.
Produce a patch.
- Fail-to-pass tests must flip,
- pass-to-pass must not regress.

Resolve rate: both sets pass, on the first patch.

That is one task, pass or fail. Next: how you turn thousands of them into a single number worth comparing.

## SWE-bench: metrics

Not string similarity, because two correct patches may share no token.

- Functional correctness: does it run and pass? The only thing that counts.
- Pass-at-k: give it k tries, count it solved if any one passes.
- Resolve rate: pass-at-one over a whole split, solved on the first try.
- Cost: dollars, calls, time. A higher score can cost much more.

Every metric scores the end of the pipeline.
Plan quality, localisation, refinement efficiency: all unmeasured.

So we settle on one number, resolve rate. But there is no single SWE-bench. It's a family of splits, and which one a paper quotes changes the story.

## The five splits

Roughly two years of splits, oldest to newest.
- Full, Oct 2023: everything.
- Lite, Mar 2024: three hundred easy single-file tasks, some of which leak the fix.
- Verified, Aug 2024: five hundred, human-screened.
- Multilingual, May 2025: nine languages.
- Pro, Sep 2025: long-horizon, multi-file, held-out repos.


## How hard is each task?

How reliable is the benchmark?

We'll focus on the Verified split from here on.

When OpenAI built Verified, annotators estimated a fix time for each issue: how long a skilled engineer would need to write the fix.


## The headline climbs

The same nine systems, April 24 to March 25. Four lines, all one dataset.
Blue is the overall rate: eighteen to sixty-five percent, SWE-agent to Augment.
Orange is easy, green medium, red hard.
Watch the spread: easy is near eighty, hard is stuck at twenty.
The overall line is just the tier-weighted average, so it hides that gap.
The next slide reads off the exact splits.

Easy and medium are ninety-one percent of Verified. The hard tier stalled near twenty percent. A system can gain ten overall points while solving no new hard task.

So the same number both drives progress and hides where it stalls. That tension hands us into the last part: where these systems actually break.

By early 2026 OpenAI retired Verified, since the frontier models and agents reach eighty percent. They have moved on to Pro.

So what are the challenges on the hard, long-horizon, multi-file tasks?

## 1. The agent never asks

Intent capture is the one stage with no feedback edge. The agent reads the issue once, decides what it means, and never revisits that. Nothing ever asks it to check. If the tests don't cover the misunderstanding, the wrong fix still passed.

## 2. The repo doesn't fit

The second challenge is that the repo doesn't fit in the context window. Before the agent can be right, it has to choose what to look at, and that choice is most of the problem.

Hard tasks are disproportionately multi-file, which is exactly where agents stall.


## 3. Passing the tests is not being right

Third: the oracle we lean on is not the thing we actually want. A test suite is a sample of intended behaviour, not the behaviour itself. So a patch can turn the tests from failing to passing and still be wrong.

Worse, that same verdict is the stopping rule: the loop stops when the tests pass.

## Takeaways

- Agentic beats one-shot because it can run things, not because it reasons more.

- Three of the four are the same gap: nothing outside the model catches a wrong guess about intent, a wrong file, or a wrong fix. The fourth says you cannot buy your way past it.

- Verification is the bottleneck. In a randomised trial, experienced developers worked real issues on their own repositories, half the tasks with AI allowed and half without.

With AI they generated code much faster. But a lot of time went into prompting, reading and repairing the output.

They ended up 19% slower, and thought they were 20% faster.

## Backup: can we trust the numbers?

Verified against Pro, same model weights.
Everything drops twenty points or more.

Read the floor, not the spread. Only Opus 4.5 uses the standardised harness.
The others self-report.
The one measured most strictly drops furthest.

- Contamination: OpenAI dropped Verified in 2026, because models reproduce gold patches from the task ID alone.
- Reward hacking: closing test leaks cuts scores by double digits.

If someone asks why the tier table is from 2025: because that is where the
stratified data stops. By early 2026 the frontier had saturated Verified near
eighty percent and OpenAI retired it as contaminated, so nobody re-ran the
per-difficulty breakdown. The 2025 snapshot is the last clean read, and the live
frontier signal has moved to Pro.

## Backup: surveyed systems on SWE-bench Verified

The best published result for each of the five surveyed systems on Verified, pulled from swebench.com.
Devin's number is on a different, easier split, self-reported, and never reproduced. Read it as context, not as a comparison.

## Backup: how planning points at real code

So how does it stop guessing? By pointing at real code. A tree lets the model try several guesses, but nothing has run yet, so it narrows the actual repository from broad to specific, and every step names code it can check.
- Agentless: files, then classes and functions, then edit locations.
- AutoCodeRover: AST-backed search APIs, coarse to fine.

The useful plans name concrete program elements you can check against the repository, not invented requirements.

## Backup: localisation in Agentless

Narrowing without letting the model wander: three fixed steps, coarse to fine.
- Rank files: from the repo layout and the issue, shortlist the files most likely involved.
- Rank elements: inside those files, narrow to specific classes and functions.
- Pin locations: inside those, the exact lines to edit.

Three prompts, no search, no tools. The narrowing is hard-coded into the scaffold, so it's fixed rather than discovered.

## Backup: localisation in AutoCodeRover

Let the model navigate, but through the syntax tree rather than whole files.
- Search APIs: search_class, search_method, search_method_in_class.
- The agent calls them coarse to fine, pulling in just the code it needs.
- AST-backed: structured, cheaper context than dumping whole files into the prompt.

The model chooses what to search. Agentless, by contrast, is a fixed funnel.
