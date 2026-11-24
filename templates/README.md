# Templates — What Gets Deployed into a Project

**Nothing generated from these templates ever comes back into the framework repository.**
Filled-out files live in the **project** repository and are excluded in [`../.gitignore`](../.gitignore).

| Template | Deploys To | Mode | Purpose |
| --- | --- | --- | --- |
| [`AGENTS.md`](./AGENTS.md) | `<project>/AGENTS.md` | **Lite** (or both) | Compact single-page engineering & verification rules for 1 agent + 1 reviewer |
| [`PROJECT.md`](./PROJECT.md) | `<project>/PROJECT.md` | **Lite & Full** | Single source of project specifics: profile, layout, commands, domain rules, generators, baselines, testbench |
| [`board/`](./board/) | `<project>/board/` | **Full** | Team board: `00-START.md` (entry summary), `OPEN.md` (truth), `DONE.md` (pruned log) |
| [`memory/_role/`](./memory/_role/) | `<project>/memory/<role>/` | **Full** | One directory per active role: `project.md`, `craft.md`, `decisions.md` |
| [`ROLE.md`](./ROLE.md) | **`../roles/<NAME>.md`** *(stays in framework repo)* | **Full** | Template + onboarding prompt for establishing a new reusable role via procedure **P-7** |

**`ROLE.md` is the only template deployed into this framework repository rather than into a target project.** Once a role is proven on one project, it becomes part of the reusable charter.

**Deployed by:** Developer directly (Lite Mode) or Team Lead via procedure **P-0** ([`../roles/LEAD-PROCEDURES.md`](../roles/LEAD-PROCEDURES.md)) in Full Fleet Mode.

**Universal rule for filling out templates:** an empty section is better than an invented one — empty honestly says "not yet determined," whereas fabricated boilerplate misleads every incoming session.
