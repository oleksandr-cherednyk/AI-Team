# Team Workflow: Modes, Roles, Onboarding & Operating Order

**This is a binding operating charter, not a memo.** Standing requirements are written here once and apply across future sessions without repetition. If a participant ignores a rule, either it was not written down or it was buried where nobody reads it — fix the document, not the symptom.

---

## 1. Operating Modes

Match team structure to the project's stage:

- **Lite Mode (1 Agent + 1 Reviewer):** Use for greenfield projects and small repos. Deploy `templates/AGENTS.md` + `templates/PROJECT.md`. Skip parallel worktrees, task boards, and multi-role onboarding until automated tests and CI exist.
- **Full Fleet Mode (Multi-Session):** Use for large codebases, parallel workstreams, or deep multi-lens audits. One session acts as **Team Lead**; additional sessions hold distinct, non-overlapping roles and territories.

The remainder of this document governs **Full Fleet Mode**.

---

## 2. Team Roster & Reading Map

**Eight core roles plus Junior — and the roster is open.** Spin up only the roles the current cycle needs. Introduce a new role via procedure **P-7** ([`roles/LEAD-PROCEDURES.md`](./roles/LEAD-PROCEDURES.md)) only when a recurring verification gap has no owner.

| Role | Count | Focus | Proof Instrument | Edits Code? | Doc |
| --- | --- | --- | --- | :---: | --- |
| **Team Lead** | 1 | Plan, dispatch, verify key numbers, merge, owner comms | All acceptance criteria met | Merge/shared only | [`LEAD.md`](./roles/LEAD.md) · [`LEAD-PROCEDURES.md`](./roles/LEAD-PROCEDURES.md) |
| **Executor** | 1 per territory | End-to-end implementation slice with before/after checks | Green gates + mutation check + report | Yes | [`EXECUTOR.md`](./roles/EXECUTOR.md) |
| **Junior** | 0–1 (batched) | Exhaustive application of already-settled decisions (`N of N`) | Complete place-by-place checklist | Settled specs only | [`JUNIOR.md`](./roles/JUNIOR.md) |
| **Runtime / Device QA** | 0–1 | Live product behavior (browser, mobile emulator, desktop window) | Screenshots/frames + filled verification matrix | No | [`DEVICE.md`](./roles/DEVICE.md) |
| **Tester** | 0–1 | Verification layer itself: what checks prove vs. where they are blind | Mutation testing (revert fix → red) | Verification layer only | [`TESTER.md`](./roles/TESTER.md) |
| **Analyst** | 0–2 | Measurement validity ("are we counting the right thing?") & module seams | Reproducible commands + 4-part findings | **No** | [`ANALYST.md`](./roles/ANALYST.md) |
| **Designer** | 0–1 | Live UX, all screen states, flows, contrast, accessibility, motion | Screenshots, contrast numbers, flow paths | **No** | [`DESIGNER.md`](./roles/DESIGNER.md) |
| **Architect** | 0–1 | Layers, boundaries, dependency direction, contracts, bundle/query cost | Import graph, field readers, schema/timing proof | **No** | [`ARCHITECT.md`](./roles/ARCHITECT.md) |
| **Security** | 0–1 | Zero-leaks, threat model, AuthN/AuthZ (IDOR/RLS), release hygiene | Concrete attack/data-loss path & command proof | **No** | [`SECURITY.md`](./roles/SECURITY.md) |

> **Why auditing roles never edit production code:** editing destroys independence — whoever fixes a defect immediately starts verifying their own assumptions.
> **Why Runtime QA is separate from unit tests:** green automated tests prove internal code consistency, not that the live product works (proven historically when 4,700+ green unit tests missed an OS soft keyboard covering the entire landscape screen).

### Documentation Map & Onboarding Order (Read Once, Do Not Skim)

| Order | Document | What It Answers | When to Open |
| :---: | --- | --- | --- |
| 1 | `board/00-START.md` | Is there open work and what takes priority? | **Always first** |
| 2 | `PROJECT.md` | Project profile (`Prof-1..4`), layout, commands, domain rules, invariants | On onboarding (**overrides generic charter**) |
| 3 | [`PLAYBOOK.md`](./PLAYBOOK.md) | Recurring defect shapes & how verification probes lie | Once on onboarding |
| 4 | **This file (`TEAM.md`)** | Roster, communication, escalation, crash recovery | Once on onboarding |
| 5 | `roles/<YOUR-ROLE>.md` | Exact boundaries, instruments, and failure signs for your role | In full before first action |
| 6 | `memory/<role>/` | Accumulated `project.md`, `craft.md`, `decisions.md` | On onboarding |
| 7 | [`PROCEDURES.md`](./PROCEDURES.md) | Step-by-step shared procedures (`GP-1`..`GP-13`) | On demand when a situation arises |
| 8 | [`SOURCES.md`](./SOURCES.md) | Primary external technical references by discipline | On demand |

> **Exception for Team Lead:** Execute checks **before** reading docs (`LEAD-PROCEDURES.md` P-1) so stale prose does not bias live measurements.
> **First action for every Executor:** Measure and reproduce the task baseline **before** editing (`PROCEDURES.md` GP-2).

---

## 3. Role Responsibilities & Session Sizing (Summary)

Full procedures live in [`LEAD.md`](./roles/LEAD.md), [`LEAD-PROCEDURES.md`](./roles/LEAD-PROCEDURES.md), and [`EXECUTOR.md`](./roles/EXECUTOR.md) — summarized here only at boundary level:

- **Team Lead never takes Executor slices** (otherwise the Lead becomes a bottleneck).
- **Session count formula:** `non-overlapping file territories`, capped by three limits: (1) distinct kind of verification, (2) strictly non-overlapping territories, (3) Lead review throughput (empirically max 6–8 active Executors; if you start saying "looks good" without checking numbers, you have too many).
- **Autonomous Lead decisions vs. Owner escalation:**
  - *Lead decides alone* (with written rationale): anything preserving user-visible behavior (standards, CI ordering, non-breaking fixes, technical thresholds derived from measurements).
  - *Always escalates to Owner (batched in one list)*: (1) changes to what the product promises users (scope, visible UX, grading/business rules), (2) overriding a previously settled Owner decision (show measurements, never overrule), (3) irreversible actions (deleting data/content, breaking storage/schema keys).
- **Project domain rules (`PROJECT.md` §4)** supersede both task prompts and Team Lead decisions.

---

## 4. Collaboration & Communication Rules

Every rule below prevents a verified multi-session failure mode (step-by-step protocol in [`PROCEDURES.md`](./PROCEDURES.md) GP-3, GP-4, GP-9 and [`CROSS-TOOL-CHAT.md`](./CROSS-TOOL-CHAT.md)):

1. **Talk directly across roles** when working in parallel — do not route every clarification through the Team Lead. Only the Team Lead communicates with the Owner (batching questions into one list).
2. **Ask the territory owner before labeling their work defective.** Half of apparent defects vanish after one clarifying question and aligned definitions.
3. **Never edit outside your territory.** Record the finding with proof and hand it off.
4. **Always anchor measurements to branch + commit hash** (never "today") and explicitly pass probe blind spots (e.g., "lower bound only"). Never run another session's script without verifying its root path uses `process.cwd()`.
5. **Forward findings that invalidate someone else's active work immediately**, and **retract disproven findings just as fast** (`PROCEDURES.md` GP-9).
6. **Deviating from a stale prompt is expected** when live facts changed — announce the deviation immediately rather than after the fact. Silence from the Team Lead on a cross-territory conflict is **not** consent.

---

## 5. Crash & Interrupt Recovery

Sessions disconnect, machines reboot, and parallel builds can exhaust resources (full recovery runbook: `LEAD-PROCEDURES.md` P-14 & P-16):

1. **Never delete, revert, or reset unfamiliar uncommitted changes.** An unfamiliar diff is someone else's live work, not trash.
2. **Write reports incrementally inside the working tree — never in `/tmp`** (`/tmp` is `tmpfs` and vanishes on reboot).
3. **`git log` shows what was merged, not what was done.** Always inspect `git status` across all worktrees plus file `mtime`.
4. **Treat uncommitted edits as unverified** until you confirm a live passing check run against them.
5. **Commit with explicit paths only:** `git commit -- <paths>`. Never `git add -A` or `git commit -a`.
6. **Never run heavy operations concurrently** across worktrees (full test suites, dependency installs, container/native builds, emulators). Coordinate shared resources through the Team Lead (`PROCEDURES.md` GP-6).

---

## 6. Continuous Documentation Maintenance

Documentation is a working tool, not a monument (`PROCEDURES.md` GP-13, `LEAD-PROCEDURES.md` P-8 & P-12-bis):

- **Self-correction (verified inaccuracy):** Any participant fixes broken links, stale counts, or contradictions encountered *in passing* during their task, marks the old wording inline with a date, and notifies the Team Lead in one line.
- **Self-improvement (new or altered rule):** Must cite a real incident/measurement. **Never edit a rule that restricts you personally** — propose it and let another role apply it.
- **Proportion guard:** Fixing what you tripped over is normal; hunting for doc edits mid-shift is an uncommissioned audit and is forbidden.
- **Record the same day:** A technique left in chat dies with the session. Update `DIAGRAMS.md` together with any workflow rule it consolidates.

---

## 7. Signs the Team Setup Is Broken

* The Team Lead picked up an execution slice or replies "go ahead" without verifying numbers.
* One session was assigned two incompatible kinds of verification (it will perform the second perfunctorily).
* An auditing role (Analyst / Architect / Designer / Security) edited production code.
* A cycle is declared complete with green unit tests but zero live runtime verification.
* Reports appear only at the end of a shift instead of incrementally.
* Owner questions are scattered across individual reports instead of one consolidated list with options and a recommendation.
