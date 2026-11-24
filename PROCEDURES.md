# General Procedures — For All Participants

Companion to [`TEAM.md`](./TEAM.md): `TEAM.md` explains **what and why**; this document explains **how, step by step**. Team Lead–specific procedures live in [`roles/LEAD-PROCEDURES.md`](./roles/LEAD-PROCEDURES.md).

**Table of Contents**

| ID | Situation |
| --- | --- |
| **GP-1** | [Starting Work](#gp-1-starting-work) |
| **GP-2** | [First Action: Measure, Do Not Edit](#gp-2-first-action-measure-do-not-edit) |
| **GP-3** | [Found Something: How to Format a Finding](#gp-3-found-something-how-to-format-a-finding) |
| **GP-4** | [Found a Defect Outside Your Territory](#gp-4-found-a-defect-outside-your-territory) |
| **GP-5** | [Red in a Test Run](#gp-5-red-in-a-test-run) |
| **GP-6** | [Need a Shared Resource](#gp-6-need-a-shared-resource) |
| **GP-7** | [Committing](#gp-7-committing) |
| **GP-8** | [Writing a Report](#gp-8-writing-a-report) |
| **GP-9** | [Retracting Your Own Finding](#gp-9-retracting-your-own-finding) |
| **GP-10** | [Proposing an Improvement](#gp-10-proposing-an-improvement) |
| **GP-11** | [Routing Knowledge: Sources, Patterns, Memory & Board](#gp-11-routing-knowledge-sources-patterns-memory--board) |
| **GP-12** | [Finished a Part, Pausing, or Wrapping Up](#gp-12-finished-a-part-pausing-or-wrapping-up) |
| **GP-13** | [Fixing or Improving Team Documentation](#gp-13-fixing-or-improving-team-documentation) |

---

## GP-1. Starting Work

Follow the exact onboarding order in [`TEAM.md`](./TEAM.md) §2:
1. `board/00-START.md` (if open work exists, read only relevant rows in `board/OPEN.md`).
2. `PROJECT.md` in full (active profile `Prof-1..4`, verification commands, domain rules, territories).
3. [`PLAYBOOK.md`](./PLAYBOOK.md) (once on entry).
4. [`TEAM.md`](./TEAM.md) + your `roles/<ROLE>.md` in full.
5. `memory/<role>/` (`project.md`, `craft.md`, `decisions.md`).
6. [`SOURCES.md`](./SOURCES.md) — your discipline's section only, when external authority is needed.

Take your first action only after completing these steps (Team Lead exception: run checks before reading per `P-1`).

---

## GP-2. First Action: Measure, Do Not Edit

**Every number in a task assignment is unverified until you reproduce it yourself** — including numbers from the Team Lead.

* **A number that fails to reproduce is a finding, not a typo.** Report it immediately: other decisions may rest on it.
* **An expectation in a prompt is not a fact.** Verify it empirically rather than confirming it blindly.
* **Anchor every measurement to branch and commit hash**, never "today."

---

## GP-3. Found Something: How to Format a Finding

**Four mandatory parts (none optional):**

1. **What is observed** — exact location (`file:line`, route, table, screen, or scene) reproducible by command or frame.
2. **How it was measured** — command, query, number, or artifact. *"Looks wrong"* is not a measurement.
3. **What it means (Impact)** — concrete consequence for the user or system. If none, state so explicitly.
4. **Whose territory it belongs to and what is proposed** — fix, owner question, or backlog item.

**Three pre-send checks:**
* **Read actual records, not just the count.** A false finding is worse than a miss: it burns teammates' time and travels upward as fact.
* **Verify input reachability.** Unreachable dead code is not a live runtime defect.
* **Distrust suspicious numbers:** round numbers (`0 of N`, `N of N`) usually signal a broken probe; huge numbers (`>70%` flagged) usually answer the wrong question.

If a finding changes another active session's work, send it **immediately**.

---

## GP-4. Found a Defect Outside Your Territory

1. **Do not fix it yourself.** Parallel edits to the same file cost more than handing off a finding.
2. **Ask the territory owner directly before labeling their work defective.** Half of apparent defects clear up with one question and aligned definitions.
3. **Confirm authorship via Team Lead** if running multiple sessions under the same git user (use branch/commit prefixes, not `git log --author`).
4. **Separate measurement from hypothesis.** Hand over the reproducible number clearly separated from your guess about root cause.
5. **State probe boundaries alongside the number** (e.g., *"lower bound; did not inspect X"*).

---

## GP-5. Red in a Test Run

1. **Re-run it.** A single run in a shared tree proves nothing.
2. **Isolate whose red it is:** check `git status` / `git diff`, or `git stash` your edits and test on a clean `HEAD`.
3. **Confirm the full runner actually executed all target packages.** Monorepo runners (e.g., `pnpm -r test`) often fail-fast on the first broken package and silently skip downstream packages — verify every expected package appears in the log.
4. **Never run test suites while a working tree is actively being edited** (yours or a neighbor's).
5. **Remember test runners ≠ typecheckers.** Transpilers like Babel/SWC skip TypeScript type checking — run both static typecheck and tests (`PROJECT.md` §3).
6. **If the red belongs to another territory, hand it to its owner** rather than patching blindly.

---

## GP-6. Need a Shared Resource

**Coordinate with the Team Lead before grabbing shared resources:**

* **Shared code/data generators** (`PROJECT.md` §5) — strictly one at a time, owned by Team Lead.
* **Full test suites & heavy builds** — run one at a time across worktrees to avoid OOM/disk exhaustion; other sessions run targeted checks on their own surface.
* **Shared testbenches** (mobile emulators, physical devices, shared staging DBs, desktop test rigs) — never assume a device is free just because it appears idle; confirm with Team Lead first.
* **Ratchet baselines** (`PROJECT.md` §6) — moving downward (tighter) is always allowed; moving upward requires owner/Lead approval and a recorded reason explaining which underlying data changed.

---

## GP-7. Committing

* **Always commit with explicit paths:** `git commit -- <path1> <path2>`. Never `git add -A`, never `git commit -a`, and avoid bare `git add <paths> && git commit` in shared trees (bare `commit` commits the whole staged index, including anything else staged).
* **Run `git status` before committing.** Nothing untouched by you enters your commit.
* **Wire new artifacts in the same commit** as the code that reads them (register new scripts/tests in `package.json`/CI/runners so they actually execute).
* **Commit messages explain "why"**, backed by measured facts rather than repeating the diff.
* **Commit finished slices promptly** — uncommitted edits look identical to half-finished work after a disconnect.

---

## GP-8. Writing a Report

Maintain your own report file (path defined in `PROJECT.md` §8), **written incrementally as you go** — never at the end, and **never in `/tmp`** (volatile `tmpfs`, wiped on reboot).

**Mandatory elements in every role report:**
* **"Before" snapshot** — baseline state of relevant gates/checks on entry.
* **Measurements with exact commands** used to produce them (anchored to branch + commit).
* **"Verified — Clean (and Why)"** — areas checked with zero defects, saving future sessions from repeating the pass.
* **"What Remained Unverified (Blind Spots)"** — listed explicitly by name, never vague "spot-checked."
* **"What I Tried to Measure and Why This Probe Failed" (Negative Instrument Results)** — record any metric, regex, or script that proved misleading or structurally blind (*what was measured → why it lied → what replaced it*). Without this paragraph, the next session builds the exact same broken meter again.

---

## GP-9. Retracting Your Own Finding

**Send a retraction just as fast as a finding.** Retracting a false alarm is high-value engineering work, not a failure.

1. Verify the retraction with the same rigor as the original finding.
2. Notify **everyone** who received the finding, starting with the Team Lead.
3. Separate what is retracted (often the explanation) from what still holds (often the raw measurement).
4. If any documented rule leaned on the retracted example, note it so the illustration is removed.
5. Record the error shape in `memory/<role>/decisions.md`.

---

## GP-10. Proposing an Improvement

Every participant has the right and duty to propose improvements to task specs, role instructions, or procedures (`TEAM.md` §6).

* **Standard of proof:** cite a concrete incident or measurement (*"this rule caused a false red in X"*), never *"it would be nicer."*
* **Deviating from stale instructions** when facts shifted is expected — announce it immediately rather than retroactively.

---

## GP-11. Routing Knowledge: Sources, Patterns, Memory & Board

| What You Found | Where It Goes |
| --- | --- |
| Primary external reference (official spec/docs; no blog roundups) | Via Team Lead → [`SOURCES.md`](./SOURCES.md) |
| Cross-role defect pattern or verification technique | Via Team Lead → [`PLAYBOOK.md`](./PLAYBOOK.md) (leave 1-line pointer locally) |
| Role-specific project fact or reusable craft lesson | Yourself → `memory/<role>/project.md` or `craft.md` |
| High-impact/non-obvious/wrong decision + **mandatory outcome** | Yourself → `memory/<role>/decisions.md` |
| Actionable work item (finding, owner question, plan item) | Yourself → `board/OPEN.md` |

Never record a rule without the concrete incident it came from.

---

## GP-12. Finished a Part, Pausing, or Wrapping Up

* **Finished and awaiting next assignment?** Write your report and close out — **never exit silently.**
* **Blocked?** Switch to the next unblocked item rather than idling.
* **Under an explicit Pause instruction:** *"Pause means pause, including cheap checks."* Any action that would appear as a completed line item (including root-cause analysis or side investigations) stops unless explicitly listed under what continues. *(Applies only during an explicit pause — do not ask permission for routine checks during normal work.)*

**Wrap-up checklist (in order):**
1. Reach a clean safe point (no half-edited broken files left behind).
2. Commit completed work with explicit file paths.
3. Update your report with final measurements and unverified blind spots.
4. Update `memory/<role>/` (*what you know now that you did not know before*).
5. Report to Team Lead: closed · in-flight · uncommitted (and why) · blockers.

---

## GP-13. Fixing or Improving Team Documentation

Documentation is a working tool — stepping silently around a verified inaccuracy forces future sessions to pay the same tax.

### Thresholds
* **Self-correction (verified inaccuracy):** broken link, stale count, renamed command, contradiction, retracted example → fix **immediately** in passing and inform Team Lead in one line.
* **Self-improvement (new/modified rule):** must derive from a real incident and **never weaken a constraint restricting you personally** → notify Team Lead before/as you write.

### Four Mandatory Edit Steps
1. Name the originating incident.
2. Mark superseded wording inline rather than erasing silently (`**Previously:** … · **Updated YYYY-MM-DD because …**`).
3. Date-stamp any cited measurement.
4. Notify the Team Lead in one line.

### Proportion Guard (Default State = Silence)
**Only fix what you tripped over while doing real work.** Never start a shift by proofreading docs, never batch uncommissioned doc tweaks, and never let doc edits overshadow the actual task. A full documentation audit must be **explicitly commissioned** by the Owner or Team Lead.

* **Never edit yourself:** Owner decisions / product invariants (`PROJECT.md` §1) · rules restricting your own role (propose; let another role edit) · another role's memory · another role's territory boundaries.
* **Sanity check:** *"Could I justify this edit to the person it restricts?"* If not, it is a proposal, not a self-correction.
