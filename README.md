# AI-Team — Operating Charter for AI Coding Agents

A portable, incident-driven operating framework for running AI coding agents with engineering rigor — from **a single agent + reviewer on a new project** up to **a parallel multi-session team on a mature codebase**.

This is neither a library nor a runtime tool. **It is an operating charter**: operating modes, role boundaries, verification procedures, decision trees, and templates for project context, task boards, and persistent memory. Every rule exists because of a real operational failure, not abstract theory.

---

## Why This Exists

AI coding sessions break predictably, and always in the same ways:

* **green checks hide real defects** — tests compile and pass while the probe is blind or never wired into the runner;
* **fixes break adjacent state** — a fix updates one variable and misses its sibling on the next line;
* **self-review misses the real question** — whoever edits code proves their own assumptions instead of challenging the premise;
* **two parallel sessions collide** — editing the same file in separate working trees and finding out only at merge time;
* **knowledge dies with the session** — a hard-won lesson discovered in the morning is paid for again in the evening;
* **work is lost on disconnect** — a session dies and nobody knows what finished or what was verified.

This charter answers each failure mode with concrete procedures rather than vague reminders.

---

## Two Operating Modes (Pick by Project Size)

Do **not** deploy eight parallel roles onto a brand-new repository. Match governance weight to project complexity:

| Mode | When to Use | What You Deploy | Reading Overhead |
| --- | --- | --- | --- |
| **Lite Mode** *(1 Agent + 1 Reviewer)* | Greenfield projects, MVPs, small codebases before strict CI & tests exist | `templates/AGENTS.md` + `templates/PROJECT.md` | ~1–2 pages |
| **Full Fleet Mode** *(Parallel Sessions)* | Mature codebases, multi-surface audits, parallel workstreams with automated gates | `PROJECT.md` + `board/` + `memory/` + selected `roles/*.md` + `CROSS-TOOL-CHAT.md` | Role-scoped |

### Quick Start — Lite Mode (Recommended for New Projects)
1. Copy [`templates/AGENTS.md`](./templates/AGENTS.md) into your project root as `AGENTS.md` and adapt the verification commands.
2. Copy [`templates/PROJECT.md`](./templates/PROJECT.md) into your project (e.g., `docs/PROJECT.md` or `PROJECT.md`) and fill in only verified facts.
3. Run one primary implementation agent and use a second session (or separate CLI tool) strictly in **read-only review mode** on diffs.

### Quick Start — Full Fleet Mode (Parallel Sessions)
Clone this repository next to (or inside) your project:
```bash
git clone <url> ai-team
```
Start one session and tell it verbatim:
> **"Review the team workflow documentation in `ai-team/` and deploy it for this project."**

That session becomes the **Team Lead** and runs procedure **P-0** ([`roles/LEAD-PROCEDURES.md`](./roles/LEAD-PROCEDURES.md)): deploying templates into the project context, populating `PROJECT.md` from live measurements, and recommending the exact number of parallel sessions and roles needed.

---

## Repository Map

| Path | Purpose | Read When |
| --- | --- | --- |
| [`TEAM.md`](./TEAM.md) | Operating modes, role roster, onboarding order, collaboration & recovery rules | Onboarding (Full Mode) |
| [`PROCEDURES.md`](./PROCEDURES.md) | 13 shared step-by-step procedures (`GP-1`..`GP-13`): measuring, findings, commits, reports, retractions, pauses | On demand |
| [`PLAYBOOK.md`](./PLAYBOOK.md) | Recurring defect shapes, how verification probes lie, high-ROI verification techniques | Once on onboarding |
| [`DIAGRAMS.md`](./DIAGRAMS.md) | Authoritative ASCII decision trees & flows (`D-1`..`D-10`) | Visual reference |
| [`SOURCES.md`](./SOURCES.md) | Primary external technical references by discipline (Web, Backend/Security, Mobile, GameDev/3D) | On demand |
| [`CROSS-TOOL-CHAT.md`](./CROSS-TOOL-CHAT.md) | File-based messaging across different AI tools & `git worktree`s | Multi-tool parallel runs |
| [`roles/`](./roles/) | 8 core roles + `JUNIOR.md` + `LEAD-PROCEDURES.md` + `EXECUTION-PROFILE.md` (11 files) | Your assigned role only |
| [`templates/`](./templates/) | `AGENTS.md` (Lite), `PROJECT.md`, `board/`, `memory/`, and `ROLE.md` | Project bootstrap (`P-0`) |
| [`skills/code-audit/SKILL.md`](./skills/code-audit/SKILL.md) | Distilled multi-stack bootstrap & read-only audit skill for AI coding agents | Agent skill install |

---

## Roles (Full Fleet Mode)

Eight core roles plus **Junior** (completeness specialist). The roster is **open** — a new role is introduced via procedure **P-7** only when a recurring systemic verification gap has no owner. Not every role is spun up on every cycle.

| Role | Responsibility | Edits Code? |
| --- | --- | --- |
| **Team Lead** | Plans, assigns territories, verifies critical numbers, merges, communicates with owner | Shared/merge ops only |
| **Executor** | Owns a non-overlapping slice end-to-end with "before" and "after" measurements | Yes |
| **Junior** | Applies settled decisions exhaustively (`N of N` completeness); never makes judgment calls | Yes (settled specs only) |
| **Runtime / Device QA** | Exercises the live product (browser, emulator, desktop window) end-to-end | No |
| **Tester** | Owns the verification layer: proves via mutation what tests catch and where they are blind | Verification layer only |
| **Analyst** | Challenges measurement validity ("are we counting the right thing?") and inspects module seams | **No** |
| **Designer** | Evaluates live UX, states, flows, contrast, accessibility, and motion | **No** |
| **Architect** | Audits layer boundaries, dependency direction, contracts, data ownership, and bundle/query cost | **No** |
| **Security** | Enforces zero-leaks, threat modeling, AuthN/AuthZ (IDOR/RLS), and release artifact hygiene | **No** |

> **Why read-only roles never edit code:** editing destroys the independence of analysis — whoever fixes a defect immediately starts verifying their own assumptions.

---

## Three Core Ideas Behind This Charter

1. **Ask not "are the numbers accurate?" — ask "are they counting the right thing?"** A working session proves it finished its task; it has no vantage point from which to doubt the metric it chose for itself.
2. **"What shape would a defect have to take for this rule not to see it?"** Not "where else?", but "how could this still be wrong while every check stays green?"
3. **Retracting your own finding is real work, not failure.** A false finding wastes teammates' hours and propagates upward as fact; a timely retraction protects the whole run.

---

## What NEVER Belongs in This Repository

This charter is portable across projects and owners. **Everything tied to a specific project stays outside tracked files** (enforced via [`.gitignore`](./.gitignore)):

| Excluded from Framework Repo | Why |
| --- | --- |
| `board/` | Open findings and work items of a specific project |
| `memory/` | Accumulated experience from a specific project |
| `PROJECT.md`, `STATE.md`, `AGENTS.md` | Project passport, live run state, project agent rules |
| `reports/`, `*-report.md`, `*-handoff.md` | Run-specific numbers and dates |
| `_chat/`, `chat/` | Ephemeral cross-session messages (live outside git worktrees by design) |
| `shots/`, `logs/`, `diagnostics/` | Run artifacts |

> **Rule:** Whenever you create a new project-specific directory next to the framework, add it to `.gitignore` and **commit that `.gitignore` line immediately**, before neighboring worktrees accidentally commit project artifacts.

Verify cleanly with:
```bash
git ls-files   # Every tracked file must belong strictly to the generic framework
```

> **Note on Examples:** `PLAYBOOK.md` and select procedures preserve anonymized case examples from real operational incidents (without private repository names or domain-locked rules). A rule without a concrete incident becomes a platitude; an example illustrates a pattern, whereas a project-specific prescription is a defect in the charter.

---

## Rights and License

**Author — Oleksandr Cherednyk.** All rights to this material belong to the author.

Licensed under [MIT](./LICENSE). You may use, copy, and adapt this framework in open-source or commercial projects provided you **preserve the copyright notice** (`Copyright (c) 2026 Oleksandr Cherednyk`) in all copies and substantial portions. Project files populated from `templates/` belong entirely to their respective projects.
