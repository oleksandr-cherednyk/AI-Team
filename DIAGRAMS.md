# Diagrams — Sequences, Decision Trees & Knowledge Flows

**Create a diagram only for branching decisions or multi-step sequences** (rosters belong in tables). Each diagram is authoritative for the flow it depicts and states what it consolidates. Whenever a source procedure changes, update its diagram in the same commit (`P-8`, `P-12-bis`).

---

## D-1. Entry & Document Map (Lite Mode vs. Full Fleet Mode)

*Consolidates: `README.md`, `TEAM.md` §1–§2.*

```text
                       New or Small Repo?
                               │
                 ┌─────────────┴─────────────┐
                 ▼ YES (Lite Mode)           ▼ NO (Full Fleet Mode)
      ┌───────────────────────┐   ┌───────────────────────────────┐
      │ 1. AGENTS.md          │   │ 1. board/00-START.md          │  Is there open work?
      │ 2. PROJECT.md         │   └───────────────┬───────────────┘
      │ 3. 1 Agent + 1        │                   ▼
      │    Read-Only Reviewer │   ┌───────────────────────────────┐
      └───────────────────────┘   │ 2. PROJECT.md                 │  Profile, commands, rules
                                  └───────────────┬───────────────┘
                                                  ▼
                                  ┌───────────────────────────────┐
                                  │ 3. PLAYBOOK.md (once)         │  Defect shapes & probe traps
                                  └───────────────┬───────────────┘
                                                  ▼
                                  ┌───────────────────────────────┐
                                  │ 4. TEAM.md                    │  Roster, comms, recovery
                                  └───────────────┬───────────────┘
                                                  ▼
                 ┌────────────────┬───────────────┴──────┬──────────────────┐
                 ▼                ▼                      ▼                  ▼
          ┌────────────┐   ┌─────────────┐       ┌──────────────┐   ┌─────────────┐
          │ roles/<me> │   │ PROCEDURES  │       │ memory/<me>/ │   │ SOURCES.md  │
          │ in full    │   │ on demand   │       │ project ·    │   │ on demand   │
          │            │   │ (GP-1..13)  │       │ craft ·      │   │             │
          └─────┬──────┘   └─────────────┘       │ decisions    │   └─────────────┘
                │ Team Lead only                 └──────────────┘
                ▼
         ┌──────────────────────┐
         │ roles/LEAD-PROCEDURES│
         │ (P-0..P-16 + FM)     │
         └──────────────────────┘
```

> **Team Lead exception (`P-1`):** Run live verification checks and worktree status **before** reading documents so stale prose never biases measurements.

---

## D-2. Lifecycle of a Finding

*Consolidates: `PROCEDURES.md` GP-3, GP-4, GP-9.*

```text
   observation
       │
       ▼
  ┌─────────────────────────┐   no    ┌──────────────────────────────┐
  │ Reachable in a live     ├────────►│ NOT A DEFECT — record why in │
  │ scenario?               │         │ report ("Verified — Clean")  │
  └───────────┬─────────────┘         └──────────────────────────────┘
              │ yes
              ▼
  ┌─────────────────────────┐   no    ┌──────────────────────────────┐
  │ Read actual records,    ├────────►│ Inspect real records first;  │
  │ not just summary count? │         │ false findings waste hours   │
  └───────────┬─────────────┘         └──────────────────────────────┘
              │ yes
              ▼
  ┌─────────────────────────┐
  │ Round # → check probe   │   0 of N / N of N → likely broken probe
  │ Huge #  → check metric  │   >70% flagged → likely wrong question
  └───────────┬─────────────┘
              ▼
  ┌─────────────────────────┐  mine   ┌──────────────────────────────┐
  │ Whose territory?        ├────────►│ Measure first → fix → prove  │
  └───────────┬─────────────┘         └──────────────┬───────────────┘
              │ someone else's                       │
              ▼                                      │
  ┌─────────────────────────────────────┐            │
  │ Ask territory owner directly first  │            │
  │ (align definitions before filing)   │            │
  └───────────┬─────────────────────────┘            │
              │ confirmed                            │
              ▼                                      │
  ┌─────────────────────────────────────┐            │
  │ Hand over: MEASUREMENT + probe      │            │
  │ blind spots (separate from theory)  │            │
  └───────────┬─────────────────────────┘            │
              ▼                                      ▼
  ┌───────────────────────────────────────────────────────────┐
  │ board/OPEN.md — 4 parts: what · proof · impact · next step│
  └───────────┬───────────────────────────────────────────────┘
              ▼
       ┌──────────────┐  changes user-     ┌────────────────────┐
       │ Who decides? ├───────────────────►│ waiting on owner   │
       └──────┬───────┘  visible promise   │ (one batched list) │
              │ preserves status quo       └────────────────────┘
              ▼
       ┌──────────────┐        ┌──────────────────────────────┐
       │ in progress  ├───────►│ done → board/DONE.md         │
       └──────┬───────┘        │ (pruned after 2 cycles)      │
              │                └──────────────────────────────┘
              │ disproven later
              ▼
       ┌──────────────────────────────────────────────────────┐
       │ RETRACTION — send immediately to all recipients.     │
       │ Separate retracted cause from valid raw data; log    │
       │ error shape in memory/<role>/decisions.md.           │
       └──────────────────────────────────────────────────────┘
```

---

## D-3. Decision Tree: Team Lead vs. Owner

*Consolidates: `roles/LEAD-PROCEDURES.md` P-4, P-11.*

```text
                        Is a decision needed?
                               │
               ┌───────────────┴───────────────┐
               ▼                               ▼
    Changes what the user               Preserves user-visible
    sees or is promised?                status quo?
               │                               │
       ┌───────┴────────┐                      ▼
       ▼                ▼              ┌────────────────────┐
      YES              NO ────────────►│ TEAM LEAD DECIDES  │
       │                               │ Record rationale — │
       ▼                               │ without "why", it  │
  ┌─────────────────┐                  │ is invalid.        │
  │ OWNER DECIDES   │                  └────────────────────┘
  └────────┬────────┘
           │                            Always Owner's (3 classes):
           ▼                            ───────────────────────────
  ┌────────────────────────────┐        1. Product promises (scope,
  │ Plain-language question:   │           visible UX, domain rules)
  │ · dated live numbers       │        2. Prior Owner decisions
  │ · what changes for user    │           (show measurement, never
  │ · options + "leave as is"  │            silently overrule)
  │ · costs + recommendation   │        3. Irreversible actions
  └────────┬───────────────────┘           (data deletion, key renames)
           ▼
    Consolidate into ONE list           Project domain rules (PROJECT.md §4)
    and ask Owner all at once           override Team Lead authority.
```

---

## D-4. Decision Tree: Red in a Verification Run

*Consolidates: `PROCEDURES.md` GP-5, `roles/LEAD-PROCEDURES.md` P-6.*

```text
                            check run is red
                                   │
                                   ▼
                        ┌──────────────────────┐
                        │ RE-RUN IT            │  Single run in shared
                        └──────────┬───────────┘  tree proves nothing
                                   ▼
                        ┌──────────────────────────────────────┐
                        │ WHOSE RED? — git status / git diff   │
                        │ or git stash & test clean HEAD       │
                        └──────────┬───────────────────────────┘
                 someone else's ◄──┴───► mine
                           │              │
                           ▼              ▼
                  hand to territory    classify failure (4 types):
                  owner
                                  ┌───────────────────────────────────────┐
                                  │ 1. Code/data violated a rule          │
                                  │    → fix code/data, keep ceiling      │
                                  ├───────────────────────────────────────┤
                                  │ 2. Code/data beat baseline (better)   │
                                  │    → lower/tighten baseline freely    │
                                  ├───────────────────────────────────────┤
                                  │ 3. Derived artifact is stale          │
                                  │    → run generator (PROJECT.md §5)    │
                                  ├───────────────────────────────────────┤
                                  │ 4. Gate itself is defective           │
                                  │    → fix the gate, never degrade      │
                                  │      product quality to appease it    │
                                  └───────────────────────────────────────┘

   Raising a baseline requires recording which underlying data changed.
   Never leave a red gate sitting "until later" — track open work on board.
```

---

## D-5. Knowledge Routing: What Lives Where

*Consolidates: `PROCEDURES.md` GP-11, GP-13, `templates/memory/README.md`.*

```text
                        I discovered something
                               │
           ┌───────────────────┼────────────────────┬─────────────────┐
           ▼                   ▼                    ▼                 ▼
    Actionable WORK?    Universal PATTERN?   Primary SOURCE?   Non-obvious
           │                   │                    │          DECISION + outcome?
           ▼                   ▼                    ▼                 ▼
   ┌──────────────┐   ┌────────────────┐   ┌──────────────┐  ┌────────────────┐
   │ board/OPEN   │   │ via Team Lead  │   │ via Team Lead│  │ memory/<me>/   │
   │ + priority   │   │ → PLAYBOOK.md  │   │ → SOURCES.md │  │ decisions.md   │
   │ + plan       │   │ (keep 1-line   │   │ (official    │  │ (outcome is    │
   └──────┬───────┘   │  pointer only) │   │  specs only) │  │  mandatory)    │
          ▼           └────────────────┘   └──────────────┘  └────────────────┘
     done → DONE.md            ▲
    (prune 2 cycles)           │
    Role-specific knowledge?───┘
          ▼
   ┌──────────────────────────────────────────┐
   │ memory/<role>/                           │
   │  project.md — true for THIS project only │
   │  craft.md   — holds across ALL projects  │
   └──────────────────────────────────────────┘

   Found an inaccuracy in team docs?
   · Restricts ME personally → propose; another role edits
   · Owner invariant         → show measurement; never overrule
   · Verified factual error  → fix in passing · mark old wording · date · inform Lead
```

---

## D-6. Board Statuses & Single Source of Truth

*Consolidates: `templates/board/README.md` §2 & §4.*

```text
    proven finding ──► open ◄── reopened (facts changed)
                        │
        ┌───────────────┼──────────────────┬──────────────────┐
        ▼               ▼                  ▼                  ▼
  ┌───────────┐  ┌──────────────┐   ┌───────────┐    ┌────────────┐
  │in progress│  │waiting on    │   │  future   │    │ unresolved │
  │+who +date │  │owner (1 list)│   │ (valid,   │    │ (explicit) │
  └─────┬─────┘  └──────┬───────┘   │  deferred)│    └────────────┘
        │               │ answered  └───────────┘
        │               ▼
        │             open
        ▼
  ┌───────────┐  ┌──────────────────────────────────┐
  │   done    ├─►│ board/DONE.md (pruned 2 cycles)  │
  └───────────┘  └──────────────────────────────────┘

  obsolete ◄── closed WITH written rationale (never silent)

  TRUTH DIRECTION:  board/OPEN.md ──summarized into──► board/00-START.md
                    If counts diverge → 00-START.md is wrong.
  STALE CHECK:      "in progress" older than 24h → investigate immediately.
```

---

## D-7. Cycle Stages (Aligned with P-1: Run First, Read Second)

*Consolidates: `roles/LEAD-PROCEDURES.md` P-1, P-12, P-13.*

```text
 ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐
 │ 1. ENTRY     │─►│ 2. DISPATCH  │─►│ 3. EXECUTION │─►│ 4. MERGE     │
 ├──────────────┤  ├──────────────┤  ├──────────────┤  ├──────────────┤
 │ RUN checks   │  │ size roster  │  │ report →     │  │ merge no-ff  │
 │ worktrees/CI │  │ (3 limits)   │  │ verify #s →  │  │ generators   │
 │ live app ok? │  │ non-overlap  │  │ decide →     │  │ IN ORDER     │
 │ THEN read    │  │ territories  │  │ update board │  │ re-run ALL   │
 │ board + plan │  │              │  │              │  │ checks fresh │
 └──────────────┘  └──────────────┘  └──────┬───────┘  └──────┬───────┘
                                            │                 │
                         impacting finding ─┘                 │
                         → forward immediately                ▼
 ┌──────────────┐  ┌──────────────────────────────────────────────────┐
 │ 6. CLOSURE   │◄─┤ 5. END OF SHIFT                                  │
 ├──────────────┤  ├──────────────────────────────────────────────────┤
 │ ALL criteria │  │ Step 0: check dirty worktrees · run checks ·     │
 │ met (never   │  │ sync board · update STATE.md · update memory ·   │
 │ majority) +  │  │ doc housekeeping (P-12-bis) · 1 Owner summary    │
 │ Runtime QA   │  └──────────────────────────────────────────────────┘
 └──────────────┘
```

---

## D-8. Crash & Interruption Recovery

*Consolidates: `TEAM.md` §5, `roles/LEAD-PROCEDURES.md` P-14, P-16.*

```text
   session interrupted
         │
         ▼
  ┌──────────────────────────────────────────────────────────┐
  │ DO NOT DELETE, REVERT, OR RESET UNFAMILIAR CHANGES       │
  └──────────────────────────┬───────────────────────────────┘
                             ▼
  ┌──────────────────────────────────────────────────────────┐
  │ git log = merged only; inspect git status + file mtime   │
  │ across ALL worktrees; snapshot diffs outside /tmp        │
  └──────────────────────────┬───────────────────────────────┘
                             ▼
  ┌──────────────────────────────────────────────────────────┐
  │ Treat uncommitted work as UNVERIFIED until matched to    │
  │ live check runs; rescue orphaned work via P-16           │
  └──────────────────────────┬───────────────────────────────┘
                             ▼
                resume from STATE.md + board/OPEN.md
```

---

## D-9. Role Workflows: Editing vs. Read-Only Auditing

*Consolidates: `roles/*.md`.*

```text
  EDITING ROLES                      │  READ-ONLY AUDITING ROLES
  (Executor, Junior, Team Lead,      │  (Analyst, Architect, Designer,
   Tester in verification layer)     │   Security)
  ───────────────────────────────────┼───────────────────────────────────
    assignment                       │    target surface / profile
       │                             │       │
       ▼                             │       ▼
    MEASURE baseline first           │    open uninspected cells / seams
       │                             │       │
       ▼                             │       ▼
    simulate before risky edits      │    challenge premise & defect shape
       │                             │       │
       ▼                             │       ▼
    implement change                 │    prove with reproducible metric
       │                             │       │
       ▼                             │       ▼
    mutation-test check              │    document & hand off to owner
       │                             │       │
       ▼                             │       ▼
    full checks + explicit commit    │    incremental report + memory
       │                             │
       ▼                             │  Runtime / Device QA sits alongside:
    incremental report + memory      │  exercises live app + fills matrix
  ───────────────────────────────────┴───────────────────────────────────
  WHY SEPARATE: editing destroys independence — fixers review themselves.
```

---

## D-10. Project Bootstrap (`P-0`)

*Consolidates: `roles/LEAD-PROCEDURES.md` P-0, `templates/README.md`.*

```text
   Clone ai-team next to project
         │
         ▼
   Choose Mode:
   · Lite Mode  ──► copy templates/AGENTS.md + templates/PROJECT.md
   · Full Mode  ──► copy PROJECT.md + board/* + active memory/<role>/*
         │
         ▼
   Fill PROJECT.md from LIVE measurements (leave unknowns empty)
         │
         ▼
   Verify repository boundary:
   · ai-team repo  ◄── generic charter, roles, procedures, templates ONLY
   · project repo  ◄── AGENTS.md, PROJECT.md, board/, memory/, reports
   · Confirm with `git ls-files` inside ai-team
```
