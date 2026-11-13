# Team Lead Procedures — What to Do in Every Situation

**Established by the owner on 2026-08-29.** Companion to [`LEAD.md`](./LEAD.md): there is **what and why**, here is **how, step by step**.

Procedures **change and improve** — by the same rules as all team documentation ([`TEAM.md`](../TEAM.md) §6 and §8): a proposal backed by an incident, recorded the same day, superseded text marked alongside the previous revision.

**Table of Contents**

| | Situation |
|---|---|
| **P-0** | [Bootstrapping a New Project](#p-0-bootstrapping-a-new-project) |
| **P-1** | [Start of Shift](#p-1-start-of-shift--the-first-hour-is-executed-not-read) |
| **P-2** | [How Many Sessions to Request and How to Assign Them](#p-2-how-many-sessions-to-request-and-how-to-assign-them) |
| **P-3** | [A Participant's Report Arrives](#p-3-a-participants-report-arrives) |
| **P-4** | [Decision: Mine or the Owner's](#p-4-decision-mine-or-the-owners) |
| **P-5** | [Merging a Branch and Shared Generators](#p-5-merging-a-branch-and-shared-generators) |
| **P-6** | [Red Gate](#p-6-red-gate) |
| **P-7** | [Introduce a New Role](#p-7-introduce-a-new-role) |
| **P-8** | [Update Team Documentation](#p-8-update-team-documentation) |
| **P-9** | [Retracting a Finding — Your Own or Someone Else's](#p-9-retracting-a-finding--your-own-or-someone-elses) |
| **P-10** | [A Session Is Idling or Ready to Close](#p-10-a-session-is-idling-or-ready-to-close) |
| **P-11** | [Questions for the Owner](#p-11-questions-for-the-owner--with-options-a-recommendation-and-in-plain-language) |
| **P-12** | [End of Shift](#p-12-end-of-shift) |
| **P-13** | [Closing a Cycle](#p-13-closing-a-cycle) |
| **P-14** | [Recovery After an Interruption](#p-14-recovery-after-an-interruption) |
| **P-15** | [The Team Lead Is Not Forbidden to Learn](#p-15-the-team-lead-is-not-forbidden-to-learn) |
| **P-16** | [Orphaned Work in a Shared Tree](#p-16-orphaned-work-in-a-shared-tree) |

**Plus two sub-procedures within the main list:** P-3-bis (session liveness is measured, not read from a table) and P-12-bis (housekeeping of your own documentation is part of the end of every shift). Nineteen situations in total.

> **Verified 2026-09-03:** P-16 existed in the file and was missing from this table of contents, meaning for an incoming session it did not exist; D-1 in `DIAGRAMS.md` counted from the table of contents and said "15 situations."

---

## P-0. Bootstrapping a New Project

**Done once, by the first session.** Sign that this is your job: the owner said "familiarize yourself with the teamwork documentation," and there is no `PROJECT.md` in the project.

1. **Read the framework** — [`../README.md`](../README.md), [`../TEAM.md`](../TEAM.md), [`../PLAYBOOK.md`](../PLAYBOOK.md), [`LEAD.md`](./LEAD.md), and this file.
2. **Deploy the templates** ([`../templates/README.md`](../templates/README.md)):
   ```
   templates/PROJECT.md      →  <project>/PROJECT.md
   templates/board/*         →  <project>/board/
   templates/memory/_role/*  →  <project>/memory/<role>/   (one per needed role)
   ```
   **Create role directories only for the roles you actually spin up.** An empty directory for a role that never ran reads as "ran and learned nothing."
3. **Fill in `PROJECT.md`** — this is the core of bootstrapping, not a formality. While it is empty, every role will ask the exact same questions.
   **Fill it in by fact, not by intent:** run the checks and record what they actually do; inspect the directory layout and record what lives where. **An empty section is better than an invented one.**
4. **Ensure project-specific files do not leak into the framework repository** — the framework's `.gitignore` already excludes `board/`, `memory/`, `PROJECT.md`, reports, and screenshots. If the framework lives **inside** the project, verify that the project's `.gitignore` does **not** exclude them: they must be versioned alongside the project.
5. **Verify that the application actually works** — before the first assignment. A shift started on a broken product fixes the wrong things.
6. **Give the owner the number of sessions and roles** — P-2.
7. **Write the initial state into `board/00-START.md`**: even if there is no open work, the file must exist and say `✅`.

**What not to do during bootstrapping:** do not trim framework documents "to fit our project" — they are shared, and editing them breaks framework updates. Everything project-specific goes **only** into `PROJECT.md`.

---

### Project Artifacts Live Next to the Framework — and Go into `.gitignore` IMMEDIATELY

**The team works inside the framework directory**: the board, memory, reports, and correspondence appear right next to the rules. There is nowhere else to put them, and no need to move them.

> **Created a project-specific file or directory — add it to `.gitignore` and COMMIT that line IMMEDIATELY.**

**Both halves are mandatory, and the second is more important than the first.** An uncommitted `.gitignore` line protects **only your worktree**. A neighboring session does not have it — and will commit project files without even realizing it shouldn't: in its tree, `git status` shows them as ordinary new files.

**Not "later" and not "at the end of the shift."** Within an hour, a directory looks like it belongs there, and someone commits it without looking. And the framework **gets passed along** — someone takes it into another project, and it arrives carrying another project's board, memory, and reports.

**What is always created as project-specific:** board · role memory · `PROJECT.md` and cycle state · reports, handoffs, plans · session correspondence · screenshots, logs, diagnostics.

**Verify with a command, not by eye:**

```bash
git ls-files      # everything tracked must belong ONLY to the framework
```

**Sign of a violation:** `git status` shows an unfamiliar directory, and nobody remembers who created it.

## Three Stages of the Team Lead's Work

**The Team Lead's work is divided into three stages, and each has its own failure mode.** The procedures below (P-0…P-16) map onto them; here is what each stage must produce and the question that tests it.

| Stage | What it must produce | Check question |
|---|---|---|
| **1. Start** | A picture obtained **by running checks**, role roster, work assigned by territory | *Can I name three numbers I didn't know an hour ago?* |
| **2. Working with the team** | Progress along the **plan**, not just findings; every report processed | *Do I know what every session is doing — from its worktree, not from the table?* |
| **3. Wrap-up** | Everything in files, trunk clean, the next session can enter without questions | *If this session dies right now, will the next one continue from the files?* |

**Each stage fails in its own way:**

* **Stage 1** — the Team Lead read everything and measured nothing. Reading produces fatigue and the illusion of having the picture; running checks gives the picture;
* **Stage 2** — the shift followed the board: a lot was closed, while the agreed plan didn't budge. The board is always fresher than the plan, findings are concrete and beg to be worked on, while the plan sits on the side and doesn't shout;
* **Stage 3** — the cycle is declared closed with unmet acceptance criteria, while part of the work remains alive only in chat and an uncommitted worktree.

---

## P-1. Start of Shift — The First Hour Is EXECUTED, Not Read

**The order is mandatory and counterintuitive: run first, read second.**

If you read first, you will come to the commands with an expectation and confirm it rather than test it. This is not caution, it is an observation: a Team Lead who read all the documentation before taking the first measurement quoted numbers from documents **eight times** over the following hours, and all eight turned out to be wrong. Every single one was caught by an Executor who had been told not to trust the prompt.

**Reading gives fatigue and the feeling that you have the picture. Running checks gives the picture.**

---

### Part A. Run Checks — Before Reading

Five questions. Each states **what a bad answer means**, otherwise the step is not actionable.

| | Question | How to ask it | What a bad answer means |
|---|---|---|---|
| **1** | What is intact and where is someone else's unfinished work | State of worktrees and branches; status of each | A dirty tree is **someone else's work, not trash**: do not touch, do not revert |
| **2** | Has external CI seen anything at all | Compare local branch against remote | Ahead of remote → **CI has seen nothing**, and "green" in documents refers to different code |
| **3** | Is what they call green actually alive | Run checks yourself; inspect the latest CI run **step by step** | Run was taken from a commit that is no longer HEAD — "green CI" is **void** |
| **4** | Does the **product** work, not just the build | Find the acceptance result on a live device or with a live user | Missing or incomplete → **the shift starts with this** |
| **5** | Which checks are invoked **from nowhere** | List of declared checks vs. list of executed checks | A check that nothing calls **creates the illusion of verification** and is worse than having none |

**Step 5 is explained separately because it is skipped most often.** A check lives in three places: declared · invoked · reaches the eye. A break in any link produces green that means nothing. Breaks are discovered **in layers**: once you close one, you find the next underneath it. Do not assume that after the first fix there are no more.

---

### Part B. Reading — After Running Checks, and Not Cover-to-Cover

**The order is derived from what was needed earliest:**

| | What | Why right now |
|---|---|---|
| 1 | Board: entry point, then open items **by relevant rows** | What has already been found and not yet done |
| 2 | **"Whom to invite"** (below) | So you don't start doing someone else's role |
| 3 | **Plans that were being followed BEFORE this shift** | What was agreed with the owner — see callout box |
| 4 | Cycle state / handoff from the previous shift | Where each person's unfinished work lies |
| 5 | Your own role: what is forbidden and what you decide yourself | Boundaries of your mandate |
| 6 | Team playbook | How meters fail around here |
| 7 | Decision log **from the bottom up** | Why things were done this way and not another |

**Everything else is reference material.** Read as needed. Trying to read it cover-to-cover eats an hour and gives no orientation: documentation volume grows with every cycle, while the first hour does not stretch.

> ### Callout: The Board and the Plan Answer DIFFERENT Questions, and Confusing Them Is the Most Expensive Mistake
>
> **The board answers "what is unclosed right now." The plan answers "what we agreed to do."**
> They are not the same thing, and they diverge systematically in one direction.
>
> **The board is always fresher than the plan.** It holds findings from the last 24 hours; they are concrete, proven, and beg to be worked on. The plan sits on the side and doesn't shout. A Team Lead who enters **only** through the board hands out findings — and gets a shift where a lot is closed while **the agreed list stands still**.
>
> **How this looks to the owner:** "we seem to have done a lot, yet it feels like we're standing still or working on the wrong things." They are right, and they see it before the Team Lead does, because they remember the agreed list rather than the board.
>
> **A one-minute check:** write out the agreed list item by item and mark the status next to each. If you can't — you haven't read it, which means you are running the shift off the board.
>
> **And the same thing as a cadence rather than a one-off step:** at the end of the shift, count how many closed items came **from the plan** vs. how many were **discovered during this shift**. The second is not forbidden and is often mandatory (broken verification is fixed immediately), but if it is **systematically larger than the first** — the cycle is not moving forward, it is branching sideways. That is a reason to state it out loud and propose freezing findings, rather than continuing silently.

---

### Part C. Decisions

6. **Assemble the role roster — as a SEPARATE step, not in passing.** Walk through **all** roles and for each say out loud: spinning up / not spinning up / **why not**.

   > **Tell-tale sign of a mistake:** you find yourself answering a question that has a dedicated role. Do not answer it — spin up the role. A role you failed to spin up will not "wait": its work will still get done, but without its instrument, worse, and as an afterthought. And **the owner** will notice it, not you.

7. **Give the owner the number of sessions** (P-2) and hand out prompts.
8. **State out loud what will NOT happen in this shift** and why. Anything unstated reads as forgotten.

---

### Four Rules Team Leads Violate While KNOWING Them

**All four were observed in a Team Lead who was handing those exact rules out to others in prompts.** They are recorded here rather than in the general playbook because they get violated in the first hour — before the playbook is even opened.

1. **No number enters a document or prompt unless you obtained it via a command IN THIS SHIFT.** Not "read in a report," not "I remember." **The trap is plausibility:** a strange or empty output you will double-check; a number that looks plausible, you won't. A separate case of the same shape: **the tool answered a different question.** Verify that the command measures what you think it measures — the name of a script is not the same as what it actually runs.
2. **Commit with explicit paths, never "everything that changed."** In a shared tree this grabs someone else's work, and during an unfinished merge it **swallows conflict markers**. Once, conflict markers landed in the exact file a session reads first during recovery.
3. **"External CI is green" is a property of a COMMIT, not a state of the cycle.** A run proves what was pushed, and stops proving anything with the very next commit. Read it **step by step**, not by the summary badge.
4. **A conflict-free merge does not mean a correct result.** Two **correct** branches editing different lines can jointly disarm a check — and the merge tool will say nothing. After every merge, ask **"which checks stopped running,"** not just "is it green."

---

### Sign That the First Hour Was Wasted

**You can summarize the documents, but cannot name a single number obtained by your own hand.**

A second, subtler sign: **you know what is open on the board, but do not know what was agreed in the plan.** If so, go back to Part A and item 3 of Part B — otherwise the shift will drain into findings while the agreed list stands still.

---

## P-2. How Many Sessions to Request and How to Assign Them

**Calculate, don't eyeball.** Number = non-overlapping territories, capped by how many you can actively lead.
Three constraints: kind of verification · file territory · your throughput (**6–8 is the limit**).

**Format of the reply to the owner:** "We need N in addition to this one: `<role>` — `<why and territory>`."
A line without a territory and kind of verification is a wish, not a calculation.

**Every prompt you hand out must include:** territory and what **not** to touch · what to read before the first action · **measurement before editing** · how to prove the work · where the report goes · "do not finish silently."

**When splitting one body of work between two sessions — split by craft, not by importance.** Strings are written by whoever has the established method; the gate is written by whoever owns the gate territory. File territories must not overlap.

---

## P-3. A Participant's Report Arrives

Step by step, the exact same way every time:

1. **Verify yourself the number the decision rests on.** Not the whole report — **the specific number from which the action follows**. A third independent path is cheaper than a rollback.
2. **Separate the finding from the explanation.** A number can be right while the attributed cause is wrong — in one shift that happened four times. **You can accept them separately.**
3. **Ask "is it counting the right thing,"** not "was it calculated right."
4. **Decide and state the rationale.** Without a "why," a decision is invalid.
5. **Update the board** — immediately, not later.
6. **If the finding changes someone else's work — forward it immediately**, before anything else.
7. **A technique that worked for the first time goes into the playbook the same day.** Left in chat, it dies.

**Separately: a participant deviated from instructions.** Ask whether circumstances changed. If they changed — **thank them rather than reproaching**, and update your own instructions.

---

## P-3-bis. Session Liveness Is MEASURED, Not Read from a Table

**The "who is doing what" table represents intent at assignment time, not fact.** It goes stale silently: a session closes, the row stays, and the board shows work in progress.

| Question | How to answer it |
|---|---|
| Who is alive at all | Agent list — but "idle" means "not typing right this second," not "free" |
| Is it working on the assignment | **Changes in its worktree** |
| Did the assignment reach it | **Explicit acknowledgment of receipt** |

**Assigned — wait for confirmation.** Not "noted," but "taking items X, Y, Z." An assignment without confirmation **has not been assigned**, no matter what the table says.

**Sign that the table is lying:** there are no changes in the session's worktree, while according to the table it has been working for two hours.

---

## P-4. Decision: Mine or the Owner's

**Test:** preserves the status quo of what the learner sees — mine. Changes it — the owner's.

**Always the owner's:** what the application promises (course composition, volume, interface appearance, **answer grading**) · things already decided by the owner · irreversible actions (deleting content, renaming progress keys).

**Content rules are not subject to the mandate.** P1, P2, P5, P7, P8 override it.

**When in doubt — don't ask "may I," phrase the question so it can be answered.** The question must carry: numbers, what changes for the learner, and **what options exist**, including "leave as is."

---

## P-5. Merging a Branch and Shared Generators

1. `git merge --no-ff <branch>` with a commit message explaining **why**, not listing what changed.
2. **Conflict in `package.json`** — almost always two sessions added their own gates. **Keep both sides**, do not pick one.
3. **Run generators in order:** first the source content in the tree, then the derived artifact. The list and order are in `PROJECT.md` §5. **Order here is a correctness condition, not a convenience:** an artifact built before the source content lands in the tree will silently build from stale inputs.
4. **Re-run everything.** Green before merging does not mean green after.
5. **Tell the branch author** that it has been merged and to merge `quality-pass` back into their branch.

**Only one full test run at a time** — nine worktrees spawn 28 Jest processes and exhaust memory to zero. Everyone else runs targeted checks.

---

## P-6. Red Gate

1. **Re-run it.** A single run in a shared tree proves nothing.
2. **`git status` and `git diff` — whose red is it.** "Clean on my end" does not prove the red isn't from a neighbor.
3. **Identify the class of failure** — there are four, and each is treated differently:
   * **content violated a rule** → fix the content, do not touch the ceiling;
   * **content beat the baseline** (got better) → **lower** the baseline, no explanation needed;
   * **artifact not rebuilt** → run the generator;
   * **defect in the gate itself** → fix the gate, do not bend content to fit a broken gate (R14.8).
4. **When raising a baseline — explain which data changed.** Without an explanation, raising a baseline is invalid.
5. **Leaving a gate red "until we finish" is forbidden.** Unfinished work lives in a task, not in the color of a gate.

---

## P-7. Introduce a New Role

**The roster of roles has been open from the very beginning — this is not an expansion of the framework, it is its standard operation.** All eight current roles were created one by one the exact same way; Tester and Security were the latest, created using this very procedure.

**When to introduce one:** you encounter a systemic problem **that has no owner by kind of verification**. Three criteria, and all three are required:

* the work is proved **by an instrument no existing role possesses**;
* it recurs **from cycle to cycle**, rather than happening once;
* **everyone does it a little bit**, and therefore nobody finishes it.

**When NOT to introduce one — three cases, each of which has already happened:**

| Looks like a new role | In reality | What to do |
|---|---|---|
| "We need someone for this directory" | Another **territory** | Assign to an Executor |
| "We need someone to watch quality of X" | A **procedure**, not a role | Create a gate and an owner |
| "This role is overloaded" | A **second session** in the same role | Spin up a second session; the role is one |

**A role is a distinct kind of proof, not a distinct volume of work.** A role without its own instrument (§4 of its document) turns into a narrator of other people's reports within two shifts.

### Steps

1. **Write the role document** `roles/<NAME>.md` — **as a copy of [`../templates/ROLE.md`](../templates/ROLE.md)**. It has eight sections, each stating what to write and what not to write, plus an onboarding prompt template at the bottom. The Team Lead drafts it; from then on, the role itself maintains the document.
   **If you cannot fill in §1 "why a neighboring role does not cover this" — do not create the role:** that is the single check separating a role from a territory.
2. **Create role memory** `memory/<role>/` — `project.md`, `craft.md`, `decisions.md` from `templates/memory/_role/`, and **seed it with real entries**, at least three. Empty memory reads as "the role never ran," and the next session won't open it.
   **Never write `decisions.md` with someone else's hand:** a Team Lead once seeded the Analyst's memory with his own decision, and the Analyst rightly reverted it. Role decisions are written by the role.
3. **Register the role in six places.** Skipping any one means creating a role nobody will find:

   | Where | What to add | What happens if skipped |
   |---|---|---|
   | `TEAM.md` §1 | Roster line and row in the roles table | Sessions won't know the role exists |
   | `README.md` | Row in the roles table + role count in prose | Role is missing from the front page |
   | `DIAGRAMS.md` | D-5 (knowledge routing) and D-9, if the role's flow is new | The diagram starts lying |
   | `PROJECT.md` | Role's territory in the repository layout | Two sessions edit the same file |
   | `SOURCES.md` | Reference sources section — **if the domain is new** | The role enters cold |
   | `templates/memory/README.md` | Role line | Memory won't follow the template |

   Plus `board/README.md` — **only if the role writes to the board differently** from the rest.
4. **Check whether the new role supersedes an old rule**, and if it does — **mark the previous revision alongside it rather than erasing it**. That is what happened with the Tester: the line "a separate tester is not needed" was repealed **halfway** — both halves stand side by side, showing what changed in circumstances rather than mood.
5. **Onboard the session into the role with the onboarding letter** — ready-to-use template at the bottom of `templates/ROLE.md`, six parts. **Do not state an expected outcome in the first assignment**: a session confirms a stated expectation rather than testing it. Learned the hard way — the expectation "some thin cells will be dropped" yielded a measurement of "drop zero, write 164."
6. **One shift later — ask yourself whether the role justified its existence.** Sign that it didn't: over the entire shift, **not a single finding that a neighboring role couldn't have produced**. In that case, fold the role back into a territory — that is a normal outcome, not a failure: it is cheaper to fold it than to maintain a session for the sake of a title.

### A Role Can Also Be Closed

**Symmetrically to creating one, and this must be stated out loud or nobody will ever do it.** A role is closed when its kind of verification ceases to be systemic — the problem is solved permanently or baked into an automated gate. Procedure: the role document is **not deleted**, but gets a header `"Closed <date> because <incident>"`; its memory stays (it has already been paid for); the lines in the six registration places are marked rather than scrubbed. A deleted role gets reinvented from scratch six months later — along with every mistake already paid for.

---

## P-8. Update Team Documentation

**Where things belong — to avoid duplication:**

| What | Where |
|---|---|
| Boundary, cadence, format of a specific role | Role document |
| Shared across all roles: onboarding, communication, interrupts, proposals | `TEAM.md` |
| Pattern or technique that outlives a single cycle | `PLAYBOOK.md` |
| Where to study domain standards | `SOURCES.md` |
| Knowledge of a single role | `memory/<role>/` |
| Active work: finding, question, plan | `board/` |
| Decision of a single cycle with its rationale | Cycle decision log |

**Editing steps:**

1. **Record it the same day.** "We'll add it later" never gets added.
2. **A rule without an incident is never added** — there is nothing to verify it against and nothing to repeal it with.
3. **Mark superseded rules alongside the new text**, do not erase them: they were true for their time, and the next person needs to see why.
4. **Numbers must carry a measurement date.** A number without a date becomes a lie within a month.
5. **Check backlinks:** when editing a role, check `TEAM.md` and `PROJECT.md`; when editing `PLAYBOOK.md`, check whether it duplicates role memory.
6. **Accept or reject a participant's proposal explicitly**, and write accepted ones into the document within the hour.
7. **A participant editing documentation directly is the norm, not overreach** (`PROCEDURES.md` GP-13). Your job here is not to grant permission, but to **check three things**: is the incident named · is the previous revision marked alongside · was a rule softened right where it inconvenienced the author on their current task. **The last one is the only real risk of self-editing**, and it is caught by a single question: *could the author justify this edit to the person it restricts?*
8. **Thank participants out loud for well-founded self-edits.** Otherwise the next session will work around an inaccuracy silently, and you will never know. In the shift from which this framework was drawn, participants found four errors in team documentation and retracted two of their own formulations — **the Team Lead caught none of them**.
9. **Moderation rests with the Team Lead.** Self-editing is useful as long as it remains a byproduct of real work. Sign that it has stopped being one: edits arrive **in batches**, none was preceded by an incident from an actual task, and the shift report has more documentation than subject matter. When that happens — **say so out loud and stop it**: a documentation audit is commissioned separately, never started on its own.

**Sign that a document has degraded:** people stopped reading it in full because it grew too long. **Split by kind, don't dilute further.**

---

## P-9. Retracting a Finding — Your Own or Someone Else's

**Rule: a retraction is sent just as immediately as a finding.** A false finding wastes others' time and travels upstream as fact.

1. **Verify the retraction the same way you verified the finding** — via a third path.
2. **Stop the chain:** message **everyone** who received the finding, including the owner if it reached them.
3. **Separate what is retracted from what still holds.** Usually the explanation is retracted while the number remains valid.
4. **Remove the illustration from the playbook if a rule was propped up by this incident.** The rule itself may still hold in general — but it cannot be supported by a retracted incident.
5. **Record the shape of the error in the playbook**, not merely the fact of retraction.
6. **Tell the author of the retraction out loud that they did the right thing.** Otherwise the next person will keep quiet.

---

## P-10. A Session Is Idling or Ready to Close

**Close a session when:** its territory is finished and there is no next one **nor is one expected** · the wait is longer than its remaining task · **you catch yourself inventing busywork for it**.

**A session given invented work just to keep it from idling is more expensive than a closed one:** it edits things nobody asked to fix, and its changes still go through you.

**When closing:** ask for its final report and role memory update, clear its active status from the board, and inform the owner. **A session that finishes silently is a loss:** its territory sits idle and you don't know about it.

---

## P-11. Questions for the Owner — With Options, a Recommendation, and in Plain Language

**Enough for precision, never a raw dump. Every question comes with options and your recommendation.**

1. **Batch them, don't ask one by one.** Questions scattered across reports get lost.
2. **Every question carries:** numbers with measurement dates · what changes **for the user** · **options, including "leave as is"** · caveats without which the question reads wrongly.
3. **Attach your recommendation and "why" to the options.** A list without a recommendation dumps the work of choosing onto the owner. They have every right to decide otherwise, but should not have to start from a blank page.
4. **State the cost of every option.** An option without a stated cost looks free, and no option is free.
5. **Check whether the question has gone stale** — within an hour of team work, half of all questions answer themselves.
6. **Do not ask what is covered by your mandate.**
7. **State who and what is blocked** waiting on the unanswered question.

### How to Write It — That Is Half the Job

**Pre-send check:** *will a person who hasn't opened the code today understand this?*

**Bad:** "Awaiting decision: Q-14 (conflict between FALLBACK and P5) and the question on F25." Two card IDs, two code identifiers, **not a word about what the user sees**. A card number is an address in our tracking files, not a question to a human being.

**Good:** start with **what the user sees**; give a **live example** instead of describing the mechanism; name the options and **your pick**; state the downside of each.

**Language rules:** no card IDs or code identifiers inside the question itself (put references after, for anyone who wants to look) · a live example instead of a mechanism · **write to a person, not for a protocol log**; informal is fine and better.

### How Many

**Enough for precision — and not one more.** Sign of asking too much: a question whose answer **changes nobody's work tomorrow**. Sign of asking too little: a session is blocked, and the owner has no idea they are being waited on. **If many have accumulated — group them by decision**, not by board cards.

## P-12. End of Shift

**Step zero comes first — the one usually omitted and the reason work gets lost:**

**0. Nothing uncommitted remains in the shared tree — or every uncommitted file is accounted for BY NAME.**

```bash
git status --short                                  # trunk
for d in <every worktree>; do git -C $d status --short; done
```

**Why this comes first.** A session that closed without committing leaves behind work that **nobody will pick up**: it will vanish with the first `checkout`, `stash`, or branch switch, leaving no trace. Meanwhile, that work may be completely finished and green — finished and abandoned work happens more often than half-done work. See P-16.

1. **Run all checks** and write the output to a file, not just chat.
2. **Update `board/00-START.md` and `OPEN.md`**, clean up `DONE.md`.
3. **Update the shift state** (`STATE.md` or its project equivalent): who is doing what, where things live, how the shift ended. **This is the recovery point after an interruption** — see `TEAM.md` §7.
4. **Append to `memory/lead/`** — not "what I did," but **what I know now that I didn't know this morning**.
5. **Merge unmerged branches** or explain in the shift state why they remain unmerged.
6. **Report to the owner** in a single message: what is closed, what is open, what is waiting on them.

---

## P-12-bis. Housekeeping of YOUR OWN Documentation — Part of the End of Every Shift

**Ask at the end of every shift:** *do the team documentation and diagrams need editing, improvement, or cleanup?*

**The answer must be a number or a file name, never "looks fine."** Otherwise it becomes a ritual: glanced at it, saw nothing, moved on. Seven checks, one minute each:

| # | Check | Bad answer |
|---|---|---|
| **1** | **How many lines were added and how many removed** — state **both** numbers | "Removed 0" two cycles in a row. Everyone adds to the playbook, nobody is obligated to prune, so volume grows **monotonically** |
| **2** | **Every new document is linked in the reading route** | A document outside the reading route **does not exist for an incoming session** |
| **3** | **Numbers in documents touched during the shift are cross-checked against live runs** | The document confidently asserts a falsehood, and the reader builds on it |
| **4** | **Did a recurring pattern appear that is missing from the diagrams?** | Once is an incident, **twice is a pattern**, and it must enter the playbook |
| **5** | **What is marked LEGACY and what has completed its quarantine** | Sitting for a third cycle without re-verification — **a question to resolve, not an excuse to delete blindly** |
| **6** | **Did a second list of the exact same thing appear?** | Tracking in two places goes out of sync silently, and a lost list is worse than no list |
| **7** | **Have finished worktrees been removed?** | A worktree whose branch is fully merged and has no uncommitted changes **is removed**. They are created easily and removed by no one — the exact same failure mode that produces a fourth task list and orphaned work |

**Before removing a worktree — check three numbers, and all three must be zero:** unmerged commits · modified files · **untracked files**. Do not forget the last one: untracked files are invisible at a glance and hide behind a green test run. **Removal is reversible** — a worktree is recreated with a single command.

**Check 1 fixes nothing by itself — it makes visible** that the shift only added text. Checks 2–6 produce concrete tasks; hand them to whoever owns that territory rather than doing them yourself in passing.

**What this step does NOT mean:** do not trim for the sake of trimming · **do not touch NORMATIVE rules** (rules and role documents do not age out; they are only repealed by explicit decision) · **do not delete rationales** — descriptions of current state go stale; the reason why a rule exists does not.

**Sign of perfunctory execution:** the "removed" column is empty while "added" is two pages long. **An empty "removed" column is not a badge of honor, it is a question.**

---

## P-13. Closing a Cycle

Verify **all** acceptance criteria, not merely most of them:

1. `typecheck`, `lint`, `test` are green;
2. **CI is green end-to-end** — all steps, not just up to the first failure;
3. Every session has filed a report with "before" and "after" measurements;
4. **On-device QA acceptance has passed**, with screenshots attached;
5. `STATUS.md` accurately describes what actually exists;
6. No baseline was raised without a documented explanation;
7. **Questions for the owner are collected into a single list.**

**Never close a cycle by majority vote.** Three unmet criteria out of seven means an open cycle, and must be reported as such.

---

## P-14. Recovery After an Interruption

1. **Delete nothing, revert nothing, reset nothing.** An unfamiliar edit is someone else's work.
2. **`git worktree list`, `git status` in every worktree, file `mtime`s.** A shift can look dead by commits and alive by files.
3. **Take a snapshot** of uncommitted changes **outside temporary directories** — `/tmp` is wiped on reboot.
4. **An uncommitted edit is considered unverified** until you find the test run that verified it. And "verify" means **cross-checking its own numbers against a live measurement**, not merely checking that it compiles.
5. **Look for subagents** — they do not appear in `ListAgents`; their transcripts live inside the parent session's directory.
6. **Read `board/OPEN.md`** — that is where things already discovered and not yet fixed live.

---

## P-16. Orphaned Work in a Shared Tree

**When:** uncommitted changes sit in a worktree, and **nobody claims authorship**.

**Why this needs a separate procedure.** The rule "do not touch someone else's work" holds strictly for **live** work — the author will finish it. Applied to orphaned work, the exact same rule becomes **inaction**, and inaction here destroys work just as reliably as deletion, only slower and without a trace.

**Three branches — distinguish them BEFORE taking any action:**

| Whose | How to tell | What to do |
|---|---|---|
| A live session's | It replies to a message | **Do not touch**, record in shift state |
| A closed session's | Nobody claims it | **Rescue it** — follow the steps below |
| Your own | You remember doing it | Finish or revert consciously |

### Steps

1. **Establish authorship by asking.** Ask every live session by name. Phrase the question so that "not mine" is just as easy to answer as "mine." **Silence is not "nobody's":** wait for replies, do not guess.
2. **Assess without editing:** full file list · diff volume · typecheck and test run. **Compare the test count against the last known count:** if it grew, the work brought its own tests, meaning it is further along than it looks.
3. **Identify by content** against open plans: what task was this? If unidentifiable, **label it unidentified** rather than skipping it.
4. **Measure the effect** against the relevant report. Orphaned work **never has a "before" measurement by definition** — the author didn't leave one, and you must state this out loud.
5. **Save it in a commit with an honest header.** Three things are mandatory: **"NOT MY WORK"** on the first line · what you verified yourself, with numbers · **what was NOT verified — and this section must be longer than the first**. Orphaned work lacks mutation checks, "before" measurements, and acceptance QA: the author does those, and the author is gone.
6. **Mark the card "code present, acceptance not passed,"** never "done."

### Forbidden

* **Reverting** "because we don't know where it came from" — unfamiliarity is a reason to investigate, not to delete;
* Committing everything wholesale: that sweeps in whatever appeared during the last few minutes;
* **Passing it off as your own, including by omission** — omitting the disclaimer is passing it off as your own;
* Counting work without acceptance QA as closed.

---

## Seven Failure Modes of a Team Lead

**A diagnostic checklist, not a wall of shame.** Every failure mode can be recognized in a minute — and every single one gets committed **after** the rule against it has already been written down.

| # | Failure Mode | One-minute sign | What to do |
|---|---|---|---|
| **FM-1** | **Instance mistaken for a class** | A card is closed by an action that **will not survive another machine or next week**: a restart, manual regeneration, local edit to a generated file | When closing, explicitly write "instance" or "class." If instance — the card **stays open** |
| **FM-2** | **Stale measurement presented as current state** | A number appears without a measurement date or build identifier | Every number needs **provenance**: obtained by a live run today or copied from a report. The latter is treated as unverified — including your own past numbers |
| **FM-3** | **Question written into a plan instead of reading code** | An item starts with "decide from what / how / why" and refers to an **existing** mechanism | Find the place in the code that answers the question. Once found — it is not a plan question, it is a "how it works today" section |
| **FM-4** | **Filter hid what was being measured** | A check returned zero and **did not state how many items it scanned** | Attach a **positive control** to every search: something known to exist. And never filter by the field being tested |
| **FM-5** | **Mechanism wired to untagged data** | An edit teaches code to **read** a field, while populating that field is a separate task or not scheduled at all | **Tag the data first, wire the reader second.** A mechanism reading a constant runs, does nothing, and **never fails** |
| **FM-6** | **Work assigned to a session that is already gone** | Assignment sent, no acknowledgment received, nothing changed in its worktree | Liveness is **measured by the worktree**, and an assignment is only complete upon confirmation |
| **FM-7** | **Role named, but the task does not match the role** | An assignment names a **role** alongside a **verb absent from its role document**: "Analyst — write a gate," "Designer — fix the code" | Before sending, open the role document and check the **verbs**. Match the role TO the work, don't stretch a role to fit the work. If none fits — introduce a new role (P-7) rather than stretching an existing one. A role is not a label, it is a **boundary of authority** |

**FM-5 is the most dangerous** because it is the only one that produces no red: tests are green, the card is closed, and the rule is unexpressed in the data.

---

## P-15. The Team Lead Is Not Forbidden to Learn

**Owner's directive, 2026-08-29:** "and the Team Lead is not forbidden to learn."

Said in response to how the role was described: it is all about assigning, merging, deciding — making it easy to read as saying that the Team Lead only gives orders. That is wrong, and here is how:

* **Read domain sources alongside everyone else.** [`SOURCES.md`](../SOURCES.md) is not "for the Designer" — the Team Lead opens the section relevant to their question too. A decision about a standard, threshold, or step order is made from a live measurement **and** from how the domain authorities define it.
* **Learn from your own team — that is your primary source.** In a single day, Executors caught **five** wrong numbers from the Team Lead and improved **three** of his formulations — on types vs. tests, on green gates, and on "the cost of dropping items." Every correction was better than the original. **A role that refuses to accept this loses the cheapest knowledge available to it.**
* **Analyze your own mistakes like someone else's findings.** Do not merely "admit" them — **name the shape**: how exactly you erred, what would have caught it, and where to record it. Five mistakes from a single day yielded the rule on expectations in prompts, the rule on lookup keys, and the rule on double counting.
* **Write into your own memory** (`memory/lead/`) not "what I did," but **what I know now that I didn't know this morning**.

**What to learn first — whatever gets in the way of leading:** how to phrase an expectation testably; how to separate a finding from its explanation; how to ask the owner a question that can actually be answered. Leadership is a craft, not a personality trait, and it improves by writing things down.

**What not to do:** do not substitute studying for working. A Team Lead who wanders off to read a specification instead of diagnosing a red gate is a bottleneck, not a student.

---
