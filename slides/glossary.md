# Glossary

## Oracle

An **authoritative oracle** is a source of ground truth that sits *outside* the model
entirely — a test runner, a compiler, a human reviewer. When it gives a verdict (e.g. a test
fails), that verdict comes from actually running the code on a real machine, not from
anything the model predicted. This means it can **surprise** the model: tell it something it
did not already believe, because the judgment didn't come from the model's own distribution
at all.

## Proxy

A **learned / generated proxy** is something that was *fitted* against ground truth at some
point in the past (a trained critic/reward model, an LLM-as-judge, tests the agent generates
for itself), but is now being consulted *instead of* the ground truth itself. A proxy can
genuinely disagree with the generator — that disagreement is real and can be worth points —
but its ceiling is its own fidelity: it can never surprise the model with information beyond
what it was trained or fitted with. Its characteristic failure mode is that it tends to be
wrong in exactly the same places the generator was wrong, since that's the region its own
training couldn't cover either.

**The key distinction:** an oracle's verdict cannot have been predicted by the model in
advance (it comes from outside); a proxy's verdict, however useful, is bounded by what a
model-like process already learned — so it can shift what the generator believes, but never
introduce information the generator's "family" of models couldn't in principle have had.

## Self-critique

**Same-distribution self-critique** is the third position on the same axis: the model
judging its own output using nothing but its own weights, with no test, critic, or other
external check involved at all. Since the critic and the generator are draws from the *same*
distribution, this carries **no new information** — it can only reorganize what the model
already believes, it can never contradict it. If a model could reliably tell its own wrong
code from its own right code by inspection, it would have written the right code in the
first place.

## The full axis

Oracle, proxy, and self-critique are three points on one spectrum — "how much can this signal
surprise the model?" — not three unrelated ideas:

| | Where the signal comes from | Can it surprise the model? |
|---|---|---|
| **Oracle** | Outside any model — a real execution/measurement | Yes, unbounded |
| **Proxy** | A model-like process fitted to ground truth in the past | Only up to its own fidelity |
| **Self-critique** | The same model/distribution being judged | No — same information, repackaged |

Two other terms that show up attached to this axis:

- **Surprise** — used in the information-theoretic sense: a signal "surprises" the model if
  it tells it something it did not already believe. This is the actual test used to sort a
  mechanism into one of the three rows above.
- **Ground truth** — a synonym for what an oracle provides: a fact established independent of
  any model's belief about it (e.g., the test suite actually passing or failing).
