# Templates — What Gets Deployed into a Project

**Nothing generated from these templates ever comes back into the governance repository.**
Filled-out files live in the **project** repository and are excluded in [`../.gitignore`](../.gitignore).

| Template | Deploys To | Purpose |
|---|---|---|
| `PROJECT.md` | `<project>/PROJECT.md` | Single source of project specifics: layout, checks, content rules, generators, baselines, testbench, branches |
| `board/` | `<project>/board/` | Team board: `00-START.md` (is there open work?), `OPEN.md`, `DONE.md` |
| `memory/_role/` | `<project>/memory/<role>/` | One directory per active role: `project.md`, `craft.md`, `decisions.md` |
| `ROLE.md` | **`../roles/<NAME>.md`** — exception: stays in the governance repo | New role definition: eight sections specifying what belongs in each, plus the onboarding prompt for spinning up a session |

**`ROLE.md` is the only template deployed into this governance repository rather than into the project.**
A role is not a project-specific detail: once established on one project, it applies to the next. The Team Lead creates it via procedure **P-7**, which also lists the six places where a new role must be registered so the team can find it.

**Deployed by the Team Lead, procedure P-0** — [`../roles/LEAD-PROCEDURES.md`](../roles/LEAD-PROCEDURES.md).

**Why a template instead of an empty directory.** An empty file reads as "the role never worked"; a template specifies **what shape** an entry must take, which is the only thing preventing role memory from turning into a dumping ground.

**Universal rule for filling out all three:** an entry without the concrete incident from which it was derived is invalid. An empty section is better than an invented one — it honestly states "not yet determined."
