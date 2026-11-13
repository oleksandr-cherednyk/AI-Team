# <PROJECT NAME> — What the Team Must Know About This Project

> **Template.** Filled out by the Team Lead during project deployment (procedure P-0) and lives **in the project repository**, not in the governance repository. An empty section is better than an invented one: an empty section says "not yet determined," whereas an invented one lies.
>
> **This is the single place where the governance framework touches a specific project.** Everything else is a reusable frame. Any role that encounters the term "gate" or "content rule" in a governance document looks it up **here**.

**Filled on:** `<date>` · **updated by:** `<role>` · **verified current on:** `<date>`

---

## 1. What This Product Is, in Four Lines

* **Application type and architectural profile** (check one or more — this determines which sections of `ARCHITECT.md`, `SECURITY.md`, `DESIGNER.md`, and `DEVICE.md` apply):
  - `[ ]` **Prof-1: Local / Offline-First Application** (no backend or runtime network calls)
  - `[ ]` **Prof-2: Web / Fullstack with Backend, API, and DB** (`client ↔ server ↔ database`)
  - `[ ]` **Prof-3: Mobile Application** (Android / iOS / emulator)
  - `[ ]` **Prof-4: Desktop Game / 3D Application** (Godot 4 Mono C# / Three.js / 3D assets)
* **What it does and for whom:**
* **How its quality is measured** (not "works well," but what specifically is verified):
* **Decisions that cannot be revised without the owner** — product invariants:

> Example from the project where this framework originated: "the application is completely local — no accounts, no backend, no analytics; this is a product decision, not a temporary state." A role may **show by measurement that such an invariant diverges from reality**, but may never overrule it.

---

## 2. Repository Layout

| Directory | Contents | Default Territory Owner |
|---|---|---|
| | | |

**Rule:** territories **must not overlap**. Two sessions editing the same file cost far more than handing off a finding.

---

## 3. Verification: How We Prove Nothing Is Broken

| Command | What It Proves | Part of Default Chain? | Runs in CI? |
|---|---|---|---|
| | | | |

**Three rows must be filled out explicitly:**

* **full suite run** — the exact command, how long it takes, and **how to tell it actually ran to completion** (we had a case where the run stopped at the first failing package and never executed subsequent packages, yet the log did not look like an early abort);
* **what is NOT included in the full run** — gates that must be invoked manually. This is the primary place where unnoticed regressions accumulate;
* **what static types check that unit tests do not, and vice versa.**

---

## 4. Content and Quality Rules

If the project produces content (texts, exercises, user-facing data) — **the rules live here** and **supersede any task description and any Team Lead decision**.

| Rule | What It Forbids or Requires | How It Is Verified |
|---|---|---|
| | | |

> In the ancestor project there were eight such rules, and two are worth citing as structural examples:
> **"Quality over quantity"** — a numeric target met with filler is a failed target; if it cannot be met with high quality, **the target is lowered**. And **"A locale is a language, not a translation"** — if no high-quality translation exists, the item **is not shown** in that locale, and leaving an empty string is not a valid way to satisfy the rule.

---

## 5. Shared Artifacts and Generators

**What is rebuilt by a script and therefore never edited by hand**, and in what order.

| Generator | What It Builds | From What Source | Who Runs It |
|---|---|---|---|
| | | | Team Lead |

**The order is mandatory, not a convenience:** an artifact is rebuilt **after** the source material from which it derives is committed to the working tree. Otherwise it silently rebuilds from stale input.

**Symptom to document here:** which storefront or summary view **diverges from its source** if the generator is not run. In our ancestor project, this cost half a shift twice: a card promised one number while the actual content delivered another, and twice it was mistaken for an application bug.

---

## 6. Baselines and Ratchets

| File | What It Guards | Direction It May Move Freely |
|---|---|---|
| | | downward |

**Rule:** downward — freely; upward — **only with a recorded explanation of which underlying data changed**. A baseline raised without an explanation is read a month later as the intended norm.

---

## 7. Testbench and Devices

* **how the live product is verified** (emulator, physical device, browser, desktop window) and how to spin it up;
* **the irreversible scenario** — the one that cannot be casually redone (for us, installing a new build on top of an older build without clearing user data);
* **environment traps** — pitfalls that have already cost time. Record them immediately, not "once enough pile up."

---

## 8. Branches and Working Trees

* **primary branch for the cycle:**
* **how parallel sessions are isolated** (separate git worktrees / branches / both):
* **who merges:** Team Lead;
* **commit rule:** explicit file paths only (`git commit -- <paths>`, never `git add -A`).

---

## 9. Questions Only the Owner Can Answer

Three classes that never change across projects:

1. **what the product promises the user** — scope, volume, interface appearance, grading/scoring model;
2. **decisions already made by the owner** — divergence from reality is demonstrated by measurement, never overruled by vote;
3. **irreversible actions** — deleting content, renaming storage keys, anything that cannot be reverted with a one-line fix.

**Project-specific owner questions go here:**

---

## 10. What Is Already Known About This Project and Cost Us Time

This section grows as we work. Record only what **will recur** and **cost real time** — not generic "good to know" trivia. Universal patterns across projects live in [`PLAYBOOK.md`](../PLAYBOOK.md); this section holds only what is specific **to this project**.
