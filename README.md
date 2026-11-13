# AI-Team — Operating Charter for a Team of Parallel AI Sessions

Multiple sessions work on the same repository simultaneously. One of them is the **Team Lead**: it
plans, assigns work, merges the results, and communicates with the owner. The others hold their
roles and territories.

This is neither a library nor a tool. **This is an operating charter**: roles, procedures, diagrams,
and rules for maintaining memory and the task board. Everything written here is derived from real
operational incidents — every rule names the actual case it came from.

---

## Why This Exists

Parallel sessions break predictably, and always in the same ways:

* **work is lost on disconnect** — a session dies, and nobody knows what it managed to finish;
* **two sessions edit the same file** and only find out at merge time;
* **a finding never reaches** the owner of that territory;
* **knowledge dies with the session** — a technique discovered in the morning is forgotten by evening;
* **a new session starts cold** and pays again for lessons already bought;
* **checks are green while a defect exists** — and nobody asks what those checks actually prove.

The charter answers each of these points not with a wish, but with a procedure.

---

## Getting Started

### 1. Project Owner

Clone this repository **next to** your project or inside it — whichever is more convenient:

```
git clone <url> ai-team
```

Then spin up a single session and tell it verbatim:

> **"Review the team workflow documentation in `ai-team/` and deploy it for this project."**

That session becomes the Team Lead and executes procedure **P-0** — deployment
([`roles/LEAD-PROCEDURES.md`](./roles/LEAD-PROCEDURES.md)). It will create the working directories for
the project, populate [`templates/PROJECT.md`](./templates/PROJECT.md) with your rules, gates, and
territories, and tell you **how many sessions to spin up** and for which roles.

### 2. Any Team Member

Reading order — [`TEAM.md`](./TEAM.md) §2. In short: first the board (whether there is open work),
then [`PLAYBOOK.md`](./PLAYBOOK.md) (how we think here), then your own role document.

---

## What Lives Here

| | What it covers |
|---|---|
| [`TEAM.md`](./TEAM.md) | team roster, onboarding, communication rules, crash recovery |
| [`PROCEDURES.md`](./PROCEDURES.md) | thirteen general procedures: onboarding, measurement, finding, commit, report, retraction, pause, and editing the documentation itself |
| [`PLAYBOOK.md`](./PLAYBOOK.md) | defect shapes, how our own probes fail, and navigating external sources |
| [`DIAGRAMS.md`](./DIAGRAMS.md) | diagrams: flows, decision trees, knowledge routing, document relationships |
| [`SOURCES.md`](./SOURCES.md) | where to study by discipline; project-specific sources are maintained separately |
| [`CROSS-TOOL-CHAT.md`](./CROSS-TOOL-CHAT.md) | **communication across sessions of different tools** via files: messages live OUTSIDE the repository, or the channel does not work |
| [`roles/`](./roles/) | **ten** role documents, including [`JUNIOR.md`](./roles/JUNIOR.md) — applying decided changes and completeness of application |
| [`roles/EXECUTION-PROFILE.md`](./roles/EXECUTION-PROFILE.md) | **which model and reasoning effort** to use for a task: profile by outcome, not by diff size |
| [`templates/`](./templates/) | `PROJECT.md`, board, memory — plus `ROLE.md`, the template for a new role document |

---

## Roles

Eight core roles, **and the list is open** — a new role is created when a recurring systemic problem
has no owner by verification type (procedure P-7).

| Role | What it does | Edits code? |
|---|---|---|
| **Team Lead** | holds the entire run together, takes no tasks for itself | own and shared files |
| **Executor** | owns its territory end-to-end, measures "before" and "after" | yes |
| **Device / Runtime QA** | taps the live application: mechanics, migration, devices | no |
| **Tester** | owns the verification system: what checks prove and where they stay silent | verification layer only |
| **Analyst** | re-verifies others' work **by what is being measured**, not just arithmetic accuracy | **no** |
| **Designer** | everything the user sees: screens, flows, behavior, color | **no** |
| **Architect** | layers, boundaries, dependencies, contracts, data ownership | **no** |
| **Security** | user data, privacy promise, release supply chain | **no** |

Roles that **do not edit code** are not decorative. Editing destroys the object of analysis: whoever
fixes something is thereafter verifying their own work.

### Creating a Ninth Role Takes One Procedure

**The list is open in practice, not just in principle.** All eight roles were created along the same
path — the last two directly via this procedure — and it is specified end-to-end:

* **when to create one** — a kind of verification with no owner, recurring across runs, with its own
  instrument of proof. Plus **three cases where it looks like you need a role, but you actually need
  something else**: an Executor territory, a procedure, or simply a second session in an existing role;
* **what to write** — [`templates/ROLE.md`](./templates/ROLE.md): eight sections, each stating what
  to include, what to leave out, and which incident produced that requirement. Plus a ready-made
  **session onboarding prompt** — six parts, copied directly into the first message of a new session;
* **where to register it** — six places, otherwise the role exists on paper but nobody will find it;
  the checklist is in the procedure;
* **how to retire it** — a role folds back into a territory if over an entire shift it produced no
  finding that a neighboring role would not have caught. The document is not deleted, only marked
  retired: a deleted role gets reinvented six months later along with all the mistakes already paid for.

Procedure **P-7** — [`roles/LEAD-PROCEDURES.md`](./roles/LEAD-PROCEDURES.md).

---

## Three Ideas Worth Reading This For

**1. Ask not "are the numbers right," but "are they counting the right thing."** A working session
proves that it completed its assigned task; it has no vantage point from which to doubt the object
of measurement it chose for itself. That is why the Analyst role exists.

**2. "What shape would a defect have to take for this rule not to see it?"** Not "where else," but
"how could this still be wrong." In the shift from which this charter grew, three sessions arrived
at this question independently, and it produced every major finding.

**3. Retracting your own finding is real work, not an admission of failure.** In a single shift,
authors retracted five of their own findings before anyone started fixing them. One of those
retractions stopped a change that would have shipped broken content back to the user.

---

## What Does NOT Belong in This Repository

**Everything project-specific.** The task board, role memories, reports, plans, `PROJECT.md` — these
are created during deployment and live in the **project's** context, never committed here. See
[`.gitignore`](./.gitignore) and [`templates/README.md`](./templates/README.md).

### What NEVER Enters This Repository — and Why It Matters

**The charter is portable:** it is taken into another project, to another owner, with another team.
Therefore everything tied to a **specific** project must remain outside tracked files.

| Never committed | Why |
|---|---|
| **`memory/`** — role memory | experience of **this** team on **this** project |
| **`board/`** — task board | findings and open work of the specific project |
| **`PROJECT.md`, `STATE.md`** | state of a specific run |
| **reports, handoffs, implementation plans** | tied to specific numbers and dates |
| **`_chat/`** — session messages | project correspondence, and **outside the repository by design** |
| **screenshots, logs, diagnostics** | run artifacts |

**All of the above are listed in `.gitignore`.** Verify this with a command, not by eye:

```bash
git ls-files                 # everything tracked must belong strictly to the framework
```

**Project files DO LIVE RIGHT HERE — and that is normal.** The team works inside the charter's
directory: the board, memory, reports, and correspondence appear right next to the rules, and there
is nowhere else to put them.

> **There is one rule: create a project file — add it to `.gitignore` and IMMEDIATELY COMMIT that line.**

**Both halves are mandatory, and the second is more important.** A line added to `.gitignore` but
not committed protects **only you**: a neighboring session in another working tree does **not** have
that rule, and it will commit project files without suspecting they should be ignored.

Not "later," not "at the end of the shift." Within an hour, a directory looks like part of the
furniture and gets committed without a second glance — and then the charter, meant to be a clean
framework, arrives at the next project carrying someone else's board and someone else's memory.

**Sign that the rule was broken:** `git status` shows an unfamiliar directory, and nobody remembers
who created it.

**One honest caveat.** `PLAYBOOK.md` and the role documents **do contain examples from the project
where this charter grew up** — Spanish strings, German lemmas, drills. This is **intentional**: a
rule without a real incident is a platitude, and the concrete case is what makes it recognizable.
**An example is welcome; a project-specific prescription is not.** If a rule assumes your domain
rather than illustrating a pattern, that is a defect in the charter and should be fixed.

Anyone who clones this repository gets an **empty frame**: rules, roles, procedures, diagrams, and
templates — and not a single task from someone else's project.

---

## How This Charter Evolves

**Documents are updated as work happens, not written once.** Maintenance rules — `TEAM.md` §6 and §8:

* **any member fixes inaccuracies directly** if they find a verified error — a number does not
  reproduce, a file is missing, a rule contradicts its neighbor. This is normal practice, not
  overstepping: silently working around a broken rule is worse than fixing it;
* **improvements are also welcome**, provided they are verified and derived from a real case. Only
  one thing is off-limits: editing a rule that constrains **you personally** — there you are an
  interested party;
* **a member's proposal** must come with an incident or a measurement, never just "it would be more
  convenient";
* **recorded the same day** — a technique left in chat dies with the session;
* **superseded rules are marked inline** next to the previous wording, not erased: they were valid
  for their time;
* **a diagram is updated together with the text** it summarizes — every diagram states what it
  summarizes and when it was last verified.

---

## Rights and License

**Author — Oleksandr Cherednyk. All rights to this material belong to him.**

License — [MIT](./LICENSE). This is **permission to use, not a transfer of ownership**: take it,
copy it, adapt it to your needs, including in closed-source and commercial projects. There is only
one condition — **preserve the copyright notice** (`Copyright (c) 2026 Oleksandr Cherednyk`) in all
copies and substantial portions. Remove it, and your use becomes unlawful.

**What this does NOT permit:** claiming this text as your own, publishing it under another name, or
selling it as your own original work.

**Files populated for your project are yours.** The templates exist here specifically to spread into
other repositories and take on a life of their own; you owe no accounting for them to anyone.
