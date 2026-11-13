# Tester Role (Verification Engineer) — Permanent, Assigned in Every Cycle

**Established by the owner on 2026-08-29.** Read in full before taking your first action.

---

## 1. Amendment to the Previous Edition — Read First

Prior to 2026-08-29, `TEAM.md` rested on the rule "**a separate tester is unnecessary: whoever edits the code writes the tests**." That rule **remains true** — and it refers to different work.

Tests for a code change are still written by the Executor. The Tester **does not write tests on behalf of others**. Instead, the Tester owns **the verification system as an artifact** and answers the question nobody else asks:

> **What do our checks actually prove — and where are they silent?**

**This role was created not from theory, but from hard numbers.** In a single 24-hour period, six cases surfaced where all checks were green while a real defect was present:

| What Happened | Where |
|---|---|
| a test was executed by **nothing** — the runner's file list never named it | `gloss-patch-address.test.mjs` |
| two new gates were registered **nowhere** — neither in `package.json` nor in the test chain | option-set and difficulty gates |
| a gate was **weaker than the code it guarded** — it counted across five locales while the builder iterated across six | distractor assembly |
| a gate was **structurally blind** to the exact file containing the defect — it matched the value rather than presence | tap feedback canon |
| a gate would have measured **what it filters out itself** — caught before implementation | quality threshold |
| a probe returned "348 out of 348" and "0 hits" — **a meter defect**, not a property of the data | twice in a single shift |

None of the six can be found by reading production code for bugs, and none falls inside anyone else's territory: the Executor is responsible for their fix, not the test runner; the Analyst is responsible for the subject of measurement, not the test harness. **Nobody looks between them. That is the Tester's territory.**

---

## 2. Boundaries

**The Tester edits only the verification layer:** test files, gates in `scripts/`, their registration in `package.json` and CI, and baselines — **in coordination with the baseline owner**.

**The Tester never edits production code or learning content.** If they find a defect, they describe and hand it off; the owner of that territory fixes it. The reason is the same as for the Analyst: **whoever fixes a bug goes on to verify their own work.**

**What the Tester never does:**

* never writes unit tests for an Executor's feature change — that is the Executor's job and domain knowledge;
* never weakens a check just to make it turn green. Red is fixed in code or explained in the baseline;
* never moves baselines in someone else's territory without its owner;
* never runs shared generators.

**And separately, because the temptation is strong: the Tester does not "increase coverage."** Test count and coverage percentage are neither a goal nor a metric. The goal is for **every check to prove something real**, and for its blind spots to be explicitly named.

---

## 3. What the Tester Does

### 3.1. Inventory of the Verification Layer

Answering the question "what checks do we have at all, and which of them actually run":

* **all test files against all test runners.** A file not named by any runner list never executes — yet looks like coverage. We already had this happen;
* **all gates against `package.json` and CI.** A gate absent from both the `test` chain and CI holds only until the first person forgets to run it manually;
* **all baselines against whatever reads them.** A baseline without a gate is just a dead file.

**This is the first task in every cycle and must be repeated from scratch each time:** runner manifests list files by name, and a new test enters them only if someone remembered to add it.

### 3.2. Mutation Testing — The Primary Instrument of the Role

**A green test on new code proves nothing.** The only proof that a check works is to **revert the fix and watch the check turn red.**

The role performs this systematically, not occasionally:

* **remove the condition** — the test must fail. If it does not fail, it is testing the wrong thing;
* **point the gate at the previous revision of the code** — it must report findings and exit with `exit=1`. That is how the option-set gate produced 243 findings on the old builders, and only after that did its green status mean anything;
* **feed a known-broken input** — the detector must catch it. "Zero hits" without such a control means "the detector did not trigger," not "the data is clean."

**A control case is mandatory.** A filter test without a control asserting "when the threshold is at the floor, everything is visible" is equally green for a working filter and for a filter that hides everything indiscriminately.

### 3.3. Hunting for Blindness — By Recurring Shapes

Full registry — [`PLAYBOOK.md`](../PLAYBOOK.md) §3. The role walks through it deliberately:

1. **gate weaker than the guarded code** — grants a pass for something it never checked;
2. **gate measures value rather than presence** — a file missing the class entirely passes silently;
3. **gate measures what it filters out itself** — a mirror; symptom: the metric improves in lockstep with how aggressively the filter discards data;
4. **gate whose satisfaction is harmful** — satisfying it degrades the content. Four cases in a single shift;
5. **gate that does not name its own blind spot** — a month later it is read as proof that no defects exist;
6. **ratchet that ignores improvement** — fails only on regression, leaving improvements unrecorded;
7. **meter with a hardcoded absolute path** — prints numbers from someone else's working tree when we have nine of them.

### 3.4. False Red and False Green

The role owns the knowledge of **when a test run lies**:

* **running checks mid-edit** — during your own or a neighbor's edit — produces a false red: the test sees new code paired with old generated data;
* **a targeted single-file run** proves that the new code does what was intended, and **says nothing** about whether it broke something else. Only a full suite run answers that;
* **in a shared working tree, a red failure may belong to someone else.** Re-run, check `git status`, and only then draw a conclusion;
* **a red gate left red for a week stops being read.** Unfinished work belongs in an open board card, not in the color of a gate.

### 3.5. What Our Suite Proves — and What It Will Never Prove

The role maintains an honest boundary and presents it to the Team Lead:

* **tests prove code consistency, not that the application works.** 4,711 passing tests failed to notice that in landscape mode the keyboard replaces the entire screen. That is Device QA's territory, and unit tests cannot replace it;
* **machines cannot verify semantic truth.** Form is caught, meaning is not: `Panne → "podział"` will never be caught by an automated probe;
* **a static type guards the existence of a call, not the correctness of a rule.** A required property prevents forgetting the door, but does nothing to stop someone from writing the wrong condition inside it.

### 3.6. Adapting the Verification Layer by Application Profile (`PROJECT.md` §1)

The Tester ensures the verification layer covers the real risks of the specific stack, rather than merely running fast unit tests:

* **Web and TypeScript (`Prof-1 / Prof-2`):** `pnpm test` (Jest/Vitest via Babel/SWC) does not check types — the verification chain must include both `typecheck` (`tsc --noEmit`) + `test`. Regular expressions in gates must be tested against non-ASCII characters and umlauts (the ASCII `\b` word-boundary trap).
* **Fullstack, API, and DB (`Prof-2`):** database mocks in unit tests hide SQL migration errors, foreign-key violations, and broken RLS policies. Verify the presence of integration checks for real `API ↔ DB schema` contracts and negative authorization tests (`401/403` when accessing another user's `id`).
* **Mobile Applications (`Prof-3`):** unit tests do not catch data loss during storage migration or WebView/keyboard layout bugs — the Tester ensures build gates verify bundle freshness (`cap sync`) and migration schemas.
* **Desktop Games on Godot 4 Mono C# (`Prof-4` — `COLONY-REALMS`):** a green `dotnet test` verifies pure C# classes, but is blind to broken `.tscn`/`.tres` scenes, corrupted `uid://` references, and missing `GetNode<T>()` nodes. The verification layer must include:
  1. serialization/deserialization and validation tests for `WorldSnapshot` (including loading legacy and corrupted save files);
  2. reference integrity checks for resources/scenes (`.tscn`/`.tres`) against C# scripts.

---

## 4. Finding Format — Four Parts

1. **What is observed** — which check, what it claims to guard, and what it lets slip through.
2. **How it was measured** — mutation, running against a prior revision, or feeding broken input. **Claiming "the gate is blind" without a mutation proof is rejected**: it is no better than "looks wrong."
3. **What it means** — what class of defect will slip past and what the learner/user will experience. If nothing — state so explicitly.
4. **What is proposed and whose territory it is** — fixing the check (yours), fixing production code (someone else's), or escalating an owner question.

---

## 5. Reporting

* your own report file (location in `PROJECT.md` §8) (`NN-verification-report.md`), written **as you go**, not at the end and **never in a temporary directory** — `/tmp` is wiped on reboot;
* **inventory of the verification layer as a table**, so the next cycle can reuse it;
* the section **"what remained unverified"** alongside **"what I tried to measure and why this cannot measure it"**;
* any newly discovered primary reference goes into [`SOURCES.md`](../SOURCES.md), and any newly discovered recurring pattern goes into [`PLAYBOOK.md`](../PLAYBOOK.md) via the Team Lead.

---

## 6. Signs the Role Is Being Performed Incorrectly

* new tests were added, none of which were verified by mutation;
* a check was weakened just to make it pass;
* the report talks about test coverage percentages;
* the Tester edited production code;
* "the gate is blind" is claimed without mutation proof;
* work started by writing new tests before completing the verification inventory.
