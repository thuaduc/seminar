# Speaking script — Agentic AI for Code Generation

15-minute seminar talk. Companion to `main.tex` (the deck).

**Thesis:** planning and refinement form a control layer over generation, but only where a
signal the generator did not produce can contradict it.

**Act I** (slides 2–9) sells the good news. **Act II** (10–14) reads it again against one
instrument: the three-position axis — oracle / proxy / self-critique. The axis is applied to
Act I's results in the conclusion.

Act I's shape: the ruler (2), the first surprise (3), then the **pipeline and a walk through
its stages** (4–7), and only then real systems (8–9). Stages before systems — the table is a
claim, and the stages are the vocabulary it's expressed in.


`Target` is the budget. `Script` is what the bullets below actually contain, at 140 wpm —
measure it again if you rewrite anything, because a script that overruns is a script whose
*unanchored* material gets dropped at the podium, and that is exactly the material carrying
the argument.

| #   | Slide                                                  | Target | Script   |
| --- | ------------------------------------------------------ | ------ | -------- |
| 1   | **Opening** (title slide)                              | 0:50   | 1:27 ⚠️  |
| 2   | What SWE-bench measures                                | 0:55   | 1:46 ⚠️  |
| 3   | No scaffold wins twice                                 | 1:20   | 0:57     |
| 4   | The agent pipeline                                     | 0:50   | 0:28     |
| 5   | Planning is localisation, not design                   | 1:10   | 1:49 ⚠️  |
| 6   | Execution: the code meets the environment              | 0:45   | 1:12 ⚠️  |
| 7   | Refinement: what to do with the signal                 | 0:40   | 0:51     |
| 8   | A map of the design space, not a ranking               | 1:00   | 1:39 ⚠️  |
| 9   | Interface and context move scores as much as the model | 1:10   | 1:10     |
| —   | *divider — Act II*                                     | 0:10   | —        |
| 10  | The question this talk asks                            | 0:30   | 0:31     |
| 11  | What can tell the model it is wrong? Three answers     | 1:50   | 1:52     |
| 12  | The benchmark can be gamed                             | 1:20   | 1:18     |
| 13  | Where a wrong guess is never contradicted              | 1:45   | 1:45     |
| 14  | Conclusion — three things the numbers showed           | 0:55   | 1:06     |

**Act I: 8:40 target, 11:21 of script — still ~2:40 over, and untouched.** That is the next
job. Act II: 6:00 target, **6:32** of script (it was 9:32).

**Act II's centre of gravity is now slide 13, not 12.** It used to be the other way round:
12 spent nearly two minutes narrating how SWE-bench Pro is built, while 13 — the slide that
*is* the thesis — carried no evidence at all, only assertion. The two figures that measure
the two ungated stages (**38 % / 61 %**) now sit on 13 where they belong, and the one Pro
construction detail that mattered (Scale pays humans to rewrite every issue) went with them.
**Say the numbers; do not re-narrate the benchmarks.**


---

## 1 — Opening (title slide)

- **The topic.** Autonomous coding agents: you hand the system a GitHub issue, and it reads
  the repository, writes a patch, runs the tests, and keeps going until they pass — with no
  human in the loop. Two years ago these solved about 2 % of real issues; today the leaders
  report over 70 %.
- **What I'll cover.** I survey five of these systems, and I organise them around a
  five-stage pipeline: intent → planning → generation → execution → refinement.
- **The question I ask of them.** Every one of these systems is a model that writes code,
  wrapped in machinery that plans before it and checks after it. So: **what is that
  machinery actually contributing?**
- **What this talk contributes, stated plainly.** I'm not presenting a new agent or a new
  benchmark. The contribution is the lens itself — the five-stage pipeline plus the
  oracle/proxy/self-critique axis — applied to the field's own published numbers, to show
  that the reported gains concentrate in exactly two structurally ungated stages.
- **The structure.** Two halves.
  - **What works** — the evidence that the machinery earns its place.
  - **What the numbers hide** — the same evidence, read again more carefully.
- **Scope.** Repository-level agents only. Not autocomplete, not code search.

## 1 → 2

*"Every number I'm going to show you is a score on one benchmark. So let me spend one
minute on what that benchmark actually measures."*

## 2 — What SWE-bench measures

- One instance = a real merged pull request that **both** closes a GitHub issue **and**
touches the test suite.
- The agent sees the repo *before* the merge, plus the issue text. It returns a patch.
- Graded by running tests: fail-to-pass must flip, pass-to-pass must not regress.
- **Resolve rate** = both hold, first patch. Every percentage in this talk is this number.
- Those two filters do different jobs — and this is the whole talk in miniature:
  - the **test filter** is what gives the benchmark a grading oracle;
  - the **issue filter** is what gives the task an intent.
- Splits are not comparable. Verified is the most *curated* split, not the hardest — humans
threw out unclear issues and unfair tests. Everything I quote is Verified unless I say so.
- Lite is the least trustworthy: some issue texts leak the fix outright.
- **Who made which** — worth having straight, because the two are easy to confuse:
  - **Verified is OpenAI's.** 93 Python developers hand-screened **1,699** random SWE-bench
  instances; **500 survived.** *Why the other two-thirds didn't is my last argument, so I'm
  saving it.* The difficulty tiers I use later (< 15 min / 15 min–1 h / 1–4 h / 4 h+) are
  theirs too.
  - **Pro is Scale AI's** — a different organisation, deliberately harder (slide 12).
- Do **not** spend the rejection breakdown here. Those two numbers are the evidence for
slide 13, and they only land once. Plant the 1,699 → 500; let 13 say what happened to the
1,199.

## 2 → 3

*"So that's the ruler. Now the first measurement — and it isn't about the model."*

## 3 — No scaffold wins twice

- Three scaffolds × three models. Every row holds the **model and the benchmark fixed** and
changes only the scaffold. That's the only way to attribute anything to the scaffold.
- **First reading:** the scaffold alone is worth **11–17 points**. Same order as a model
generation.
- **Second reading, the real one:** the winning column changes row by row. mini-SWE-agent —
a hundred lines of Python — is *best* for GPT-5-mini and *worst* for DeepSeek R1 (11.0 vs
SWE-agent's 22.6).
- There is no "best scaffold". Scaffold and model interact. Naming a winner tells you
nothing until you name the model.
- Caveat, unprompted: the budgets are **not** normalised across scaffolds (dollar cap vs
step cap). This isolates the scaffold, not the budget — a limitation of this comparison,
not a claim I resolve later.

## 3 → 4

*"Scaffolds matter that much — so what is a scaffold actually made of? Five stages."*

## 4 — The agent pipeline

- Every paper uses its own vocabulary. Underneath, nearly all do these five things.
- A **lens, not an architecture** — a device for comparing systems that share no vocabulary.
- Feedback flows backwards from execution into refinement, and from refinement into
planning and generation.
- **The dotted edge:** nothing re-enters intent capture. Going back there is a *new task*,
not a correction. Nothing here ever fixes a misread issue.

## 4 → 5

*"Five boxes. Three of them are where the argument lives, so let me take those one at a
time — plan, execute, refine — and say what each actually does. Then we'll look at real
systems."*

## 5 — Planning is localisation, not design

- **Where it sits.** Second stage of the pipeline: input is an issue, output is *a place to
  edit*.
- **What a "plan" actually is here.** Not a design for the code — not "first I'll add a
  helper, then refactor the caller." It is a **narrowing**: repository → candidate files →
  classes and functions → the exact edit locations. Planning, in these systems, is
  *localisation*.
- **What shape a plan takes.** A linear chain; a tree that can backtrack; a graph that
  merges branches back together. (Backup slide has the picture.)
  - But branching is only worth anything if something can **score a half-finished patch** —
    and at repository scale, before a test has run, nothing can. The evaluator, not the
    branching, is the hard part. SWE-Search has to build one, and pays a model call at every
    node.
- **What planning contributes: grounding, not deliberation.** Tying the hypothesis to
  artefacts that can be *checked* — real files, real symbols — rather than thinking harder
  in the abstract.
- **Two witnesses for that claim:**
  - **Agentless** plans least — a fixed localise → repair → validate pipeline, no loop — and
    still leads. It *hard-codes* the narrowing an agentic loop has to rediscover by
    exploring. Same grounding, none of the search. (You'll see it top the table in three
    slides — this is why.)
  - **AutoCodeRover's** SBFL variant changes *nothing* about repair — only localisation — and
    buys three points. Finding the right place is harder than editing it once found.
- **Planning is not reasoning about *how* to write the code. It is reasoning about *where*
  the code is.**

## 5 → 6

*"The plan says where. Generation writes the patch. Then the patch has to meet something
that isn't a language model."*

## 6 — Execution: the code meets the environment

Keep this one **short and concrete**. It is the stage everyone skips, and the whole of Act II
turns on it — but its privilege is *not* argued here. Name the machinery; slide 11 names what
makes it special.

- **Where it sits.** Input: a patch. Output: **evidence**.
- **What actually runs.** The repository, checked out at the parent commit, inside a
  container. The agent runs the test suite, the interpreter, the linter. This is a real
  machine executing real code — not a model predicting what would happen if it did.
- **What comes back is a *symptom*, not a score.** A traceback, three failing tests, a
  rejected edit. Nobody hands the agent "you are wrong, here is why" — it gets a stack trace
  and still has to *read* it. That reading is the next stage.
- **Why it's the expensive stage.** Every observation is a container round-trip, and the
  transcript grows with each one. Cost is superlinear in turns — the context slide in the
  backup has the numbers if anyone pushes.
- **Land it, then move on:** *"the only stage whose output the model did not write."* Say it
  flatly and don't explain it yet. It should feel like a passing remark now and like a
  hinge in eight slides.

## 6 → 7

*"So execution hands back a symptom. Refinement is the stage that decides what to do with
it."*

## 7 — Refinement: what to do with the signal

Thin on purpose — the *thinking* about refinement is Act II. Here: name the stage, show the
loop pays, set up the turn.

- **Where it sits.** Input: whatever execution returned. Output: a **decision** — repair the
  patch, re-plan, or stop.
- Three ways agents do it, cheapest first:
  - **Repair from the trace.** The traceback goes straight back into the prompt.
  - **Explain your own code.** The model narrates its own patch; discrepancies surface. Up to
  +12 % — but note the condition: *only where tests exist*.
  - **Sample N, rerank with a trained critic.** OpenHands on Verified: 60.6 → **66.4 %**.
- Read plainly: **agents improve their own code.** Every number here is real and published.
  Deliver this straight — no irony. The audience should leave this slide believing it.
- *"…or so this reads."* — say it lightly, and move on. (Don't unpack it. Slide 11 does.)

## 7 → 8

*"That's the pipeline. Now — five real systems, and how differently they fill those boxes
in."*

## 8 — A map of the design space, not a ranking

- SWE-agent, AutoCodeRover, OpenHands, Devin: observe–think–act loops with different
interfaces and context strategies.
- **Agentless is the odd one out — and it leads.** Not an agent at all: a fixed three-phase
pipeline (localise, repair, validate), no loop. **And you already know why** — three slides
ago: planning is localisation, and Agentless hard-codes the narrowing instead of searching
for it. The table is where that claim cashes out.
- **State the table's weakness before anyone asks.** Two things are wrong with it:
  - the rows **don't share a backbone** (GPT-4 Turbo vs GPT-4o vs Sonnet 3.5), and swapping
  the backbone alone can move a resolve rate ten points;
  - there is **no split every system reports**. Agentless and OpenHands were never run on
  Full at all — their authors cite compute cost — which is why the comparison is on Lite.
  And Lite is the split I just told you leaks.
- So read this as a **map of the design space**, not a ranking. It cannot settle anything.
- **What actually carries the Agentless claim:** its authors *also* ran the agentic
baselines on their own GPT-4o backbone — model held fixed — and the gap held. That
matched comparison is the evidence, not this table.
- Devin is here for architectural influence, not as a data point: vendor claim, no paper,
self-reported on a random 25 % subset, never reproduced. Independent trial solved 3 of 20.

## 8 → 9

*"Different as they are, three things are common to all of them — and one of them is where
generation actually gets its hands."*

## 9 — Interface and context move scores as much as the model

- **The observe–think–act loop.** Observe, reason, act via a tool call. Near-universal.
- **Context as a managed resource.** Repos exceed any window, so every system decides what
to read: skeletons, AST search, observation compression.
- **The action interface.** The model can't touch the repo directly — it only gets the
commands we hand it, and only sees what those commands print back. *That* is the
interface. **This is the generation stage's real content:** the question isn't "can the model
write code" — it's *what is it allowed to do*.
  - The lazy design is a bash shell, because that's what a human uses. But the model isn't
  a human user: a human scrolls a file and spots a broken paren; the model gets a wall of
  text and has no eyes.
  - So design for the model: a 100-line window, an `edit` the linter can **refuse**,
  summarised search.
  - Take one part away and re-measure — same model, same benchmark: raw search −6 pp, whole
  files −5.3, no linter −3.

## 9 → 10  *(section divider — Act II)*

*"I've shown you the good news. Now I want to take some of it back — with one question."*

## 10 — The question this talk asks

- **At each stage, what can tell the model it is wrong?**
- This is what the pipeline exists to make askable.
- As a control system: intent is the uncontrolled input, generation is the plant, execution
is the oracle, planning and refinement are the control layer.
- A refinement mechanism is a control loop *only* if something outside the model's own
judgement can contradict it. Otherwise it's an open loop in a control loop's clothes.

## 10 → 11

*"Answer that question honestly and the feedback signals sort into three kinds, not two."*

## 11 — What can tell the model it is wrong? Three answers

The pivot of the talk. Do not rush it, and do not assume "surprise" is self-explanatory — it
isn't. Introduce it in plain words *first*, then use it as shorthand for the rest of the
talk. The two small lines now on this slide — the answer key, and reorganise-vs-add — are
defensive: each closes an objection a sharp listener will otherwise raise in Q&A.

> *"Can this signal tell the model something it did not already believe? That's the whole
> test. I'll call that **surprising** the model."*

- **Authoritative oracle** — test runner, compiler, human. A failing test contradicts the
model however confident it was: the code was *run*, and reality answered. That is what it
means to surprise it. Costs infrastructure; slow.
- **Learned / generated proxy** — a trained critic, generated tests, an LLM judge. It carries
real information: it was fitted against ground truth the generator cannot consult. But never
*more* than it was fitted with.
  - **The line to land:** a generated test *runs* for real — but the model wrote the answer
  key. This axis is not about whether code *executes*; it is about where the *specification*
  came from.
  - A proxy **can** disagree with the generator — worth 5.8 points. What it cannot do is
  bring in information from *outside* the models involved. So it is wrong precisely where the
  model was wrong.
- **Same-distribution self-critique** — the model judging itself. Free, fast, and it inherits
the blind spots of the error it is meant to catch. Huang et al. found no reliable gain, and
some degradation — *on reasoning tasks.* Generalising to code is **my** claim, not theirs.
- **An agent that could tell its wrong code from its right code by inspection would have
written the right code in the first place.**
  - Too strong alone — taken literally it would rule out the reranking gain I just credited.
  The precise claim is the small line beneath: self-critique can **reorganise** what the
  model believes; it cannot **add** to it.

## 11 → 12

*"Hold that axis in mind. Now look again at the headline numbers everyone quotes."*

## 12 — The benchmark can be gamed

Lead with the line at the top of the slide — it is this slide's licence to exist. Without it,
a benchmark critique dropped into the middle of a control-theory argument reads as a
digression. And do **not** re-narrate Pro's three tiers or its 107-lines-across-4.1-files:
that cost a full minute and bought nothing the floor does not already buy. The one Pro
construction detail that matters now lives on slide 13, where it does argumentative work.

- **The grading tests *are* the oracle** — the agent's, and ours. Same suite, both jobs. So
this is not a change of subject: it is the axis, applied to the instrument. **A weak oracle
makes optimising against it overfitting.**
- The **same weights** drop ≥ 20 points on SWE-bench Pro — Scale AI's, not OpenAI's, and
contamination-resistant *by construction*: copyleft repos a commercial lab would have
excluded from training, plus held-out proprietary code no model has seen.
- **Read only the floor.** Pro is harder *as well as* fresher, and the column mixes
harnesses — I won't claim the drop *size* measures memorisation. The floor is what survives.
- **What the headline is a score on.** 91 % of Verified is work its own annotators judged
finishable **within the hour** — only ~45 of 500 are hard. A system can gain points while
solving no additional hard task.
- **OpenAI dropped Verified in early 2026** — models reproduce gold patches from the task ID
alone. An admission from the people who *built* it. And: **reward hacking is swamping model
intelligence gains.** That is the top line, cashed out.

*If asked why there's no difficulty-split chart:* the per-instance results that make one
computable stopped being published in mid-2025 — vendors now self-report a single number. The
composition fact holds; the time series can no longer be built.

## 12 → 13

*"A weak oracle. Now — the two places in the pipeline where there is barely one at all."*

## 13 — Where a wrong guess is never contradicted

The payoff slide, and the one that used to carry no evidence at all. It now holds the two
numbers Act I deliberately withheld. Take the time — this is where the argument is cashed.
Guard the title as you say it: the claim is *not* that nothing checks these stages (a test
suite does say no, to everything it covers), and the old title — "nothing can check" — was
contradicted by this slide's own right-hand block. The exact shared property is narrower and
worse, so say it in those words:

> *"A wrong guess, in either of these two stages, is never **contradicted**."*

- **Intent capture — 38.3 %.** OpenAI paid 93 developers to screen 1,699 real SWE-bench
issues. They **flagged 38.3 % as underspecified.** That is this stage's failure rate in the
wild, measured by the people who built the benchmark.
  - No surveyed system asks a clarifying question; each commits to a reading. **Open loop.**
  And the mechanism is **not missing** — TiCoder, ClarifyGPT, and at repo scale the agent
  that *asks* beats the agent that guesses. The gap is **transfer, not invention.**
- **Test-based validation — 61.1 %.** The same screen flagged **61.1 % for tests that may
unfairly mark a valid solution incorrect.** The suite is not a trustworthy account of what
"correct" means.
  - And that is the failure we can *see* — a human can look at a good patch the tests
  rejected. The one we *cannot* see runs the other way: a wrong guess about an **untested**
  invariant is never contradicted, and scores as success. Measuring *that* would need a
  better oracle — the very thing we do not have.
- **The punchline — slowly.** Both flagship benchmarks **hand-solve these two stages before
the agent starts.** Verified *discards* **68.3 %** of everything it screened. Pro keeps the
hard issues and pays **human experts to rewrite every one.** Two organisations, two methods,
one concession: **nothing in the pipeline can do intent capture on its own, so a person does
it first.**
- **No agent does either. Nothing ever asked it to.**

**Know the threshold before you stand up.** OpenAI's severity scale is 0–3; 0–1 are minor,
2–3 severe, and the *discard* filter was severity ≥ 2. But the post never says whether the
headline 38.3 % / 61.1 % count severity ≥ 1 or ≥ 2. So use their verb — **"flagged"** — and
their hedge — **"may unfairly mark a valid solution incorrect"**. Do not upgrade these to
"too underspecified to determine the patch" or "would reject a valid fix": you cannot cash
that if asked at what severity. *If asked:* say plainly that the post reports the flag rate
and separately states the discard threshold is 2, and that 68.3 % were in fact discarded —
which is the number that does the work anyway.

## 13 → 14

*"So, to close."*

## 14 — Conclusion: three things the numbers showed

Fifty-five seconds. One sentence per bullet, then stop. This slide used to run two and a half
minutes — everything cut from it had already been said once, and a conclusion that re-argues
is a conclusion nobody remembers. Say the slide numbers out loud: the claim is that the
evidence was *shown*, not cited.

- **1.** *(slide 3)* The scaffold alone was worth **11–17 points** — a model generation — but
the winning column changed row by row. What the wrapper *feeds* the generator does the work.
- **2.** *(slide 7, against the axis on 11)* **Execution** grounds the gains. A **trained
critic** is real — 60.6 → 66.4 — but it is still a model: it cannot *surprise* the generator.
**Self-critique** alone: no reliable gain. The control layer is exactly as strong as its
signal.
- **3.** *(slides 12–13)* **Intent** and **validation** have no gate — **38 %** and
**61 %** — and the benchmark hid it. **And yet:** the one paper that *instrumented* intent
lifted **all nine** cells of the grid I opened with — its own control condition.
- **Close.** Whether this crosses from *impressive* to *dependable* depends on whether we
learn to let the environment say no to the model — because on this evidence, the model will
not say it to itself.

*Held for Q&A, not spoken:* the cheap experiment nobody has run — hand the agent the gold
file set, making localisation oracular by construction. If hard-tier rates rise, the ceiling
was retrieval and I'm wrong. If they don't, only training will move it. I expect the latter,
and the experiment could prove me wrong.

---

## Backup slides (Q&A)


| Slide                                  | Reach for it when asked                                                                                                                                                                        |
| -------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Every refinement mechanism, sorted     | "Where does mechanism X sit on your axis?"                                                                                                                                                     |
| How plans are shaped (CoT / ToT / GoT) | "What about tree search / MCTS?" — the evaluator, not the branching, is what makes structure useful, and it's what a repo-scale agent finds hardest to supply.                                 |
| Context and cost                       | "Won't longer context windows solve this?" — dependency context helps, padding *hurts*; cost is O(n²) in turns; METR's RCT had developers 19 % *slower* while believing they were 20 % faster. |


