# Start — The First File You Open

> **Template.** Deploys to `<project>/board/00-START.md`. Operating rules — `board/README.md`.
> Filled out and updated by the **Team Lead**: at the start of a shift, upon every card status change, and at the end of a shift.

**Updated: `<date>` · updated by: `<role>`.**

> This file answers a single question in ten seconds: **is there open work, and where is it?**
> If the answer is "no" — you do not need to read any further.

---

## ✅ NO OPEN WORK

*(default state upon initial deployment — replace with the block below as soon as open work appears)*

Below is only the table of plans. **Listing closed tasks here is forbidden:** that is what `DONE.md` is for, and `DONE.md` gets pruned regularly. Otherwise this file turns into a graveyard and people stop opening it — defeating the sole reason it exists.

---

<!-- ─── REPLACE THE TOP BLOCK WITH THIS ONE WHEN THERE IS OPEN WORK ────────
## ⛔ OPEN WORK EXISTS

```
P0  needs attention    N     visible to the user right now
P1  blocking           N     someone else's work is stalled
P2  normal             N
P3  future             N
──────────────────────────
    waiting on owner   N     cannot proceed, decision is not ours
    unresolved         N     we do not know what to do with this yet
```

**These numbers must match the exact count of cards in `OPEN.md`.** If they diverge — THIS file is wrong.

## What to Tackle First

| | ID | What | Assigned To |
|---|---|---|---|
| **P0** | | | |
| **P1** | | | |

──────────────────────────────────────────────────────────────────────────── -->

## By Plans — Not by Individual Tasks

| Plan / Workstream | Status | Remaining Scope |
|---|---|---|
| | 🟢 closed · 🟡 in progress · 🔴 not started | |

**A closed plan is a single row.** It tells you at a glance that there is no need to read its report.

---

## Priorities

| | Meaning | Criterion |
|---|---|---|
| **P0** | **visible to the user right now** | a person opening the product today will hit it |
| **P1** | **blocks someone else's work** or breaks a verification check | a session is stalled or receiving a false signal |
| **P2** | normal work | neither P0 nor P1 |
| **P3** | future | accepted as valid, but not for the current cycle |

**Changed only by the Team Lead, and always with a date** — so a session resuming after an interruption can tell how fresh the assessment is. A priority without a date is invalid.

**P0 never piles up.** More than three P0 items at once means either priorities are being inflated or work has stalled — and in either case the Team Lead must raise it **out loud with the owner**, rather than leaving it sitting in a file.
