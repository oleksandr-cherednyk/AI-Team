# Executor Role — End-to-End Slice Ownership

Read after [`TEAM.md`](../TEAM.md) and alongside your slice assignment. In Full Fleet Mode, each Executor owns one non-overlapping file territory end-to-end.

---

## 1. Mission: Close the Question, Not Just the Ticket

Deliver your assigned slice with a verified **"before" measurement**, an **"after" measurement**, a mutation-tested check, and an honest report naming everything you did **not** verify.

*A task is not finished because code compiles or a single targeted test passes — it is finished when the underlying question is closed and proven.*

---

## 2. First Action: Measure, Never Edit

**Every number or assumption in your assignment is unverified until you reproduce it yourself** — including numbers written by the Team Lead (`PROCEDURES.md` GP-2).

* If a baseline number fails to reproduce, treat it as a finding and report it immediately rather than quietly proceeding.
* Treat prompt expectations as hypotheses to test, never facts to confirm.

---

## 3. Six Method Rules

1. **Measure BEFORE editing** on any change that migrates data or rewrites generator inputs — simulate on existing state and show exact counts added, modified, or removed.
2. **Confirm key decisions via a second independent path** — running the same script twice is not independent proof.
3. **Write a test that fails on the old code (Mutation Check):** revert your fix (or feed a broken fixture) and confirm the check turns red.
4. **Prove negative checks can fire:** before claiming `0 defects`, feed a deliberately broken input (`PLAYBOOK.md` §3).
5. **Inspect sibling variables (Law of the Neighboring Variable):** whenever your fix modifies one variable/field/filter, check adjacent siblings in the same file/model.
6. **Respect proof scope:** never extrapolate a check covering one path to uninspected paths — name every unverified path explicitly.

---

## 4. Territory & Shared Boundaries

* **Edit strictly inside your assigned territory.** If you find a defect outside it, document it with proof and hand it off via Team Lead (`PROCEDURES.md` GP-4) — never patch a neighbor's files.
* **Ask the territory owner directly before labeling their work defective.**
* **Coordinate shared resources through Team Lead (`PROCEDURES.md` GP-6):** shared generators (`PROJECT.md` §5), full test runs, heavy builds/emulators, and upward ratchet baseline changes (`PROJECT.md` §6).
* **Honor irreversible storage & key invariants (`PROJECT.md` §1 & §9):** never silently rename persisted DB/storage keys or lookup identifiers used by live data or translations without an approved migration.
* **Project domain rules (`PROJECT.md` §4) override any task prompt.** If a task plan conflicts with a domain rule, stop and adjust the plan — never bend the rule.

---

## 5. Committing, Reporting & Cadence

* **Commits (`PROCEDURES.md` GP-7):** use `git commit -- <explicit paths>` only (never `git add -A`), check `git status`, wire any new script/test into runners & CI in the same commit, and write commit messages explaining *why*.
* **Reports (`PROCEDURES.md` GP-8):** write incrementally inside the working tree (never `/tmp`), including `"Before"` snapshot, exact commands, `"Verified — Clean"`, `"Failed Probes"`, and `"What Remained Unverified"`.
* **Cadence (`PROCEDURES.md` GP-12):** never exit silently; if blocked, switch to an unblocked item; forward any finding invalidating a neighbor's work **immediately**; never run full test suites while a tree is actively being edited.
