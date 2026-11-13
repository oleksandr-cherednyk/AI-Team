# General Procedures — For All Participants

**Established by the Owner on 2026-08-29.** Companion to [`TEAM.md`](./TEAM.md): `TEAM.md` explains
**what and why**; this document explains **how, step by step**.

> "Instructions can also be maintained for others, for all participants; perhaps some instructions
> will be general." — Owner, 2026-08-29

**This applies to all roles.** On top of these, the Team Lead has fifteen role-specific procedures in
[`LEAD-PROCEDURES.md`](./roles/LEAD-PROCEDURES.md). Procedures **evolve and improve** under the rules
of `TEAM.md` §6 and §8.

**Table of Contents**

| | Situation |
|---|---|
| **GP-1** | [Starting Work](#gp-1-starting-work) |
| **GP-2** | [First Action: Measure, Do Not Edit](#gp-2-first-action-measure-do-not-edit) |
| **GP-3** | [Found Something: How to Format a Finding](#gp-3-found-something-how-to-format-a-finding) |
| **GP-4** | [Found a Defect Outside Your Territory](#gp-4-found-a-defect-outside-your-territory) |
| **GP-5** | [Red in a Test Run](#gp-5-red-in-a-test-run) |
| **GP-6** | [Need a Shared Resource](#gp-6-need-a-shared-resource) |
| **GP-7** | [Committing](#gp-7-committing) |
| **GP-8** | [Writing a Report](#gp-8-writing-a-report) |
| **GP-9** | [Retracting Your Own Finding](#gp-9-retracting-your-own-finding) |
| **GP-10** | [Proposing an Improvement to Your Own Work](#gp-10-proposing-an-improvement-to-your-own-work) |
| **GP-11** | [Found a Source or a Pattern](#gp-11-found-a-source-or-a-pattern) |
| **GP-12** | [Finished a Part, Pausing, or Wrapping Up](#gp-12-finished-a-part-pausing-or-wrapping-up) |
| **GP-13** | [Found an Error or Improvement in the Documentation Itself](#gp-13-found-an-error-or-improvement-in-the-documentation-itself) |

---

## GP-1. Starting Work

1. **`board/00-START.md`.** `✅` means there is no open work. Otherwise, read what has already been
   found and remains undone — **by the relevant lines**, not the entire file.
2. **[`PLAYBOOK.md`](./PLAYBOOK.md)** — ten minutes so you do not start cold.
3. **[`TEAM.md`](./TEAM.md)** §2 — general reading list.
4. **Your own role document** in full.
5. **Your own memory** — `memory/<role>/`: `project.md`, `craft.md`, `decisions.md`.
6. **[`SOURCES.md`](./SOURCES.md)** — your section, if the task requires external domain knowledge.
7. `PROJECT.md` and `PROJECT.md` §4, if you are working with project material.

**Only after completing these steps should you take your first action.**

---

## GP-2. First Action: Measure, Do Not Edit

**Every number in a task assignment is considered unverified until you reproduce it yourself.** That
includes the Team Lead's numbers: in a single day, Executors caught six inaccurate numbers from the
Team Lead.

* **A number that fails to reproduce is a finding, not a typo.** Report it immediately: other
  people's decisions may depend on it;
* **An expectation in the assignment is not a fact.** "Some cells will drop off" is something to be
  verified, not executed blindly;
* **Every measurement must state what state it was taken against** — branch and commit, not "today".
  There are nine working trees.

---

## GP-3. Found Something: How to Format a Finding

**Four parts, none optional:**

1. **What is observed** — reproducible by command, screen, or frame;
2. **How it was measured** — command, number, file. "Looks wrong" is not a measurement;
3. **What it means** — for the learner/user or the work. **If nothing, say so explicitly**: a
   finding without user impact can still be valid, but its weight is different;
4. **Whose territory it belongs to and what is proposed.**

**Before sending — three checks, each of which caught an error today:**

* **Read the actual cases, not just the count.** A false finding is more dangerous than a missed
  one: a miss sits quietly, whereas a false finding wastes other people's time and travels upward as
  fact;
* **Verify input reachability.** A code branch that no live scenario can reach is not a defect;
* **Test round numbers against the probe, and large numbers against meaning.** "348 out of 348" is
  almost always a defect in the probe; "3015 out of 4094" is almost always the answer to the wrong
  question.

**If a finding changes someone else's work, send it immediately** — before doing anything else.

---

## GP-4. Found a Defect Outside Your Territory

1. **Do not fix it yourself.** Two sessions editing the same file cost more than handing off the
   finding.
2. **Ask the territory owner directly** (`ListAgents` → `SendMessage`) **before calling their work
   defective.** Half of all "defects" are resolved with a single question.
3. **Ask the Team Lead who authored someone else's work.** `git log --author` **does not distinguish**
   participants — everyone commits under the same user name; only the branch and commit message
   prefix distinguish them. And subagents do not appear in `ListAgents`.
4. **Hand over the measurement, not your explanation of the cause.** A number and its explanation
   have different reliability; bundling them together lends the explanation the weight of the number.
5. **Along with the number, state the boundaries of your probe** — where it is blind. "74" without
   the words "lower bound" will be read a month later as "defect closed".

---

## GP-5. Red in a Test Run

1. **Re-run it.** A single run in a shared tree proves nothing.
2. **Determine whose red it is** — `git status`, `git diff`, or better yet: **stash your changes
   with `git stash` and run on a clean HEAD.** That is how you prove non-involvement rather than
   merely claiming it.
3. **`pnpm test` runs `pnpm -r` and stops on the first package failure.** A failure in `lang-en`
   means `apps/mobile` **never ran at all**, and the log will not make that look like a skip. **A
   green `pnpm test` proves the mobile app passed only if its lines actually appear in the log.**
4. **Do not run tests while editing the tree** — yours or a neighbor's. The test runner will see new
   code paired with stale data.
5. **`pnpm test` does not type-check** — Jest runs through Babel. Run both before committing.
6. **If the red belongs to someone else, hand it to the owner** — do not fix it blindly.

---

## GP-6. Need a Shared Resource

**Ask the Team Lead; do not just grab it.**

* **Shared generators** (listed in `PROJECT.md` §5) — coordinate strictly through the Team Lead, one
  at a time: two simultaneous runs create a race condition that no tool will diagnose;
* **Full test suite run** — one at a time: nine trees spawn 28 Jest processes and exhaust memory to
  zero. Everyone else should run targeted tests on their own surface;
* **Emulator and Gradle build** — one at a time, and never concurrently with someone else's
  `pnpm install`. **"The emulator is free" expresses someone's intention, not the state of the
  device**;
* **Someone else's ratchet baseline** — ask the owner. Moving it down is always allowed; moving it
  up requires an explanation of which underlying data changed.

---

## GP-7. Committing

* **Always use `git commit -- <explicit paths>`.** Never `git add -A`, never `git commit -a`, and
  **not even `git add <paths> && git commit`** — the latter was tested during an incident and does
  not protect you: `commit` without a pathspec commits the entire staged index, including someone
  else's staged files;
* **Run `git status` before committing.** Any file you did not touch must not be included;
* **Declare any new artifact in the same commit** as the code that reads it: a script goes into
  `package.json` and the `test` chain; a policy goes wherever the gate expects it;
* **The commit message explains "why", rather than listing "what".** Numbers in the message must be
  measured;
* **Commit finished work promptly instead of stockpiling it.** In a diff, a half-finished edit in
  the working tree is indistinguishable from a finished one — that has already cost us an
  investigation. **Only what reaches the git tree survives.**

---

## GP-8. Writing a Report

**Maintain your own report file** (location specified in `PROJECT.md` §8), **written as you go** —
not at the very end, and **never in a temporary directory**, which gets wiped on reboot (a reboot
once wiped the scratch files of nine sessions at once).

Required elements:

* **"Before" snapshot** — the state of the gates upon entry, so someone else's red is not attributed
  to you;
* **Measurements with the exact command** used to capture them;
* **"What remained unchecked"** — listed by name, never "spot-checked";
* **"What I tried to measure and why this metric fails to measure it"** — documenting a flawed probe
  saves the next session a wasted pass; without a record, they will build it again;
* **Boundaries of your probe** — where it is structurally blind;
* **"No defects found" must be recorded in the same detail as a finding.** Stating "this area is
  clean and here is why" saves the next shift the exact same hour.

---

## GP-9. Retracting Your Own Finding

**Send a retraction just as immediately as a finding.**

1. Verify the retraction with the same rigor as the original finding;
2. **Notify everyone who received the finding** — starting with the Team Lead, who may have already
   propagated it further;
3. **Separate what is retracted from what still holds.** Usually the explanation is retracted while
   the underlying number remains valid;
4. **If a rule was supported by this case, say so.** The rule itself may still hold in general, but
   it must not remain propped up by a retracted case;
5. **Record the shape of the error** in `decisions.md`, not merely the fact of the retraction.

**A retraction is real work, not a confession.** Five retractions occurred in a single shift, and
each one saved someone else's time.

---

## GP-10. Proposing an Improvement to Your Own Work

**This is both your right and what is expected of you** (`TEAM.md` §6). Rationale: **the session
executing a role knows more about it than the person who wrote its description.**

You may propose changes to: your task specification · your role instructions · coordination
procedures · a new role.

**How:** send it to the Team Lead with the exact same standard required of a finding — **how it was
measured or which specific case it was derived from.** "It would be more convenient" is not a
proposal; "this rule produced a false red right here, and here is why" is a proposal.

**Deviating from an instruction is permissible when the underlying circumstances have changed** —
but state it **immediately and directly**, not after the fact. The Team Lead writes task assignments
based on the state known at the time of writing.

---

## GP-11. Found a Source or a Pattern

| What you found | Where it goes |
|---|---|
| A high-quality **source** | To the Team Lead → [`SOURCES.md`](./SOURCES.md). Primary sources only: specifications, official documentation. Blog summaries and trend roundups are not sources |
| A **pattern** useful to all roles | To the Team Lead → [`PLAYBOOK.md`](./PLAYBOOK.md) |
| Knowledge specific to **your role** | Yourself → `memory/<role>/craft.md` or `project.md` |
| A **decision** that proved right or wrong | Yourself → `memory/<role>/decisions.md`, **high bar**: non-obvious · wrong · non-standard · changed someone else's work. **Outcome is mandatory** |
| **Work item**: finding, question, plan | Yourself → `board/OPEN.md` |

**A rule without a concrete case is never recorded anywhere** — there is nothing to verify it with
and nothing to repeal it by.

---

## GP-12. Finished a Part, Pausing, or Wrapping Up

**If you finished a part and have not received the next one** — write your report and wrap up.
**Never exit silently:** the Team Lead will not know you finished, and your territory will sit idle.

**If you are blocked waiting on something** — switch to your next task rather than sleeping.

**A pause means a pause, including cheap checks.** Formulation by Device / Runtime QA, 2026-08-29,
adopted verbatim:

> "The check is cheap, but it is still work on the same task, and **its cheapness does not make it
> different work**. If I decided on the Team Lead's behalf that 'this little bit is fine,' a pause
> would cease to be a pause — and next time I would make the same call about something more
> expensive."

**A pause must state what CONTINUES, not just what stops.** A test run, an edit, or a commit has an
explicit command — they are visible and easy not to start. **Correspondence, root-cause analysis,
and self-checks have no command, so at the moment of decision there is nothing to weigh them
against, and they continue on their own momentum.** Executor's formulation, 2026-08-29: *it is easy
to stop when work looks like work, and hard when it looks like a conversation.*

> **Test:** would this appear in the report as a "completed" line item? If yes, it is work, and it
> does not happen during a pause.

By this test, investigating someone else's bug is work; a brief status report is not. The case from
which this rule was derived: two sessions declined a one-minute verification check and immediately
spent several messages conducting an analysis that cost more than the declined check.

**Discipline breaks down not on expensive tasks, but on cheap ones:** a cheap task always looks like
an exception. When you see work that needs doing, **propose options in descending order** instead of
asking permission: the lead will be able to pick one in five seconds.

**Boundary of this rule:** it applies **under an explicit instruction to pause**, not during normal
work. It is neither a ban on cheap checks nor a reason to ask permission for every minor step —
otherwise we get the opposite extreme, where an Executor does nothing without clearance.

**Pausing or wrapping up — in order:**

1. **Reach a safe point.** A half-finished edit in the working tree is the worst thing you can leave
   behind: in a diff, it is indistinguishable from a finished one;
2. **Commit finished work** using explicit paths;
3. **Update your report** to reflect the current state;
4. **Update your memory** — `project.md`, `craft.md`, `decisions.md`. Not "what I did" (that belongs
   in the report), but **what I know now that I did not know this morning**;
5. **Report to the Team Lead:** what is closed · what is mid-flight and in what state · **what is
   uncommitted and why** · what is blocking you.

---

## GP-13. Found an Error or Improvement in the Documentation Itself

> **Owner's requirement, 2026-08-29:** "Self-correction of inaccuracies or problems is permitted if
> there is a verified issue in the documentation. Self-improvement of the documentation is permitted
> if it is verified and reliable. If it is all reliable, that is actually a good thing — because
> during work you can spot errors, problems, or ways to improve how the team operates."

**Documentation is a working tool, not a monument.** It is written by the same sessions that write
the code, and it errs in the same ways. Silently stepping around an inaccuracy is worse than fixing
it: ten more people will step around it, and each will waste their time all over again.

### Two Cases with Different Thresholds

**A. Self-correction — an inaccuracy.** Fix it **yourself and immediately**, and inform the Team
Lead in the same message where you report the rest of your work.

What counts as self-correction:

* **A number fails to reproduce** — you re-measured and got a different value;
* **A named file, flag, or command does not exist** or has been renamed;
* **A broken link**, wrong section numbering, or duplicate entry;
* **A rule contradicts another rule** within the same documentation;
* **An example was retracted** — the case supporting a rule turned out to be invalid. In that event,
  **the rule itself may remain**, while the illustration is removed and stated explicitly.

**B. Self-improvement — a new or modified rule.** Also **permitted**, provided it is **verified and
reliable**: derived from a concrete case you can name, and it does not weaken a constraint that
restricts you. Notify the Team Lead **before or at the time of writing**, so they can object immediately
rather than finding out a week later.

### How to Edit — Four Mandatory Steps

1. **Name the case.** A rule without a case is neither added nor repealed — there is nothing to
   verify it with.
2. **Mark the previous wording alongside the new one; do not erase it.** It was valid for its time,
   and the next person must be able to see that, otherwise they will think a mistake was made. Use
   the same format as superseded memory entries:
   `**Previously:** … · **Updated <date> because …**`
3. **Date-stamp it.** A statement containing a number without a measurement date becomes a lie
   within a month.
4. **Tell the Team Lead** — in a single line: what you updated and on what grounds.

### This Is a Margin Note, Not Your Main Job

**Owner's requirement, 2026-08-29:** *Self-correction and self-improvement must not become full-time
work. The team must never spend its time working only on documentation. It is merely a margin note:
if an issue or discrepancy comes up, fix it and adjust it for the future — reliably and
double-checked.*

**Therefore the rule is: you only fix what you tripped over on the way to your actual task.** Never
what you went looking for.

| Do this (Yes) | Not this (No) |
|---|---|
| Opened a document for your task, a number did not match — fixed it and moved on | **Started the shift by auditing the documentation** |
| A rule failed on a live task — recorded the case | Created a task to "proofread team documents" |
| Tripped over a broken link — fixed it in the same commit | **Went searching for what else might be inaccurate** |
| Two edits in a shift | Ten edits in a shift — that is a different job, and nobody ordered it |

**Three signs that documentation editing has displaced real work:**

* **In the shift report, documentation takes up more space than the subject of the task**;
* Edits came **in a batch** — they were hunted down, not encountered;
* **None of them was preceded by a case** from real work: "I was reading and noticed" is an audit.

**A documentation audit is separate work and must be ordered separately.** The Team Lead can assign
it, or the Owner can request it; **it never starts on its own**. A participant who sees that many
small discrepancies have accumulated **mentions it in a single line** and continues their task —
they do not take it upon themselves to clean it up.

**This raises the reliability bar rather than lowering it.** Fixing something on the fly is no
excuse to skip verification: an inaccuracy must be **reproduced**, and an improvement must be
**derived from a case**. If in doubt, it is not a fix — it is a proposal to the Team Lead, worth one
line in your report rather than an edit to the file.

### By Default, Do Not Monitor Yourself

**Owner's clarification, 2026-08-29:** *By default, do not track how to correct or improve yourself.
It should surface only in the course of work — when you stumble across an issue, or when a crystal-clear
idea to improve the workflow emerges. If a problem is systemic, record it.*

**Think of it like an error log.** Nobody goes reading a log "just in case" — you **write to it when
an event occurs**, and open it when something does not add up. The same applies here: there is no
scheduled reflection, no self-check asking "has this document gone stale?", and no dedicated time
allocated to it.

**Three states — and the first is the default:**

| State | When | What you do |
|---|---|---|
| **Silence** — default | Nothing happened | **Nothing.** Do not search, cross-check, or re-read |
| **Stumbled across** | Hit an inaccuracy on the way to your task | Fix it on the spot, add one line in your report |
| **Clear idea** | A concrete improvement derived from a case | **Record it; do not implement it** |

**Recording does not mean executing.** An idea goes **as a single line onto the board** with status
`future`, and stays there. Work on it **is ordered by the Team Lead or Owner**, separately from the
current task. That is what recording means: the event is captured, and zero time is spent executing
it now.

**A systemic problem is one encountered more than once.** The first time it happens, fix it and move
on. If it recurs, create a card — because now you know it is not a coincidence. Creating a card for
a one-off occurrence only clutters the board with items nobody will ever pick up.

**Sign that proportion has been lost:** you opened a document **not for your task**, but to check
whether everything in it is still accurate. That is an audit, and audits are ordered, never
self-started.

### What You Must NEVER Fix Yourself

* **Owner decisions and product invariants** (`PROJECT.md` §1) — a discrepancy is demonstrated with
  a measurement, never overruled;
* **A rule that restricts you specifically.** Same principle as "the person checked by a probe does
  not edit the probe": if a rule gets in your way, you are an interested party. Propose the change,
  but let someone else's hands make the edit;
* **Someone else's memory** (`memory/<role>/`) — you may read it, never edit it. There is only one
  exception: the Team Lead may **mark** an entry as superseded without erasing it;
* **Anything in another role's document** that alters its boundaries. Fixing a typo is fine.

### Signs That an Edit Is Damage, Not Improvement

* **The document grew longer, without adding any new reason to open it**;
* **A rule was softened in the exact spot where it inconvenienced you on this task**;
* **The concrete case** from which the rule was derived disappeared, leaving only a bare assertion;
* **The previous wording was erased**, making it impossible to tell from the document that it
  changed.

**One-sentence test:** *Could I justify this edit to the person it restricts?* If not, it is a
proposal, not a self-correction.
