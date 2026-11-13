# Role Memory — How It Is Structured and Maintained

**Established by the owner on 2026-08-29.**

> "For team members — add a dedicated directory for each role that serves as its memory. Separate memory regarding the specific project it is working on, and separate memory that records useful solutions, patterns, verified techniques... not raw code, but a name to orient yourself, or a rule, or explanatory text... maintaining its own experience independent of the project. Including problems encountered, how they were solved, and core principles. And whatever goes stale — delete it or mark it as obsolete, because..."
> — Owner, 2026-08-29

---

## 1. Why Have Role Memory When PLAYBOOK Already Exists?

**Three distinct kinds of knowledge that do not replace one another:**

| File | Contents | Maintained By | Outlives |
|---|---|---|---|
| [`../PLAYBOOK.md`](../../PLAYBOOK.md) | **universal for all roles**: defect shapes, how our meters fail, cross-project orientation | Team Lead, from proposals by all roles | cycles |
| `memory/<role>/project.md` | **this role on this project**: where things live, which areas are fragile, what has already been verified and with what result | the role itself | cycles |
| `memory/<role>/craft.md` | **the role's craft in general**: principles, patterns, problems, and solutions that work on the next project too | the role itself | **the project** |
| `memory/<role>/decisions.md` | **which decision in which situation turned out right or wrong** — and why | the role itself | **the project** |

**How to decide which file an entry belongs in** — ask one question: **"Will this remain true in another project?"**
Yes → `craft.md`. No → `project.md`. Useful **to every role, not just mine** → propose it to the Team Lead for `PLAYBOOK.md`, and leave a one-line pointer in your own memory.

**Never duplicate.** Once an entry is promoted to `PLAYBOOK.md`, delete the body from role memory and leave a single pointer line: `See PLAYBOOK §N`.

---

## 2. First Action of a Role in a New Session

Right after reading `TEAM.md` and your own role document — **read your role memory in full**. It holds your accumulated experience and is shorter than any cycle report.

**Last action before finishing a session** — append what you learned. Not "what I did today" (that belongs in the cycle report), but **what you know now that you did not know this morning**.

---

## 3. Mandatory Entry Format

One entry = **one insight**. The heading is a **landmark** by which you can find it a month later, not a summary of the paragraph.

```markdown
### Short landmark title
**Status:** active · 2026-08-29
**What:** one line — the rule or solution itself.
**Why:** the concrete incident or measurement from which it was derived. Without this, the entry is invalid.
**How to apply:** specific enough to execute without guessing.
```

**What never belongs in an entry:**

* **code blocks.** No snippets, no diffs. Naming a function or file as a landmark is fine; pasting its implementation is not — code goes stale faster than the rule;
* **a rule without an incident.** "It's better this way" can neither be verified nor retired when conditions change;
* **session IDs.** An incident is remembered **by its cost, not by its author**: a month later "workstream 3" means nothing, whereas "forwarded a symptom as a root-cause finding, costing another session a full shift" immediately identifies the situation where the rule applies. Naming the role is fine; naming the session ID is not;
* **retelling existing documentation.** If it is already in the role file or `PLAYBOOK.md`, read it there;
* **anything specific to a single one-off task.** That belongs in the cycle report.

---

## 3a. `decisions.md` — The Decision Log, with a High Bar for Entry

**Owner's requirement, 2026-08-29:** "maintain experience on which decision in a given situation turned out right or wrong, what helped or failed and why — **but not everything indiscriminately, so as not to clutter it**; record only genuinely complex, high-impact, or non-standard decisions and findings."

**Threshold: create an entry only if at least one of these four conditions holds:**

1. **the decision was non-obvious** — there were two reasonable paths, and choosing between them required real justification;
2. **the decision turned out wrong** — which is even more valuable than a right one, because a wrong decision explains how we fooled ourselves;
3. **the technique was non-standard** — we did something unconventional and it worked;
4. **a finding changed someone else's workstream** or overturned a previously settled decision.

**Never record:** a routine decision that follows directly from an existing rule (it is already in the rule); "tried X and it worked"; or anything tied to a single task that will never recur.

**Format — the same, plus the outcome:**

```markdown
### Situation in one line
**Status:** active · 2026-08-29
**Fork in the road:** what the options were, including "do nothing."
**Chosen:** what was chosen and on what grounds.
**Outcome:** worked / failed / turned out to be the wrong question — **and how that was discovered**.
**What I would do differently:** one line. If nothing — state so explicitly.
```

**The outcome is mandatory.** An entry without an outcome is a plan, not experience, and misleads future sessions worse than having no entry at all: the next session will mistake an unverified choice for a proven one.

**A wrong decision is never deleted when it turns out to be wrong** — its `Outcome` field is updated instead. That is where the entire value of this file lies.

---

## 4. Obsolete Entries

**Marked as obsolete, never erased silently** — per the owner's explicit instruction.

```markdown
### Landmark title
**Status:** OBSOLETE since <date> — <what changed and by which decision>.
**What it used to be:** …
```

**Deleted outright** only when an entry turned out to be **false from the very beginning** (rather than becoming obsolete over time), and even then its removal is noted with a single line in the cycle log explaining why it was false. The distinction matters: an obsolete rule was valid in its own time, and the next session needs to see that so they do not think it was simply a mistake.

**Signs an entry needs review:** it got in the way twice and never helped once; the number it cites no longer reproduces; the file or flag it names no longer exists.

**Checking `Status` is part of reading memory**, not a separate chore: if you spot an outdated entry while reading, mark it on the spot.

---

## 5. Who Writes to Whose Memory

**Only the role itself writes to its own memory.** Reading another role's memory is encouraged (an Analyst reading the Designer's memory sees what has already been verified), but editing someone else's memory is forbidden.

**The single exception:** the Team Lead may mark another role's entry as obsolete if it contradicts an adopted owner/project decision — recording the date and reason, without deleting the text.

---

## 6. Directory Layout

```
memory/
  README.md          this file
  lead/              Team Lead
  analyst/           Analyst
  designer/          Designer
  architect/         Architect
  executor/          Executor — SHARED across all executors: territories change, the craft does not
  tester/            Tester
  security/          Security
  device/            Device / Runtime QA
```

Each role directory contains `project.md`, `craft.md`, and `decisions.md`. An initialized empty template file is better than a missing file: it shows the role was deployed and has not accumulated entries yet, rather than making sessions wonder whether the directory was forgotten.

**On the shared Executor memory.** Multiple sessions write to `executor/` in parallel, making it the only place where two sessions' edits meet in the same file. The rule is simple and proven across merges:

* **always append to the end of your section**, never rewriting neighboring entries;
* **merge conflicts here are resolved by UNION**, never by picking one side: both entries are valid and each was derived from its own real incident. Picking one side throws away someone else's hard-won experience;
* **near-duplicates are fine** — two phrasings of the same rule derived from different incidents are more valuable than one, because they prove the pattern recurs. The Team Lead consolidates them when the file grows large, **combining the concrete examples rather than collapsing them into one**.
