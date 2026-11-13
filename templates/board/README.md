# Team Board — Open Findings, Questions, and Plans

**Established by the owner on 2026-08-29.** Part of the team documentation, **bound to a specific project**.

> "The Team Lead must know to inspect this folder for tasks and open items, check what has gone stale or irrelevant, delete old items, and execute what is open... And clean out old and outdated items on time so it doesn't get cluttered."
> — Owner, 2026-08-29

---

## 1. Purpose and How It Differs from Neighboring Files

| File | Contents | Lifespan |
|---|---|---|
| **`board/OPEN.md`** | **what is unfinished right now**: unowned findings, questions for the owner, active plans | until closed |
| **`board/DONE.md`** | what was closed — by whom and when | **two session cycles, then pruned** |
| [`../PLAYBOOK.md`](../../PLAYBOOK.md) | recurring patterns and verification techniques | permanent |
| `memory/` | role-specific memory | permanent |
| cycle log | decisions with their rationale | archived with the cycle |

**The distinction is simple: the board is about work; the playbook and memory are about knowledge.** Closed work leaves the board; the knowledge extracted from it stays in the playbook and memory.

---

## 2. How the Board Is Structured: One Entry Point, Three Levels

**Owner's requirement, 2026-08-29:** "the base file immediately tells you whether there are tasks or not... at a minimum it orients you on where open tasks live and states their priority... if all tasks are solved, record that there are no tasks, and summarize **by plans rather than by every individual task**, so we don't make a mess."

```
00-START.md   ← ALWAYS opened first. Answers in ten seconds:
              │  whether open work exists, how many items per priority, what to tackle first,
              │  and the PLANS table — which are closed and where remaining work lives
              ↓
OPEN.md       ← opened ONLY for the specific rows you need. Never read cover-to-cover
              ↓
DONE.md       ← opened rarely: to check who closed something and when. Regularly pruned
```

**The purpose of having three levels is simple: never read what you do not need.** A closed plan in `00-START.md` is a single line, showing at a glance that there is no need to read its detailed report.

### 2.1. The Rule That Keeps the Board from Drifting out of Sync with Reality

**`OPEN.md` is the single source of truth. `00-START.md` is derived FROM it, never written independently.**

From this follows a one-minute check the Team Lead performs on every update:

* **the numbers in the header of `00-START.md` must match the exact count of cards in `OPEN.md` by status.** If they diverge, **`00-START.md` is wrong**, not `OPEN.md`;
* **every card in `OPEN.md` must have a "Plan" field** — otherwise it will be omitted from the plans table and forgotten when the plan closes;
* **a plan is declared closed only when `OPEN.md` has zero cards carrying its tag.** Not "I think we finished it," but "a search for its tag returns zero results."

### 2.2. When the Board Is Updated

Three moments, all three mandatory:

1. **at the start of a shift** — the Team Lead re-reads `OPEN.md`, updates priorities, and refreshes the `00-START.md` summary;
2. **whenever any card changes status** — immediately, not "later": a board that lags by an hour lies if the session is interrupted;
3. **at the end of a shift** — summary update and pruning of `DONE.md`.

**Self-revealing symptom of a missed update:** a card with status `in progress` whose date is more than 24 hours old. Either the session died or someone forgot to update the status — either way, investigate it rather than scrolling past.

### 2.3. When Everything Is Closed

`00-START.md` begins with the heading `✅ NO OPEN WORK`, followed **only by the table of plans with their closure dates**. `OPEN.md` then contains a single line: `Empty as of <date>.`

**Listing closed tasks in the entry file is forbidden** — that is what `DONE.md` is for, and `DONE.md` gets pruned. Otherwise the entry file turns into a junkyard and sessions stop opening it, destroying its sole purpose.

---

## 3. Team Lead's Duty When Starting Work

**Open `00-START.md`. If it says `✅` — you do not need the board; go start work.** Otherwise, read `OPEN.md` **for the relevant rows**, not cover-to-cover. Do this before handing out assignments, not "when I get around to it" — because `OPEN.md` holds issues that were already discovered and left unfinished, and assigning new work without checking it means losing those findings a second time.

Three actions, every cycle:

1. **review all open items** and decide for each: tackle now, defer, or mark obsolete;
2. **verify currency** — any entry whose target file was renamed, whose metric no longer reproduces, or whose root cause vanished is updated or removed **with a one-line explanation of why**;
3. **prune `DONE.md`** of anything older than two session cycles.

---

## 4. Statuses

**Priority (`P0`…`P3`) sits next to the status and is changed only by the Team Lead, always with a date.**
`P0` — visible to the learner/user right now · `P1` — blocks someone else's work or breaks a verification check · `P2` — normal · `P3` — future. **P0 never piles up:** more than three at once means either priorities are being inflated or work has stalled, and the Team Lead must report this to the owner.

| Status | Meaning | Action |
|---|---|---|
| `open` | stated and proven, no owner assigned yet | assign to a session or explain why it waits |
| `in progress` | claimed, naming who took it and when | leave alone, await report |
| `waiting on owner` | blocked on a decision that is not ours to make | consolidate into a single list and ask the owner at once |
| `needs attention` | visible to the learner/user **right now** or breaks someone else's work | tackle ahead of everything else |
| `future` | accepted as valid, but not for the current cycle | do not assign now, do not lose |
| `unresolved` | we do not know what to do with this yet | state that out loud rather than hiding it under `open` |
| `done` | closed and verified | move to `DONE.md` with date and author |
| `obsolete` | root cause disappeared or subject changed | remove **with an explanation**, never silently |

**An idea for improving the team's workflow is recorded as a card with status `future`, and never otherwise.** It is filed as **a single line** and waits until requested. Its author does not implement it on the spot "since I noticed it": per `PROCEDURES.md` GP-13, we do not self-modify process mid-flight — we record the idea and move on. **A one-off incident never spawns a process card** — only a recurring one does; the first occurrence is fixed on the spot.

**A status without a date is invalid.** Status describes a point in time; two weeks later, an undated `in progress` means "nobody knows."

---

## 5. Card Format

```markdown
### <ID>. Short title — what specifically is wrong
**Status:** P0 · needs attention · 2026-08-29 · found by `-69`, owned by `-44`
**Plan:** P6 → P2          ← mandatory: who found it and whose workstream owns the fix. Without this, the card gets lost
**What:** one or two lines, without retelling the history.
**How proven:** command, number, file. Without this, the entry is an opinion, not a finding.
**Impact:** what it means for the learner/user or the team's work. If nothing — state so explicitly.
**Next step:** concrete next action and whose territory it belongs to.
```

**IDs are sequential and never reused**, even after closure: reports and commits reference them permanently. Standard prefixes include `F` for device findings, `D` for design, `Q` for owner questions, and `P`/`T` for plan items. Introduce a new prefix only for a genuinely new category.

**What never belongs on the board:** a retelling of a report (link to the report — yes; paste it — no); a task without proof; or something that can be fixed within an hour inside your own territory — that is regular work, not a board item.

---

## 6. Who Writes to the Board

**Any participant may add their own findings to `OPEN.md`.** Roles that do not edit production code (Analyst, Designer, Architect, Security) write here **instead** of editing code: this is their mechanism for handing off work.

**Statuses and pruning are managed by the Team Lead.** A participant may set `open` on their own new entry and `done` on their own completed task — changing anyone else's status is reserved for the Team Lead.

---

## 7. Pruning — Mandatory, Not Optional

**`DONE.md` is pruned after two session cycles** (roughly a week of work). The entire entry is removed; if it produced lasting knowledge, that knowledge is already in `PLAYBOOK.md` or role memory by then, so nothing is lost.

**What gets pruned from `OPEN.md`:** `obsolete` — immediately upon discovery; `future` items that have sat untouched for three cycles without being referenced — reviewed and either promoted to the improvement registry or dropped.

**Sign that the board has decayed:** sessions stop opening it at the start of a shift because it has grown too long. When that happens, prune and split it by category rather than letting it bloat further.
