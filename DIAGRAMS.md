# Diagrams — Sequences, Branches, and Relationships

**A diagram is created only where the subject is a sequence or a branch.** A list of roles belongs in
a table, not a diagram; the choice "my decision or the Owner's" belongs in a decision tree, because
in prose it takes a paragraph to read, whereas as a tree it takes three seconds.

**A diagram is the source of truth, not a copy.** For whatever it depicts, it is authoritative; the
adjacent text merely explains it. Two truths go stale independently, and one of them starts lying
more confidently than the other.

**Maintenance rule.** Every diagram states **what it consolidates** and **when it was last
verified**. If you edit that source document, you open the corresponding diagram. This requirement is
recorded in three places, and all three are mandatory: **P-8 step 5** ("verify backlinks") · **P-7
step 3**, where diagrams appear as a dedicated row in the "where to register a role" table ·
**P-12-bis check 4**, where the end-of-shift checklist asks whether a new workflow shape emerged that
is missing from the diagrams. **Diagrams rot faster than prose** because fixing a line of text is
easy, whereas redrawing a tree takes effort.

> **Previous wording (prior to 2026-09-03):** "this is recorded as a dedicated step in procedure P-8
> 'update documentation'". There was no dedicated step about diagrams in P-8 — the rule pointed to a
> procedure where no such step existed, so by the letter of the law, diagrams were never updated.
> Case: the 2026-09-03 audit found two stale numbers in D-1 within this very file, and the
> maintenance rule caught neither of them.

---

## D-1. Document Map: What Is Read from Where

*Consolidates: `TEAM.md` §1. Last verified: 2026-08-29.*

```
                       ┌─────────────────────────────┐
   ALWAYS FIRST ──────►│  board/00-START.md          │  Is there open work?
                       │  "✅ none" or P0/P1/P2/P3   │  10 seconds
                       └──────────────┬──────────────┘
                                      │ work exists
                                      ▼
                       ┌─────────────────────────────┐
   ONCE ──────────────►│  PLAYBOOK.md                │  How we think here:
   upon entry          │  defect shapes · how our    │  10 minutes
                       │  own probes lie             │
                       └──────────────┬──────────────┘
                                      ▼
                       ┌─────────────────────────────┐
                       │  TEAM.md                    │  Who is who, comms, recovery
                       └──────────────┬──────────────┘
                                      ▼
        ┌───────────────┬─────────────┴───────┬──────────────────┐
        ▼               ▼                     ▼                  ▼
 ┌────────────┐  ┌─────────────┐     ┌───────────────┐   ┌──────────────┐
 │ roles/<me> │  │ PROCEDURES  │     │ memory/<me>/  │   │ PROJECT.md   │
 │ what & why │  │ how, step   │     │ what I        │   │ gates, rules,│
 │            │  │ by step,    │     │ already know: │   │ territories  │
 │ read in    │  │ when a      │     │ project ·     │   │ — THE SINGLE │
 │ full before│  │ situation   │     │ craft ·       │   │ project-wide │
 │ 1st action │  │ arises      │     │ decisions     │   │ source       │
 └────────────┘  └─────────────┘     └───────────────┘   └──────────────┘
        │                                                        ▲
        │ Team Lead additionally                                 │
        ▼                                                 come here for
 ┌──────────────────────┐        ┌─────────────┐          "how does it
 │ roles/LEAD-PROCEDURES│        │ SOURCES.md  │◄── as needed, work here?"
 │ 19 situations        │        │ where to    │    your section
 └──────────────────────┘        │ learn       │
                                 └─────────────┘
```

**Read top to bottom exactly once.** After that, open files on demand: a procedure when a situation
arises, the board by the relevant line, and sources when an external answer is needed.

**For the Team Lead — not the very first step.** This map answers "what is read from where", not
"how to start a shift": the Team Lead's first hour is **executed**, and the order is reversed — run
the suite first, then read (P-1, Part A). Entering a shift by following this diagram would bring the
Team Lead to the commands with pre-formed expectations, priming them to confirm those expectations.

> **Verified 2026-09-03:** "15 situations" updated to 19. The table of contents in
> `LEAD-PROCEDURES.md` lists P-0…P-15, while the file body also includes P-3-bis, P-12-bis, and
> P-16 (with P-16 missing from the table of contents). The number in the diagram lagged by four while
> looking authoritative — the exact reason a stale diagram is more dangerous than stale prose.

---

## D-2. Lifecycle of a Finding: From Observation to Closure

*Consolidates: `PROCEDURES.md` GP-3, GP-4, GP-9. Last verified: 2026-08-29.*

```
   observation
       │
       ▼
  ┌─────────────────────────┐   no    ┌──────────────────────────────┐
  │ Is this code reachable  ├────────►│ NOT A DEFECT. Record in the  │
  │ by a live scenario?     │         │ same detail as a finding     │
  └───────────┬─────────────┘         └──────────────────────────────┘
              │ yes
              ▼
  ┌─────────────────────────┐   no    ┌──────────────────────────────┐
  │ Did you read THE ACTUAL ├────────►│ Read them. A false finding   │
  │ CASES, not just a count?│         │ is worse than a missed one   │
  └───────────┬─────────────┘         └──────────────────────────────┘
              │ yes
              ▼
  ┌─────────────────────────┐
  │ Round # → check probe   │   "348 of 348" — almost always a probe bug
  │ Huge #  → check meaning │   "3015 of 4094" — almost always the wrong question
  └───────────┬─────────────┘
              ▼
  ┌─────────────────────────┐  mine   ┌──────────────────────────────┐
  │ Whose territory?        ├────────►│ Fix it myself; measure first │
  └───────────┬─────────────┘         └──────────────┬───────────────┘
              │ someone else's                       │
              ▼                                      │
  ┌─────────────────────────────────────┐            │
  │ ASK the territory owner             │            │
  │ before calling it a defect          │            │
  │ (half are resolved by one question) │            │
  └───────────┬─────────────────────────┘            │
              │ confirmed                            │
              ▼                                      │
  ┌─────────────────────────────────────┐            │
  │ Hand over: MEASUREMENT, not theory  │            │
  │ + probe boundaries ("74 — lower")   │            │
  └───────────┬─────────────────────────┘            │
              ▼                                      ▼
  ┌───────────────────────────────────────────────────────────┐
  │ board/OPEN.md — card: what · how proven · what it means   │
  │ · next step · PLAN · dated priority                       │
  └───────────┬───────────────────────────────────────────────┘
              ▼
       ┌──────────────┐  changes user-     ┌────────────────────┐
       │ Who decides? ├───────────────────►│ awaiting Owner     │
       └──────┬───────┘  visible behavior  │ (into ONE list)    │
              │ preserves status quo       └────────────────────┘
              ▼
       ┌──────────────┐        ┌──────────────────────────────┐
       │ in progress  ├───────►│ done → DONE.md               │
       └──────┬───────┘        │ (pruned after 2 cycles)      │
              │                └──────────────────────────────┘
              │ finding turns out to be wrong
              ▼
       ┌──────────────────────────────────────────────────────┐
       │ RETRACTION — just as immediately as a finding.       │
       │ Notify everyone who received it. Separate what is    │
       │ retracted from what still holds. Shape → decisions.md│
       └──────────────────────────────────────────────────────┘
```

---

## D-3. Decision Tree: My Decision or the Owner's

*Consolidates: `roles/LEAD-PROCEDURES.md` P-4. Last verified: 2026-08-29.*

```
                        Is a decision needed?
                              │
              ┌───────────────┴───────────────┐
              ▼                               ▼
   Changes what the user               Preserves the status quo
   sees or experiences?                       │
              │                               ▼
      ┌───────┴────────┐              ┌────────────────────┐
      ▼                ▼              │ MINE.              │
     YES              NO              │ Record the grounds │
      │                │              │ — without a "why", │
      │                └──────────────► the decision is    │
      ▼                                 invalid            │
 ┌─────────────────┐                  └────────────────────┘
 │ THE OWNER'S     │
 └────────┬────────┘                   Three classes, always the Owner's:
          │                            ──────────────────────────────────
          ▼                            1. What the product PROMISES
 ┌────────────────────────────┐           (scope, volume, answer grading)
 │ The question must carry:   │        2. ALREADY DECIDED by the Owner
 │ · numbers with measurement │           (discrepancy → show measurement,
 │   dates                    │            never overrule)
 │ · what changes for the user│        3. IRREVERSIBLE actions
 │ · options, including       │           (deletion, renaming keys)
 │   "leave as is"            │
 │ · caveats without which    │        Material rules ARE NOT SUBJECT
 │   the question misleads    │        to mandate — they outrank the
 └────────┬───────────────────┘        Team Lead's authority.
          ▼
   Batch into ONE list,
   ask all at once
```

**Self-test:** if the Owner cannot choose an option from your question without asking "what did you
mean by this?", the question is not ready.

---

## D-4. Decision Tree: Red in a Test Run

*Consolidates: `PROCEDURES.md` GP-5, `roles/LEAD-PROCEDURES.md` P-6. Last verified: 2026-08-29.*

```
                            test run is red
                                  │
                                  ▼
                       ┌──────────────────────┐
                       │ RE-RUN IT            │  A single run in a shared tree
                       └──────────┬───────────┘  proves nothing
                                  ▼
                       ┌──────────────────────────────────────┐
                       │ WHOSE IS IT? — not "mine looks clean"│
                       │ but: git stash your edits and        │
                       │ run on a clean HEAD                  │
                       └──────────┬───────────────────────────┘
                someone else's ◄──┴───► mine
                          │              │
                          ▼              ▼
                 hand to territory    classify the KIND — there are four:
                 owner
                                 ┌───────────────────────────────────────┐
                                 │ 1. Material violated a rule           │
                                 │    → fix the material, do not touch   │
                                 │      the ceiling                      │
                                 ├───────────────────────────────────────┤
                                 │ 2. Material BEAT the baseline (better)│
                                 │    → LOWER the ratchet baseline freely│
                                 ├───────────────────────────────────────┤
                                 │ 3. Artifact not rebuilt               │
                                 │    → run the generator                │
                                 ├───────────────────────────────────────┤
                                 │ 4. Defect in the gate itself          │
                                 │    → fix THE GATE, do not degrade     │
                                 │      the material to appease it       │
                                 └───────────────────────────────────────┘

  Hallmark of the fourth kind — the most important and most frequently missed:
  ┌──────────────────────────────────────────────────────────────────────┐
  │ "To make the gate green, we would have to make the material WORSE" — │
  │ that is a defect in the gate, not in the material.                   │
  │ Example: a repetition metric rose because one question was split     │
  │ into four; satisfying the metric meant collapsing them back to one.  │
  └──────────────────────────────────────────────────────────────────────┘

  Raising a ratchet baseline → mandatory record of WHICH DATA changed.
  Leaving a gate red "until we get to it" is FORBIDDEN: people stop reading
  a red gate within a day.
```

---

## D-5. Knowledge Routing: What Gets Saved Where

*Consolidates: `PROCEDURES.md` GP-11, `memory/README.md`. Last verified: 2026-08-29.*

```
                        I learned something
                              │
          ┌───────────────────┼────────────────────┬─────────────────┐
          ▼                   ▼                    ▼                 ▼
   Is it WORK?         Is it a PATTERN      Is it a SOURCE?   Is it a DECISION
   (needs to be done)  useful to EVERYONE?                    and its outcome?
          │                   │                    │                 │
          ▼                   ▼                    ▼                 ▼
  ┌──────────────┐   ┌────────────────┐   ┌──────────────┐  ┌────────────────┐
  │ board/OPEN   │   │ via Team Lead  │   │ via Team Lead│  │ memory/<me>/   │
  │ + priority   │   │ → PLAYBOOK.md  │   │ → SOURCES.md │  │ decisions.md   │
  │ + plan       │   │                │   │ PRIMARY      │  │                │
  └──────────────┘   │ keep a 1-line  │   │ sources only │  │ HIGH BAR:      │
          │          │ pointer locally│   └──────────────┘  │ non-obvious ·  │
          │          │ never duplicate│                     │ WRONG ·        │
          ▼          └────────────────┘                     │ non-standard · │
        done                  ▲                             │ changed others'│
          │                   │                             │ work           │
          ▼                   │                             │                │
  ┌──────────────┐            │                             │ OUTCOME IS     │
  │ board/DONE   │            │                             │ MANDATORY      │
  │ pruned after │            │                             └────────────────┘
  │ 2 cycles     │            │
  └──────────────┘            │
                              │
   Is it MY role's knowledge?─┤
          │                   │
          ▼                   │
  ┌───────────────────────────┴──────────────┐
  │ memory/<role>/                           │
  │  project.md — about THIS project         │
  │  craft.md   — craft that outlives project│
  │                                          │
  │ Routing test: "Will this still hold true │
  │ in ANOTHER project?" Yes → craft,        │
  │ No → project                             │
  └──────────────────────────────────────────┘
```

**Separate branch: I found an error in the documentation itself.**

```
   inaccuracy in a team document
              │
              ▼
   ┌──────────────────────────┐   yes   ┌────────────────────────────┐
   │ Does this rule restrict  ├────────►│ YOU ARE AN INTERESTED PARTY│
   │ ME SPECIFICALLY?         │         │ — propose, let others edit │
   └───────────┬──────────────┘         └────────────────────────────┘
               │ no
               ▼
   ┌──────────────────────────┐   yes   ┌────────────────────────────┐
   │ Is it an Owner decision  ├────────►│ Show a MEASUREMENT,        │
   │ or product invariant?    │         │ do not overrule            │
   └───────────┬──────────────┘         └────────────────────────────┘
               │ no
               ▼
   ┌───────────────────────────────────────────────────────────────┐
   │ EDIT IT MYSELF: name the case · MARK previous wording nearby  │
   │ · add a date stamp · inform the Team Lead in one line         │
   └───────────────────────────────────────────────────────────────┘
```

**Nothing is duplicated.** When an entry is promoted to `PLAYBOOK.md`, it is removed from role
memory and replaced with a one-line pointer. **Memory entries never contain session names:** a case
is identified by its **cost**, not its author.

---

## D-6. The Board: Statuses and Transitions

*Consolidates: `board/README.md` §4. Last verified: 2026-08-29.*

```
                       ┌──────────┐
      finding ────────►│   open   │◄──────── reopened: underlying
      proven           └────┬─────┘          circumstance changed
                            │
        ┌───────────────────┼──────────────────┬──────────────────┐
        ▼                   ▼                  ▼                  ▼
 ┌─────────────┐    ┌──────────────┐   ┌─────────────┐   ┌──────────────┐
 │ in progress │    │ awaiting     │   │ future      │   │ undecided    │
 │ + WHO       │    │ Owner        │   │ valid, but  │   │ we do not    │
 │ + date      │    │ + added to   │   │ not in this │   │ know what to │
 │             │    │   master list│   │ cycle       │   │ do — state   │
 └──────┬──────┘    └──────┬───────┘   └──────┬──────┘   │ that openly  │
        │                  │                  │          └──────────────┘
        │                  │ answer           │ 3 cycles untouched
        │                  ▼                  ▼          → review
        │           ┌─────────────┐    ┌─────────────────────────────┐
        │           │ open        │    │ improvement backlog or drop │
        │           └─────────────┘    └─────────────────────────────┘
        ▼
 ┌─────────────┐         ┌──────────────────────────────────────────┐
 │ done        ├────────►│ board/DONE.md — by whom and when         │
 └─────────────┘         │ pruned after two session cycles          │
                         └──────────────────────────────────────────┘

 ┌─────────────┐   cause vanished / subject changed
 │ obsolete    │◄──── closed WITH AN EXPLANATION, never silently
 └─────────────┘

 SELF-CHECK that catches forgetfulness:
 ┌────────────────────────────────────────────────────────────────────┐
 │ An "in progress" card dated OLDER THAN 24 HOURS = either the       │
 │ session died or someone forgot to clear the status. Unmissable.    │
 └────────────────────────────────────────────────────────────────────┘

 SINGLE DIRECTION OF TRUTH:
    OPEN.md ──summarized into──► 00-START.md
    If counts diverge → 00-START is wrong, never OPEN
```

---

## D-7. Stages of a Cycle

*Consolidates: `roles/LEAD-PROCEDURES.md` P-1, P-12, P-13. Last verified: 2026-08-29.*

```
 ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐
 │ 1. ENTRY     │─►│ 2. DISPATCH  │─►│ 3. EXECUTION │─►│ 4. MERGE     │
 ├──────────────┤  ├──────────────┤  ├──────────────┤  ├──────────────┤
 │ board        │  │ compute      │  │ report →     │  │ merge        │
 │ run all gates│  │ session count│  │ verify       │  │ generators   │
 │ working trees│  │ (3 limits)   │  │ number →     │  │ IN ORDER     │
 │ is the app   │  │              │  │ decide →     │  │ re-run all   │
 │ WORKING?     │  │ non-overlap  │  │ update board │  │ gates AFRESH │
 │              │  │ territories  │  │              │  │              │
 └──────────────┘  └──────────────┘  └──────┬───────┘  └──────┬───────┘
                                            │                 │
                          finding changes   │                 │
                          others' work ─────┘                 │
                          → forward IMMEDIATELY               │
                                                              ▼
 ┌──────────────┐  ┌──────────────────────────────────────────────────┐
 │ 6. CLOSURE   │◄─┤ 5. END OF SHIFT                                  │
 ├──────────────┤  ├──────────────────────────────────────────────────┤
 │ ALL          │  │ run gates · board · STATE · memory · merge       │
 │ acceptance   │  │ branches · single consolidated report to Owner   │
 │ criteria,    │  └──────────────────────────────────────────────────┘
 │ not "most"   │
 │              │   Three unmet criteria out of seven = AN OPEN CYCLE,
 │ device QA    │   and must be reported as such.
 │ is MANDATORY │
 └──────────────┘
```

---

## D-8. Recovery After an Interruption

*Consolidates: `TEAM.md` §7, `roles/LEAD-PROCEDURES.md` P-14. Last verified: 2026-08-29.*

```
   sessions interrupted
        │
        ▼
 ┌──────────────────────────────────────────────────────────────┐
 │ DO NOT DELETE, REVERT, OR RESET ANYTHING                     │
 │ An unfamiliar edit is someone else's work, not garbage.      │
 │ The cost of error is asymmetric: mistaking live work for     │
 │ garbage is irreversible; the reverse costs 30 mins of reading│
 └──────────────────────┬───────────────────────────────────────┘
                        ▼
 ┌──────────────────────────────────────────────────────────────┐
 │ git log answers "what was MERGED", NOT "what was DONE"       │
 │ ──────────────────────────────────────────────────────────── │
 │ A shift can look dead by commits and alive by files:         │
 │ last commit at 22:36, file edits continuing until 23:54      │
 └──────────────────────┬───────────────────────────────────────┘
                        ▼
        ┌───────────────┴──────────────┐
        ▼                              ▼
 ┌─────────────────┐          ┌──────────────────────┐
 │ status of ALL   │          │ file mtime           │
 │ working trees   │          │ — the only thing     │
 │ (more than one) │          │ distinguishing live  │
 └────────┬────────┘          │ work from old        │
          │                   └──────────┬───────────┘
          └────────────┬─────────────────┘
                       ▼
 ┌──────────────────────────────────────────────────────────────┐
 │ SNAPSHOT uncommitted work — OUTSIDE temporary directories    │
 │ (temp dirs are wiped on reboot, which already cost a shift)  │
 └──────────────────────┬───────────────────────────────────────┘
                        ▼
 ┌──────────────────────────────────────────────────────────────┐
 │ An uncommitted edit = UNVERIFIED until you find the test     │
 │ run that verified it.                                        │
 │ "Verify" = check its own numbers against a measurement,      │
 │ not merely confirm that it compiles.                         │
 └──────────────────────┬───────────────────────────────────────┘
                        ▼
              board/OPEN.md — what was already found and left undone
```

---

## D-9. Role Workflows: Two Distinct Shapes

*Consolidates: role documents. Last verified: 2026-08-29.*

```
 ROLE THAT EDITS CODE            │  ROLE THAT DOES NOT EDIT CODE
 (Executor, Team Lead,           │  (Analyst, Designer, Architect,
  Tester within their layer)     │   Security)
 ────────────────────────────────┼──────────────────────────────────────
                                 │
   assignment                    │    domain area
      │                          │       │
      ▼                          │       ▼
   MEASUREMENT (not an edit)     │    open a cell where
   every number in the task      │    NOBODY has looked yet
   is unverified                 │       │
      │                          │       ▼
      ▼                          │    ask: "Are they counting the right
   simulation BEFORE editing     │    thing?" "What shape would a defect
      │                          │    take so the rule misses it?"
      ▼                          │       │
   edit                          │       ▼
      │                          │    prove with a MEASUREMENT
      ▼                          │       │
   test that FAILS on old code   │       ▼
      │                          │    document → hand over
      ▼                          │    SOMEONE ELSE FIXES IT
   test run (full, not targeted) │       │
      │                          │       ▼
      ▼                          │    report — THE ONLY artifact
   commit with explicit paths    │       │
      │                          │       ▼
      ▼                          │    memory + board
   report + memory               │
                                 │
 ────────────────────────────────┴──────────────────────────────────────
  WHY THE BOUNDARY IS STRICT: editing destroys the subject of analysis —
  anyone who fixes the code ends up reviewing their own work.
```

**Device / Runtime QA stands apart:** they neither edit nor analyze code — they **tap the real
device**. Their proof is a captured frame and a completed matrix, and nothing substitutes for it:
green tests prove internal code consistency, not that the product works.

---

## D-10. Bootstrapping a New Project

*Consolidates: `roles/LEAD-PROCEDURES.md` P-0, `templates/README.md`. Last verified: 2026-08-29.*

```
   clone of the framework repository
         │
         ▼
   Owner tells a single session:
   "Read the teamwork documentation and bootstrap it for this project"
         │
         ▼
 ┌────────────────────────────────────────────────────────────────────┐
 │ P-0. BOOTSTRAPPING — executed by the Team Lead                     │
 ├────────────────────────────────────────────────────────────────────┤
 │  templates/PROJECT.md      ──copy──►  <project>/PROJECT.md         │
 │  templates/board/*         ──copy──►  <project>/board/             │
 │  templates/memory/_role/*  ──copy──►  <project>/memory/<role>/     │
 │                                       (one per active role)        │
 └────────────────────────────┬───────────────────────────────────────┘
                              ▼
 ┌────────────────────────────────────────────────────────────────────┐
 │ FILL IN PROJECT.md — the single project-specific file:             │
 │ layout · checks · material rules · generators ·                    │
 │ ratchet baselines · device rig · branches · Owner-only decisions   │
 │                                                                    │
 │ An empty section is BETTER than a fabricated one: it says "unknown"│
 └────────────────────────────┬───────────────────────────────────────┘
                              ▼
 ┌────────────────────────────────────────────────────────────────────┐
 │ RECOMMEND SESSION COUNT AND ROLES TO THE OWNER                     │
 │ = non-overlapping territories, never more than you can supervise   │
 └────────────────────────────┬───────────────────────────────────────┘
                              ▼
                         work proceeds
                              │
                              ▼
 ┌────────────────────────────────────────────────────────────────────┐
 │ WHAT GOES WHERE ON PUSH                                            │
 │  framework repository ◄── roles, procedures, diagrams, templates   │
 │  project repository   ◄── PROJECT.md, board/, memory/, reports     │
 │                                                                    │
 │ The framework's .gitignore excludes board/, memory/, PROJECT.md,   │
 │ reports, and frames — so anyone cloning it gets a CLEAN FRAME      │
 └────────────────────────────────────────────────────────────────────┘
```

---

## What Is Intentionally Omitted from the Diagrams

* **Role roster** — that is a table; there is no branching;
* **Project-specific content rules** — those live in `PROJECT.md` and differ for every project;
* **Reading order within a single document** — a table of contents handles that better;
* **Anything that changes more than once a month.** A diagram with a high edit frequency is
  maintained by no one and quickly starts lying.

**A diagram that people have stopped verifying is worse than having no diagram at all:** stale prose
looks stale, whereas a stale diagram still looks verified.
