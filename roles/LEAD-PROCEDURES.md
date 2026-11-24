# Team Lead Procedures — Step-by-Step Operational Runbook

Companion to [`LEAD.md`](./LEAD.md): `LEAD.md` defines **what and why**; this runbook defines **how, step by step** across every operational situation.

**Table of Contents**

| ID | Situation |
| --- | --- |
| **P-0** | [Bootstrapping a New Project](#p-0-bootstrapping-a-new-project) |
| **P-1** | [Start of Shift — Execute First, Read Second](#p-1-start-of-shift--execute-first-read-second) |
| **P-2** | [Calculating Sessions & Dispatching Prompts](#p-2-calculating-sessions--dispatching-prompts) |
| **P-3** | [Processing a Participant's Report](#p-3-processing-a-participants-report) *(incl. P-3-bis Liveness)* |
| **P-4** | [Decision: Autonomous vs. Owner Escalation](#p-4-decision-autonomous-vs-owner-escalation) |
| **P-5** | [Merging Branches & Shared Generators](#p-5-merging-branches--shared-generators) |
| **P-6** | [Handling a Red Gate](#p-6-handling-a-red-gate) |
| **P-7** | [Introducing or Retiring a Role](#p-7-introducing-or-retiring-a-role) |
| **P-8** | [Updating Team Documentation](#p-8-updating-team-documentation) |
| **P-9** | [Retracting a Finding](#p-9-retracting-a-finding) |
| **P-10** | [Closing or Reassigning an Idling Session](#p-10-closing-or-reassigning-an-idling-session) |
| **P-11** | [Writing Consolidated Questions for the Owner](#p-11-writing-consolidated-questions-for-the-owner) |
| **P-12** | [End of Shift](#p-12-end-of-shift) *(incl. P-12-bis Documentation Housekeeping)* |
| **P-13** | [Closing a Cycle](#p-13-closing-a-cycle) |
| **P-14** | [Recovery After an Interruption](#p-14-recovery-after-an-interruption) |
| **P-15** | [Continuous Learning as Team Lead](#p-15-continuous-learning-as-team-lead) |
| **P-16** | [Rescuing Orphaned Work in a Shared Tree](#p-16-rescuing-orphaned-work-in-a-shared-tree) |
| **FM** | [Seven Failure Modes Diagnostic Checklist](#seven-failure-modes-of-a-team-lead) |

---

## P-0. Bootstrapping a New Project

Triggered once when there is no `PROJECT.md` in the target project.

1. **Choose the right mode first (`README.md`):** if the repo is greenfield/small without CI and tests yet, recommend **Lite Mode** (`templates/AGENTS.md` + `templates/PROJECT.md`) instead of full multi-session orchestration.
2. **Deploy templates into the target project** (`templates/README.md`):
   - `templates/AGENTS.md` → `<project>/AGENTS.md`
   - `templates/PROJECT.md` → `<project>/PROJECT.md`
   - *(Full Fleet Mode only)* `templates/board/*` → `<project>/board/` and `templates/memory/_role/*` → `<project>/memory/<role>/` (only for active roles).
3. **Populate `PROJECT.md` from live facts, not intentions:** run existing checks, inspect repo layout, identify active profile(s) `Prof-1..4`. Leave unknowns empty rather than fabricating.
4. **Guard repository boundaries:** ensure project files (`board/`, `memory/`, `PROJECT.md`, `AGENTS.md`, `STATE.md`, reports, screenshots) stay ignored in `ai-team/.gitignore`. If you add any new local project directory next to the framework, add it to `.gitignore` and **commit that line immediately** so other worktrees do not commit project files.
5. **Confirm the live product works** before assigning feature work.
6. **Initialize `board/00-START.md`** (`✅ NO OPEN WORK` if clean) and report recommended session count to the Owner (`P-2`).

*Do not modify shared framework documents to hardcode project specifics — all project specifics live exclusively in `PROJECT.md`.*

---

## P-1. Start of Shift — Execute First, Read Second

Reading docs first primes you to repeat stale numbers as truth. **Run checks first, then orient.**

### Part A — Run Live Checks Before Reading
1. **Worktrees & branches:** inspect `git worktree list`, `git status`, and `git log` across all trees. Uncommitted edits are someone else's live work — never delete or revert them.
2. **Remote & CI parity:** compare local `HEAD` against remote (`CI green` applies only to the exact pushed commit; inspect CI steps, not just the summary badge).
3. **Execute verification suite** (`PROJECT.md` §3) yourself; confirm all expected packages/checks actually ran and verify declared checks vs. executed checks.
4. **Live runtime check:** confirm whether a valid Runtime QA verification exists for the current state.

### Part B — Targeted Reading (Not Cover-to-Cover)
Read in order: (1) `board/00-START.md` + relevant rows in `OPEN.md` → (2) **agreed cycle plan** (do not confuse board findings with the agreed plan!) → (3) `STATE.md` / previous shift handoff → (4) `PROJECT.md` → (5) recent decision log (bottom-up).

> **Board vs. Plan trap:** The board is always fresher than the plan and pulls attention toward newly found issues. Check at shift end how many closed items advanced the agreed plan vs. newly discovered findings; if findings dominate, explicitly pause new discovery and confirm priority with the Owner.

### Part C — Roster & Dispatch
Walk through every role explicitly (spinning up / not spinning up / why), give the Owner the exact session count (`P-2`), dispatch prompts, and state aloud what is **out of scope** for this shift.

*Sign the first hour failed:* you can summarize docs, but cannot cite a single live measurement obtained by your own command in this shift.

---

## P-2. Calculating Sessions & Dispatching Prompts

`Session count = non-overlapping territories`, capped by: distinct verification kind · zero file overlap · Team Lead review throughput (**max 6–8 Executors**).

* **Owner format:** `"We need N sessions in addition to this one: <role> — <count> — <territory & verification kind>."`
* **Mandatory prompt elements:** exact territory & forbidden files · onboarding reading list · *"measure before editing"* · required proof/checks · report file path · *"never exit silently"* · execution profile (`EXECUTION-PROFILE.md`).
* **Split by craft, not by importance** when two roles touch related work, keeping file ownership strictly disjoint.

---

## P-3. Processing a Participant's Report

1. Independently verify the specific number on which a decision rests.
2. Separate **measurement** from **cause explanation** (a number can be right while the explanation is wrong).
3. Ask *"is this counting the right thing?"*
4. Decide with written rationale and update `board/` immediately.
5. Forward findings that invalidate another session's work **immediately**.
6. Record any newly proven reusable technique in `PLAYBOOK.md` the same day.
7. Thank participants when they validly deviate from stale prompts due to changed facts.

### P-3-bis. Measure Session Liveness (Don't Trust Status Tables)
A status table records *intent at assignment*, not reality. Confirm: (a) explicit acknowledgment of the assignment, and (b) actual file changes / progress in the session's worktree.

---

## P-4. Decision: Autonomous vs. Owner Escalation

* **Preserves user-visible behavior** → Team Lead decides (record *"why"*).
* **Changes what the user sees/is promised**, overturns a prior Owner decision, or triggers an irreversible action → **Owner decision** (`P-11`).
* **Project domain rules (`PROJECT.md` §4)** supersede Team Lead authority.

---

## P-5. Merging Branches & Shared Generators

1. Merge with `git merge --no-ff <branch>` and a commit message stating *why*.
2. On config/manifest conflicts (e.g., two branches adding checks to `package.json`), preserve **both** additions.
3. Run shared generators (`PROJECT.md` §5) **after** all upstream source edits have landed, strictly in dependency order.
4. Re-run the full verification suite after merging (green before merge ≠ green after merge; confirm no check stopped running).
5. Notify branch authors to sync the updated primary cycle branch (`PROJECT.md` §8) into active worktrees.
6. Run only one full heavy test suite at a time across worktrees (`PROCEDURES.md` GP-6).

---

## P-6. Handling a Red Gate

1. Re-run to rule out transient/shared-tree noise.
2. Isolate ownership via `git status` / `git diff` / clean `HEAD` stash (`PROCEDURES.md` GP-5).
3. Classify the failure into one of four categories:
   - **Code/data violated a rule** → fix the code/data; do not raise the ceiling.
   - **Code/data beat the baseline** → lower/tighten the baseline freely.
   - **Derived artifact is stale** → run the generator (`PROJECT.md` §5).
   - **Gate itself is defective** (fixing it would degrade product quality) → fix the gate.
4. Never raise a baseline without recording which underlying data changed; never leave a gate red as a reminder.

---

## P-7. Introducing or Retiring a Role

Create a new role **only** when all three hold: (1) distinct proof instrument no existing role holds, (2) recurs across cycles, (3) everyone touches it partially so nobody finishes it.
*Do NOT create a role for:* a new directory (assign an Executor), a single check (add a gate), or overload (add a second session in the existing role).

**Creation steps:**
1. Draft `roles/<NAME>.md` from `templates/ROLE.md` (must justify why adjacent roles cannot cover it).
2. Create `memory/<role>/` seeded with real entries in `project.md`/`craft.md` (never write another role's `decisions.md`).
3. Register the role in 6 places: `TEAM.md` §2 · `README.md` · `DIAGRAMS.md` (`D-5`/`D-9`) · `PROJECT.md` · `SOURCES.md` (if new discipline) · `templates/memory/README.md`.
4. Mark any superseded rule inline rather than erasing.
5. Onboard with the 6-part prompt from `templates/ROLE.md` (no preset expected outcome).
6. Review after one shift: if it yielded zero findings an adjacent role would not have caught, retire it.

**Retiring a role:** Do not delete the file — add `"Retired <date> because <reason>"` at the top and mark registration rows so the team remembers why and does not reinvent it blindly.

---

## P-8. Updating Team Documentation

Route edits to their single owner (`TEAM.md` · `PLAYBOOK.md` · `SOURCES.md` · `roles/<ROLE>.md` · `memory/<role>/` · `board/` · cycle decision log) to prevent duplication.

1. Record accepted improvements the same day; never add a rule without an originating incident.
2. Mark superseded wording inline rather than erasing; date-stamp numbers.
3. Update backlinks (`TEAM.md`, `DIAGRAMS.md`, `PROJECT.md`).
4. Review participant self-edits (`PROCEDURES.md` GP-13) on three axes: incident named · old wording preserved · rule not softened where it restricted the author. Stop uncommissioned doc-editing batches immediately.

---

## P-9. Retracting a Finding

1. Verify the retraction via an independent path.
2. Immediately notify every recipient (including the Owner if already escalated).
3. Separate retracted explanation from any still-valid raw measurement.
4. Remove retracted cases from `PLAYBOOK.md` examples if cited; log error shape in `decisions.md`.
5. Praise the author publicly so future sessions never hide false alarms.

---

## P-10. Closing or Reassigning an Idling Session

Close a session when its territory is finished with no follow-up queued, when blocked longer than remaining work, or the moment you catch yourself inventing busywork. Request its final report & memory update, clear its board status, and notify the Owner.

---

## P-11. Writing Consolidated Questions for the Owner

* Batch all Owner questions into **one consolidated list** — never scatter across reports.
* Write in plain human language (*"will someone who did not open the code today understand this?"*): start with **what changes for the user**, give a concrete example, list dated numbers, present **options (including "leave as is") with trade-offs/costs**, state **your recommendation and why**, and note what is blocked waiting on the answer. Put internal card IDs/symbols only as trailing references.

---

## P-12. End of Shift

**Step 0 (Mandatory First):** Check every worktree for uncommitted changes (`git status --short`). Account for every dirty file by name (`P-16`).

1. Run verification checks and save output inside the repo/artifacts (never chat-only or `/tmp`).
2. Sync `board/00-START.md` from `board/OPEN.md`; prune `board/DONE.md`.
3. Update `STATE.md` (who worked on what, branch/worktree state, exact next step — primary crash-recovery checkpoint).
4. Append new insights to `memory/lead/`.
5. Merge ready branches or record why each remains unmerged.
6. Send one concise Owner summary: closed · open · waiting on Owner.

### P-12-bis. End-of-Shift Documentation Housekeeping
Answer with concrete numbers/filenames (never *"looks fine"*):
1. Lines added vs. lines removed in team docs (watch for monotonic bloat).
2. Every new doc linked in the reading route (`TEAM.md` / `README.md`).
3. Cited numbers in touched docs cross-checked against live runs.
4. Any pattern seen twice added to `PLAYBOOK.md` / `DIAGRAMS.md`.
5. Legacy/quarantined items reviewed.
6. Duplicate tracking lists eliminated.
7. Merged, clean worktrees removed (verify `0` unmerged commits, `0` modified files, `0` untracked files first).

---

## P-13. Closing a Cycle

Verify **every** acceptance criterion (never close by majority vote):
1. All static checks (`typecheck`, `lint`) and test suites green.
2. External CI green end-to-end on the exact final commit.
3. Every active session filed a report with before/after measurements and unverified blind spots.
4. Live Runtime QA acceptance passed with evidence (`PROJECT.md` §7).
5. `STATE.md` / project status reflects actual `HEAD`.
6. No baseline raised without documented rationale.
7. Remaining Owner questions consolidated into one list.

---

## P-14. Recovery After an Interruption

1. **Delete nothing, revert nothing, reset nothing.**
2. Inspect `git worktree list`, `git status` across all trees, and file `mtime`s.
3. Snapshot uncommitted diffs outside `/tmp`.
4. Treat all uncommitted edits as unverified until matched to a live passing check run.
5. Check subagent transcripts/logs if applicable, and read `board/OPEN.md` + `STATE.md`.

---

## P-15. Continuous Learning as Team Lead

The Team Lead is not just a dispatcher — leadership improves by learning:
* Consult [`SOURCES.md`](../SOURCES.md) alongside specialists when making standard/threshold decisions.
* Treat corrections from Executors and Auditors as your cheapest high-signal knowledge source.
* Post-mortem your own mistakes by naming the failure shape and adding a guardrail.
* Record new lessons in `memory/lead/` (*what you know now that you did not know before*) without letting reading displace active unblocking.

---

## P-16. Rescuing Orphaned Work in a Shared Tree

When uncommitted changes sit in a worktree and nobody claims authorship:

1. **Confirm authorship explicitly** by asking active sessions (silence ≠ unclaimed).
2. **Assess without editing:** file list, diff size, static checks & tests (compare test count against prior baseline).
3. **Map to open plans/cards** (label unidentified work honestly rather than guessing).
4. **Commit with an explicit `"NOT MY WORK"` header** listing: (a) what you verified live, and (b) everything **not** verified (before-measurements, mutation checks, runtime QA).
5. **Mark board status `"code present, acceptance pending"`** — never `"done"`, never revert unfamiliar work blindly, never claim authorship by omission.

---

## Seven Failure Modes of a Team Lead

| ID | Failure Mode | Fast Diagnostic Sign | Remedy |
| --- | --- | --- | --- |
| **FM-1** | **Instance mistaken for a class** | Fixed one local artifact/restart rather than the root generator/code | Label fix *"instance"* vs. *"class"* before closing; keep card open if instance |
| **FM-2** | **Stale number treated as live fact** | Number lacks run date or commit hash | Re-run command in this shift before quoting |
| **FM-3** | **Plan asks what code already answers** | Plan item asks *"how does X work?"* for existing code | Read code first and convert to a factual section |
| **FM-4** | **Filter hid what was measured** | Probe returned `0` without printing total scanned or positive control | Require scanned count + positive control input |
| **FM-5** | **Reader wired to unpopulated data** | Code reads a new field that nothing populates yet (tests stay green!) | Populate/tag data first, wire consumer second |
| **FM-6** | **Task assigned to a dead/gone session** | Prompt sent, zero acknowledgment, zero worktree changes | Verify receipt + worktree liveness (`P-3-bis`) |
| **FM-7** | **Role stretched past its authority** | Assignment asks a read-only role to edit code (e.g., *"Analyst: write gate"*) | Match role to task verbs; hand edits to Executor/Tester |
