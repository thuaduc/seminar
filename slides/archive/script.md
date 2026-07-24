# Speaking script — Agentic AI for Code Generation

**20-minute** seminar talk (hard ceiling). Companion to `main.tex` (the deck).

**The story:** in three years, AI that writes code went from completing the line you are
typing to autonomously resolving real GitHub issues — under 2 % in 2023, over 70 % in 2026.
This talk says what an agent *is*, walks the five-stage pipeline every surveyed system fills
in, defines the one number everything is measured by, reads the climb off the systems the
report actually surveys — **and then shows what the climb hides: split by difficulty, the
hard tier stalled near 20 % in late 2024 and has not moved.** That stratification is the
talk's one insight, and everything before it exists so the audience can read it.

> **RESTRUCTURED 2026-07-19.** The four-act "asymmetry of verification" version of this talk —
> the law, the turn, the checker axis, Olausson, the jagged edge, the benchmark audit — was
> deliberately cut. This script matches the new 11-slide linear deck. The old script is in git
> history. The cool-point question is settled: slide 10 is the difficulty-stratified
> trajectory; the scaffold grid moved to backup.

---

## The shape of the talk

**One linear story, no acts, no outline slide, no section dividers.** The spoken transitions
are the only signposts, so say them.

**Concepts before measurement.** Slides 4–7 carry no percentages: a resolve rate means nothing
until the ruler slide defines it. The one exception is the hook pair on slide 3 (under 2 % →
over 70 %), which is flagged on its own slide as sitting on two different rulers. Every other
number in the talk appears after the ruler is defined.

**The numbers are told on the surveyed systems.** The climb slide uses only systems the report
covers — no leaderboard names the audience will never hear again.

**Pro numbers are measured numbers.** Wherever Pro is quoted, it is Scale's own uniform-harness
leaderboard (≈55–60 % for the July-2026 leaders), never vendor self-reports (70–80 %, mixed
harnesses, verified by nobody). If pushed on Pro's reliability, the July-2026 OpenAI audit
(~30 % of public tasks flagged as broken) is on the backup benchmark slide — concede it freely;
it is why the deck says "measured" and attaches a caveat.

---

## Running order

> ⚠️ **THE TIMINGS BELOW ARE ESTIMATES AND MUST BE RE-MEASURED.** Rows whose content did not
> change carry their old measured value; rewritten rows are estimates at 140 wpm. Read it aloud
> with a stopwatch before you trust the total.

| #   | Slide                                                  | Script  |
| --- | ------------------------------------------------------ | ------- |
| 1   | **Opening** (title slide)                              | 0:20    |
| 2   | **What is agentic AI for code generation?** *(figure)* | 1:00    |
| 3   | **Three years, three eras** *(figure)*                 | 1:00    |
| 4   | The agent pipeline *(figure)*                          | 1:00    |
| 5   | Planning is localisation, not design *(figure)*        | 1:10    |
| 6   | Execution: the code meets the environment *(figure)*   | 0:51    |
| 7   | Refinement: what to do with the signal                 | 0:40    |
| 8   | What SWE-bench measures                                | 1:05    |
| 9   | **Three years, in numbers** *(figure)*                 | 1:30    |
| 10  | **What the climb hides** *(figure)*                    | 1:15    |
| 11  | Conclusion                                             | 0:50    |

**Estimated total: ~10:45 at 140 wpm** — with real delivery 10–15 % slower, budget
**12:00–12:30**. **The deck is well under the 20-minute ceiling.** That is deliberate slack:
it belongs to slower delivery, pauses on the two figure slides at the end, and Q&A. Do not
pad the existing slides to fill time.

---

## 1 — Opening (title slide)

Twenty seconds. Name, title, supervisor, move. **Do not argue anything here** — the next two
slides are the argument, and they have pictures.

## 1 → 2

*"Let me start with what the field actually is — because 'AI that writes code' has meant three
different things in three years, and only the third one needs any of this talk."*

## 2 — What is agentic AI for code generation?

**The definitional slide.** Nothing later in the deck has a subject until this lands.

**Walk the figure left to right. Let them derive the definition; don't hand it over.**

- **Two things everyone here has already used.** *Autocomplete* — it completes the line you are
  typing. *One-shot generation* — you write a docstring, it writes the function.
- **In both, notice who does the work that matters.** *You* run the code. *You* read the error.
  *You* re-prompt. **The human closes the loop.** The model never finds out whether it was
  right.
- **An agent is the system that closes its own loop.** Give the model tools and a real
  environment, and it plans, edits files, runs the test suite, reads the traceback, tries
  again. **That** — not the code writing — is what "agentic" is doing. Land it flat; it is the
  sentence the conclusion comes back to.
- **The task arrives wherever the developer already is.** A GitHub issue. A pull-request
  comment. A Slack thread. A ticket. A line in the terminal. **Plain language in — a code
  change out.** *(The bold line is all the slide keeps — the list is yours.)*
- **The measurement aside — yours alone now, it is no longer on the slide:** measured, almost
  always, on one narrow slice of that — resolving GitHub issues. *That is how the field is
  measured, not what it is.* Six words now; the ruler slide cashes them.

## 2 → 3

*"So where did these systems come from? Three years, three eras."*

## 3 — Three years, three eras

**The progression slide — walk the timeline left to right, one era per breath.**

- **2021 — autocomplete ships.** Copilot. The unit of work is the line you are typing.
- **2022 — one-shot generation.** ChatGPT. The unit of work is a function, from a docstring.
  Still: you run it, you read the error, you paste it back.
- **2023 — the task gets real.** SWE-bench arrives: not toy functions — actual GitHub issues
  from real repositories. And the best systems of the day resolve **under 2 %** of them.
- **2024 — the agents arrive.** SWE-agent, Devin, OpenHands — systems that hold the tools
  themselves.
- **2026 — autonomy at scale.** Leaders report **over 70 %**.
- **The trap — say it yourself, before anyone else can.** *Those two numbers are not on the
  same ruler.* The 2 % is the full benchmark; the 70 % is the human-curated split. The caveat
  is printed on the slide — point at it. **Plant it and move on**; the ruler slide pays it off.

*Why quote a pair you immediately caveat? Because the jump is real, and part of it is genuine —
and "what happened in between" is this talk.*

## 3 → 4

*"To compare these systems at all, the report reads every one of them through the same lens —
five stages."*

## 4 — The agent pipeline

- **The five stages, in one breath:** intent capture reads the request; planning narrows it to
  *where* to edit; generation writes the patch; execution runs that patch for real; refinement
  decides what to do with whatever comes back.
- **The feedback edges are the agentic part — point at them.** Execution feeds refinement;
  refinement either repairs, or goes back and re-plans. That loop is the entire difference
  between this and one-shot generation.
- **The dotted edge, briefly.** Nothing re-enters intent capture: going back there is a new
  task, not a correction. State it as a fact of the diagram and move on.
- **The closing line, flat:** five stages, one vocabulary — every system in this talk fills
  these boxes, each in its own way.

## 4 → 5

*"Three of those five are where the interesting design decisions live — plan, execute, refine.
One at a time."*

## 5 — Planning is localisation, not design

The funnel does the narrowing for you — **do not read the boxes out loud.** Spend the time on
the **boundary**: it is the only thing here an audience cannot guess, and it pre-empts the
objection anyone who knows the APR literature will otherwise raise.

- **What planning turned out to be.** Not task decomposition, not design documents:
  **localisation**. The issue says *what* is wrong; the plan's job is **where** — which files,
  which functions, out of tens of thousands.
- **What it contributes: grounding, not deliberation.** Tying the hypothesis to artefacts that
  can be *checked* — real files, real symbols — rather than thinking harder in the abstract.
- **The boundary — the point of the figure.** Localisation **flips sign** at the function.
  *Above* it, localisation is the task: which function, among tens of thousands? AutoCodeRover
  ranks functions by suspiciousness, and it helps. *Below* it, it **hurts** — D4C hands the
  model the failing test and the whole function, gives it **no line-level pointer at all**, and
  beats systems given *perfect* localisation. Line-level targeting fights the model's infilling
  prior. Two results that look opposed, measuring opposite sides of one boundary.
- **The numbers are yours to say, not the slide's.** SBFL buys AutoCodeRover **three points**;
  D4C beats perfect-localisation systems by **ten per cent**. Deliberately not on the slide —
  the audience has no ruler yet. **Say them; don't show them.** They are on a backup slide if
  anyone wants them written down.
- **Agentless is the witness.** It plans least — a fixed localise → repair → validate pipeline,
  no loop — and it is the strongest 2024 point on the numbers slide you'll see later: it
  *hard-codes* the narrowing an agentic loop has to rediscover by exploring.
- **Planning is not reasoning about *how* to write the code. It is reasoning about *where* the
  code is — and only down to the function.**

✂️ *If over time, cut the D4C half:* say only *"localisation is the task above the function, and
stops being useful below it,"* and let the figure carry the rest. **≈ 20 s.**

*Plan shape (chain / tree / graph) is a backup slide only.* If asked: branching is worthless
unless something can score a half-finished patch, and at repository scale nothing can — the
evaluator, not the branching, is the hard part. SWE-Search has to build one, and pays a model
call at every node.

## 5 → 6

*"The plan says where. Generation writes the patch. Then the patch has to meet something that
isn't a language model."*

## 6 — Execution: the code meets the environment

Short and concrete. **The figure argues this slide for you.** The vertical rule — the model on
one side, a real machine on the other — is the thing to point at, not to explain.

- **What actually runs.** The repository, checked out at the parent commit, inside a container.
  The agent runs the test suite, the interpreter, the linter. **A real machine executing real
  code — not a model predicting what would happen if it did.**
- **What comes back is a *symptom*, not a score.** A traceback, three failing tests, a rejected
  edit. Nobody hands the agent "you are wrong, here is why" — it gets a stack trace and still
  has to *read* it. That reading is the next stage.

*Cost is superlinear in turns — the backup slide has the numbers if anyone pushes.*

## 6 → 7

*"So execution hands back a symptom. Refinement decides what to do with it."*

## 7 — Refinement: what to do with the signal

Thin on purpose: name the stage, list what people do, move on.

- **Where it sits.** Input: whatever execution returned. Output: a **decision** — repair,
  re-plan, or stop.
- Three ways agents do it, cheapest first:
  - **Repair from the trace.** The traceback goes straight back into the prompt.
  - **Explain your own code.** The model narrates its own patch line by line; discrepancies
    surface. Note the condition out loud: *only where tests exist.*
  - **Sample N, rerank with a trained critic.** Generate many candidates, score them, ship one.
- **Read plainly: agents improve their own code.** Every mechanism here is real and published —
  and one of them shows up with its number on the climb slide in two minutes
  (the trained critic; don't spend the number here).

## 7 → 8

*"Every number I'm about to show you is a score on one benchmark. So — one minute on what that
benchmark actually measures."*

## 8 — What SWE-bench measures

**The ruler, arriving exactly where it is needed:** the next slide is unreadable without it.

- One instance = a real merged pull request that **both** closes a GitHub issue **and** touches
  the test suite.
- The agent sees the repo *before* the merge, plus the issue text. It returns a patch.
- Graded by running tests: fail-to-pass must flip, pass-to-pass must not regress.
- **Resolve rate** = both hold, first patch. **Every percentage from here on is this number.**
- **The splits — the slide keeps four names; the details are yours.** Full is everything
  scraped. Lite is 300 single-file tasks — *and the least trustworthy: some issue texts leak
  the fix outright.* Verified is 500 instances OpenAI paid 93 developers to hand-screen —
  the most *curated* split, not the hardest. Pro is Scale AI's: held-out repos, multi-file,
  deliberately harder.
- **The one retained fact:** *rates are not comparable across splits.* Say which split every
  number you quote is on; the next slide tags every point.

## 8 → 9

*"So that's the ruler. Now — the climb, told on the systems this report actually surveys."*

## 9 — Three years, in numbers

**The payoff of the first half. Walk it point by point, left to right, with your hand.** Every
point is a system the audience has now met by stage: they know what a plan, a run, a
refinement is — so each point can be one sentence.

- **2023, the baseline.** Retrieval-augmented prompting of a frontier model: **under 2 %**, on
  the full set. This is the "before" photo.
- **Early 2024 — the first agents.** SWE-agent, **18** on Lite: the observe–think–act loop,
  with a purpose-built interface. AutoCodeRover, **19**: adds structure-aware search — planning
  as localisation, as advertised.
- **Mid 2024 — Agentless, 32 on Lite.** The strongest 2024 point — and it is *not an agent*:
  a fixed localise → repair → validate pipeline, no loop. It hard-codes the narrowing. This is
  the planning slide, cashing out.
- **Late 2024 — OpenHands, 26 on Lite.** The general code-as-action interface. *(Lower than
  Agentless and later — say it without embarrassment; different backbones, and this is exactly
  why the footnote exists.)*
- **2025 — OpenHands plus a trained critic: 60.6 → 66.4 on Verified.** Refinement's
  sample-and-rerank, with its number — the loop, paying.
- **2026 — leaders above 70 on Verified.**
- **The hollow point — slow down, this is the honest ending.** Same era, measured on **Pro** —
  held-out repos, harder tasks, one uniform harness on Scale's own leaderboard: **roughly
  55–60 %.** *Measured*, not self-reported — vendor claims run 70–80 on Pro, but they mix
  harnesses and nobody verifies them, so this talk doesn't quote them.
- **The footnote is load-bearing — read it aloud:** splits are not comparable — **read the
  climb, not the deltas.** Then the takeaway, which is yours alone now: *the held-out, harder
  split says the climb is not over.*

*If pushed on Pro in Q&A:* OpenAI's July-2026 audit flagged ~30 % of Pro's public tasks as
broken and retracted its recommendation — it is on the backup benchmark slide. Concede it
freely; it is why every Pro number here says "measured" and carries a caveat.

## 9 → 10

*"That's the climb. Now let me split it by how hard the tasks actually are — because the
average hides the only part that matters."*

## 10 — What the climb hides

**The insight slide. This is what the whole talk has been setting up — give it its time.**
Same benchmark as the last slide (Verified), nine leaderboard systems over one year, but now
split by the difficulty the human annotators assigned: how long would a developer need —
under 15 minutes, 15 minutes to an hour, over an hour.

**Walk it top to bottom, with your hand:**

- **The black line is what the leaderboard shows you** — the overall score, climbing steadily.
  Every headline number you have ever seen about these agents is a point on a line like this.
- **The green line — easy tasks, under 15 minutes.** From 27 to 80 %: a factor of 2.9. It is
  near its ceiling now.
- **The blue line — medium.** From 10 to 62 %: a factor of **6.2**. This is where almost all
  recent movement in the overall score comes from.
- **The red line — hard tasks, over an hour. Slow down here.** It climbed from zero to about
  20 % by the end of 2024 — **and then it stopped.** The last three systems: 24.4, 24.4, 20.0.
- **The dashed line — the union.** Pool *all nine systems*: some agent solves 95 % of easy and
  84 % of medium instances — but only **42 % of hard ones**, and just 9 of the 25 hard
  multi-file issues. More than half the hard tier defeats *every* system at once.
- **The takeaway — read it off the slide, then be quiet for a beat:** *a system can gain ten
  points — and solve no additional hard task.* The headline number is a poor proxy for
  progress on exactly the tasks that matter most.

**Caveats you own (say if asked, don't volunteer):**

- The **July '24 dip** in every curve is SWE-agent resubmitted with a weaker backbone — the
  curves connect submissions in date order; they are *not* a running frontier.
- The hard tier is **n = 45**, so each hard instance is worth ~2.2 points — the stall is
  three systems in a row, not one noisy point, but don't over-read decimals.
- Per-instance results **stopped being published mid-2025**, so this series cannot be
  extended to today — which is itself a finding about the field's reporting.
- Easy + medium are 91 % of Verified — the composition is why the headline tracks them.

## 10 → 11

*"So — what to take away."*

## 11 — Conclusion

**Three lines on the slide, one argument, under a minute. Then stop.**

- **Beat 1 — the jump.** In three years: under 2 % to over 70 %. It is on the slide in large
  type; read it.
- **Beat 2 — what changed.** Not just bigger models: **the loop.** The system plans, edits,
  runs its own code, reads the result, and tries again. *(This is the definitional slide's
  sentence, coming home.)*
- **Beat 3 — where the climb stopped.** It was mostly the easy strata: hard tasks stalled
  near 20 % and have not moved. *(Spoken aside, if it fits: and on the harder, held-out Pro
  split, even measured leaders sit near 60 %.)* **The climb is not over.**

---

## Q&A pocket

The six backup slides mirror the talk's order and are titled by the question they answer:
*the numbers behind the stage walk* → *how are plans shaped?* → *how do agents refine?* →
*does the scaffold matter?* → *is the 70 % real?* → *what does running an agent cost?*

- **"Is the 70 % real?"** — Partly. Verified is the most curated split; the same weights
  measured on Pro land ≈20–35 points lower (the *"is the 70 % real?"* backup — say its three
  caveats: read only the floor, Pro is harder as well as fresher, the column mixes harnesses).
- **"Can we trust Pro then?"** — OpenAI's July-2026 audit flagged ~30 % of its public tasks and
  retracted its February recommendation of it (same backup slide). Benchmarking moves faster
  than auditing; quote measured numbers, attach caveats.
- **"Why no multi-agent systems?"** — Scope: single-agent, repository-level. Role-play
  frameworks (ChatDev, MetaGPT) are out; multi-agent *search* (SWE-Search) is treated as a
  planning variant — the *"how are plans shaped?"* backup.
- **"Why did the hard tier stall — can't the models do it, or can't they find it?"** — Open,
  and the report says so (§6.4): a capability ceiling and a shared retrieval blind spot look
  identical from the union. The cheapest experiment to separate them — hand the agent the
  gold file set so localisation is oracular — has, as far as the report is aware, never been
  run.
- **"Does the scaffold matter?"** — The backup of that name: same model, same benchmark, the
  scaffold alone is worth 11–17 points — and no scaffold wins twice; the best one depends on
  the model. (Budgets not normalised across scaffolds — it isolates the scaffold, not the
  cost.)
- **"Why is Agentless ahead of the agents in your chart?"** — Different backbones on that
  split; but its authors also ran the agentic baselines on the same GPT-4o backbone and the gap
  held. The honest claim: hard-coded narrowing beats searched-for narrowing on this
  distribution.
- **"Do the agents refine or just retry?"** — The *"how do agents refine?"* backup: every
  surveyed mechanism, what closes its loop, and its characteristic failure — with the
  oracle / proxy / self legend printed under the table, so you can read the colours cold.
- **Cost questions** — the *"what does running an agent cost?"* backup: quadratic token growth
  in turns, resolve rate decoupling from compute, and the METR RCT (developers 19 % slower
  while believing 20 % faster).
- **Magnitude questions on the stage walk** (SBFL +3, D4C +10 %, self-debug +12 %, critic
  60.6→66.4) — the first backup slide, *the numbers behind the stage walk*.
