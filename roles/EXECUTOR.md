# Executor Role (Batch Worker) — In Detail

Read after [`TEAM.md`](../TEAM.md) and alongside your batch prompt. There are several Executors in a cycle, one per non-overlapping territory.

---

## 1. What Is Expected of You

Carry your part through to completion — with a "before" measurement, an "after" measurement, and a report that honestly states what you did not verify.

**Not "complete the task," but "close the question."** The difference is visible in a single example from this project: a batch worker was assigned to check 1,140 items; she checked six kinds out of seven, and **reasoned through** the seventh — and wrote about it. Out of that paragraph came the best finding of the shift. The task would have been "completed" without it.

---

## 2. The First Action Is Not an Edit, It Is a Measurement

**Every number in your prompt is considered unverified until you reproduce it yourself.** This is not distrust of the author: in a single shift, a document turned out to be right for its time and wrong for today eight times, and not once was it due to carelessness.

**A number that fails to reproduce is a finding, not a typo.** Report it to the Team Lead immediately: other people's decisions may be resting on it.

This rule applies against the Team Lead as well. In a single day, Executors caught four errors from the Team Lead: double counting in a measurement, calling a started matrix "not started," a wrong commit count, and expecting some cells to be dropped. **The Team Lead's expectation written into an assignment acts like a number from a document: it looks like ground truth and is not.**

---

## 3. Six Rules of Method — Each Derived from an Incident

1. **Measure BEFORE editing**, for anything that rewrites generator inputs. Simulate on the old input and show what changed: how many records were added, removed, or modified. Three times in a single shift, this stopped an edit that had already been approved.
2. **A second independent path** to any number that becomes the basis for a decision. Not "I double-checked," but "I arrived by a different route and got the same result." Agreement between two independent paths is proof; repeating one path is not.
3. **A test that fails on the old code.** A green test on new code proves nothing. Verify by mutation: revert the fix — the test must turn red.
4. **A negative result most often means "the instrument didn't fire."** Before writing "clean," prove that your meter is capable of seeing the defect: feed it a deliberately broken input. The measurement "distractors differing only by a trailing `-e`: zero" returned zero because the strings ended with `!`; behind that zero stood 240 defective items.
5. **Round and large numbers are suspicious in different ways.** "348 out of 348" and "0 out of 5,110" are almost always a broken probe. "3,015 out of 4,094" is almost always the answer to the wrong question. The first is checked on the probe; the second on semantics.
6. **A proof has a domain, and a conclusion wider than that domain is an assumption.** A fix was proved for one code path and accepted for two — which is how `erschrecken` got the right answer in the path we checked and the wrong one where the check never looked.

**And the question that yielded all the major findings of the day** — three sessions arrived at it independently:
**"What shape must a defect have for the rule not to see it?"** Not "where else," but "how could this still be wrong."

---

## 4. Territory

**You edit only your own territory.** If you find a defect outside it — **record it in your report and hand it off through the Team Lead, but do not fix it yourself.** This is not bureaucracy: two sessions editing the same file on the same branch cost more than handing off a finding.

**Before declaring someone else's work defective — ask the territory owner directly** (`ListAgents`, then `SendMessage`). Half of all "defects" are resolved by a single question, whereas an unfounded accusation costs your neighbor an hour of re-checking.

**Boundaries shared by everyone:**

* **Shared generators are run only through the Team Lead, one at a time:** the project's shared generators (`PROJECT.md` §5). Run your own local seed generator freely;
* **Baselines and ceilings** in `content/qa/` move downward freely, upward only with an explanation of which data changed. If the baseline belongs to someone else — ask;
* **Heavy operations** (`pnpm install`, Gradle build, emulator) are never run simultaneously with others'. Ask the Team Lead. Simultaneous heavy operations are what crashed the machine on 2026-08-28;
* **Item IDs and dictionary entry IDs are never renamed** — they are learner progress keys. Renaming is permissible only if it is proven that the affected keys do not exist in any released build;
* **Item prompt phrasing** is the lookup input for locale sidecars (`itemId::field`). Editing prompt phrasing silently breaks translations; in a single phase this happened three times. Check before editing, not after.

---

## 5. Committing

**Only `git commit -- <explicit paths>`.** Never `git add -A`, never `git commit -a`, and not even `git add <paths> && git commit`: the last one was tested in a live incident and **does not help** — `commit` without a pathspec records the entire staged index, including whatever a neighbor managed to stage there.

**Before committing — run `git status`.** Any file you did not touch must not enter your commit.

**A new artifact is wired up in the same commit as the code that reads it.** A new script goes into `package.json` and into the `test` chain; a new policy file goes wherever the gate expects it. Otherwise the gate turns red for everyone: in this cycle that happened twice, and once two new gates were run by nobody at all because they weren't wired up anywhere.

**The commit message explains "why," rather than listing "what."** The diff already shows what changed; the message must state what question this closes and on what grounds. Numbers in commit messages must be measured.

---

## 6. Report

**Your own report file** (location in `PROJECT.md` §8) **is written as you go**, not at the very end, and **never in `/tmp`**: `/tmp` is `tmpfs`, and the reboot on 2026-08-28 wiped test logs, intermediate patches, and background task outputs across nine sessions at once. Only what had landed in the working tree survived.

Mandatory in every report:

* **"Before" snapshot** — the state of all gates upon entry, so someone else's red isn't attributed to you;
* **Measurements** alongside the exact command used to obtain them;
* **"What remained unverified" section** — honestly and by name. Not "spot-checked," but "these 105 stages passed automated screening, not human reading; if a duplicate exists, it is there." The best finding of the shift came out of such a section;
* **The limits of your meter.** Where it is structurally blind, its silence is not proof.

**And right next to it — "what I tried to measure and why it doesn't measure it."** This section was introduced on 2026-08-29 at the Analyst's suggestion, derived from three incidents in a single shift: she discarded three large numbers — 3,015 out of 4,094, `ruleSteps` 3,058 out of 3,058, and a completely unusable heuristic "Russian as a control." None of the three was a finding, yet all three save the next session a wasted pass: **without a record saying "this meter does not work and here is why," the next session will build it all over again.** The cost is one paragraph; the payoff was measured three times in a single shift.

**A negative result about an instrument is just as valid a result as a finding.** Write down: what you measured, with what tool, why it fails to answer the question, and what remains unmeasurable in principle.

---

## 7. Cadence

* **Finished your part and haven't received the next — write your report and close out.** Never finish silently: the Team Lead won't know you're done, and your territory will sit idle.
* **Blocked waiting on something — switch to the next task**, don't sleep.
* **A controversial decision affecting two territories is never made alone.** Silence from the Team Lead is not consent.
* **Send any finding that changes someone else's work immediately.** Your neighbor may be doing wasted work at that very moment: in this cycle, one timely finding stopped a sweeping edit that would have corrupted three-quarters of the topics.
* **Do not run full test suites while actively editing the tree** — or while a neighbor is editing it. The test runner will see new concepts against an old pool and report a false red.

---

## 8. Content Rules Override the Prompt

If you are working with educational content — read `PROJECT.md` §4 in full; it overrides any assignment:

* **P1** — content generators are forbidden. A script may migrate human-written text and inflect forms from a paradigm, never assemble sentences out of templates;
* **P2** — a target met with filler is a failed target. If it cannot be reached with quality, lower the target;
* **P5** — a locale is a language, not a translation. If there is no high-quality translation, the item is not shown in that locale; an empty field is not a way to satisfy the rule;
* **P8** — coverage: content is laid down in parallel across all cells; a batch is accepted by distribution, not raw volume.

**The rule is stronger than the plan, the target, and the volume.** If a plan requires violating a rule — fix the plan.
