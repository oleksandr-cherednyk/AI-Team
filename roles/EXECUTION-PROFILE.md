# Execution Profile: Which Model and Effort Level to Use for an Assignment

**The Team Lead must specify the execution profile in every assignment; the Executor has the right to challenge it.**

**Why.** By default, the strongest model at medium effort is used, and for part of the work that is waste: executing a settled plan does not require the same capacity as drafting it. A properly matched profile saves time and cost **without sacrificing quality for that specific task**.

---

## 1. The Rule Everything Follows From

> **The profile is chosen by the KIND OF OUTPUT required, not by the volume of edits.**

Not by file count, not by line count, not by "looks simple." Five places edited identically is mechanics. Seven identifiers where you must **decide** whether they are needed is judgment.

| Assignment output | Profile |
|---|---|
| Apply a settled decision to N places · collect numbers with an existing command · localize a string | **light** |
| **Decide whether X is true** · **figure out why** · plan · architecture · resolving contradictions | **strong** |
| Review | **by subject:** checking against a fixed list — light; "what is wrong here" — strong |

---

## 2. The Caveat Without Which the Rule Causes Harm

**Work that looks mechanical regularly turns out to require judgment.** While "wiring up an existing field," you discover the field is attached to the wrong object; while "removing unused code," that it represents a recorded architectural decision; while "counting numbers," that the numbers expose a contradiction in the rules.

**Two mandatory corollaries:**

1. **You may lower the profile only where the decision has already been made.** "Investigate and fix" is never light, even if the resulting edit is a single line;
2. **Escalating the profile by the Executor is free and expected.** Saying "this requires more than assigned" is **work, not failure**. Conversely, **silently executing a judgment task on a light profile is more dangerous than slow work**: it produces a confident wrong answer that nobody will catch.

**Sign of an under-provisioned profile:** the Executor replies quickly and **brings back zero caveats**.

---

## 3. What the Team Lead Writes in the Assignment

```
Profile: light · mechanical application, decision is settled, no deviation needed
Profile: strong · requires a verdict, not an edit
Profile: strong, high effort · planning and resolving contradictions
```

**And the explicit right to challenge, verbatim:** "if it turns out to require judgment — **stop and say so**, do not push through on a light profile."

**Profiles are revisable.** If it turns out the profile was chosen wrongly, change it — that is normal course of work. The mistake is leaving an under-provisioned profile in place **after learning** it is too light.

---

## 4. The Profile Is a Property of the SESSION, Not the Task

**Not because a model cannot be switched mid-session — it can.** Rather, for two reasons stronger than technical ones:

1. **Switching breaks the provenance of conclusions.** Our method rests on the question "who measured this and with what." If the model changed mid-flight, you **cannot tell from the report which reasoning produced which conclusion**. **A number can be re-measured; a judgment cannot be re-measured** — it can only be redone from scratch;
2. **Switching downward gives the worst of both worlds:** the light model inherits **the entire accumulated context** and pays for token volume right where it is weakest.

**Only one direction of mismatch is dangerous.** A strong session doing mechanical work is overpayment, nothing more. **A light session doing judgment work produces a confident wrong answer**, costing far more than everything saved.

### Hybrid Execution — Yes, Through One Specific Shape

**Not "switch the session's model," but "a strong session delegates bounded mechanics to a light subagent."**

| | Switching session model | Subagent delegation |
|---|---|---|
| Who is accountable for the result | **Unclear** | **The strong session, in full** |
| Task boundary | Blurred | **Declared explicitly** |
| Provenance of conclusions | **Lost** | Preserved |
| Context | Inherited in full | **Only what is needed is passed** |

**Conditions for delegation:** name and justify the boundary · escalate any ambiguous boundary to the Team Lead · **accountability for the result remains with the delegating session**.

---

## 5. Small Off-Plan Tasks — A Dedicated Queue

Such work always exists, and it never fits neatly into a major plan: captions, version bumps, wiring up a ready field, syncing a document with a live test run, removing items once a verdict is reached.

**Sign that this queue is missing: the Team Lead does small chores personally** because no active session fits them.

**The queue is a tag on a board card**, not a fifth list of open work:

```
Status: P3 · open · profile: LIGHT — decision settled, only application remains
```

**An untagged card is treated as requiring judgment** — defaulting to the safe side.
**The Team Lead batches tagged cards into a run** and requests a light-profile session for them.

---

## 6. What This Document Does NOT Promise

* **It does not replace the Team Lead's judgment** — the table in §1 covers common cases, not every case;
* **It does not cut corners on acceptance.** A light profile makes execution cheaper, but **mutation testing, "before" measurements, and the "what remained unverified" section are mandatory under every profile**;
* **It is not about "Junior is worse."** Mechanical work done accurately and completely is a first-class deliverable, and its metric is the same: **zero missed places**, not the raw number of edits.
