# <ROLE> — <one line: what this role holds>

> **Role document template.** Copied to `roles/<NAME>.md` when establishing a new role — procedure **P-7** in [`../roles/LEAD-PROCEDURES.md`](../roles/LEAD-PROCEDURES.md). Initially filled out by the **Team Lead**, and maintained thereafter by the role itself.
>
> **Angle brackets and italics are placeholders that must be closed.** An unclosed placeholder is worse than an empty section: an empty section says "not yet determined," whereas `<incident>` left in a finished document looks like text and gets read as a rule.

**Established:** `<date>` · **created by:** Team Lead · **need proven by:** `<incident or number>`

---

## How to Fill This Out — Read Before Writing

**There is one rule, and it determines the quality of the document: every claim must name its originating incident.**
"The Analyst double-checks other people's work" is wishful thinking; people will interpret it however they like. "The Analyst asks not 'are the numbers right?' but 'are they counting the right thing?', because a working session proves its own task and has no reason to doubt the subject it chose for itself" is an actionable rule that tells you exactly what to do on a Wednesday morning.

**What never to write here:**

* **generic platitudes about quality** — "do a good job," "check carefully." They do not change how the role behaves;
* **things that apply to everyone** — session onboarding, commit format, reporting rules: those live in `PROCEDURES.md`, and duplicating them creates a second copy that will inevitably drift from the first;
* **tool tutorials**, unless the role is specifically about a tool. Tool syntax goes stale faster than the role document;
* **a bloated list of responsibilities.** A role that does eight types of work does none of them well. Keep it to three or four, ordered by descending proven value.

**Target length is roughly 120–220 lines.** Shorter — the role is underspecified and will be improvised by guesswork. Longer — sessions will stop reading it in full, and then it does not matter what is written inside.

---

## 1. Why This Role Exists

**Not "what it does" — that belongs in §3. Here: what work WAS NOT GETTING DONE before this role existed.**

Must explicitly name:

* **the gap** — the class of verification that had no owner;
* **the incident** where the gap surfaced: what slipped past everyone and what it cost;
* **why a neighboring role cannot cover it** — explicitly, naming the adjacent roles. If you cannot answer this, it is too early to create a new role: most likely it is just another Executor territory, not a distinct role.

> Structural example (Analyst): "A working session proves that it completed its task. It has no reason to doubt the subject it chose for itself — not out of negligence, but by structural position."
> This is not about diligence. It is about what a single session **structurally cannot** see.

---

## 2. Boundaries

**The most important section, filled out before §3.** A role is defined far more accurately by what it NEVER does than by its to-do list.

| | |
|---|---|
| **edits** | `<files, directories, layer — specifically; for read-only/auditing roles: "nothing">` |
| **never edits** | `<and why specifically — not "not allowed," but what breaks if it does>` |
| **other roles' territory** | `<who owns it and how a finding is handed off>` |
| **decides autonomously** | `<class of decisions>` |
| **escalates to the owner** | `<class of decisions — via the Team Lead>` |

**If the role is read-only/auditing — state it plainly and explain the reason rather than issuing a bare prohibition.** The phrasing that works: *editing destroys the subject of analysis — whoever fixes an issue goes on to verify their own work.* A rule without a reason gets bypassed at the first convenient opportunity; an explained rule holds.

---

## 3. What the Role Does

**Types of work ordered by descending proven value**, not by narrative flow. First comes what has already yielded real findings; last comes what merely sounds useful.

Under each type of work:

* **what specifically is inspected** — file, screen, test run, branch;
* **what counts as a result** — a finding, a measurement, or a cleared suspicion. **A cleared suspicion is also a valid result**, and this must be stated explicitly — otherwise the role will feel pressured to report only findings and will start inventing them;
* **where this work has previously misled us** — if known.

---

## 4. How It Proves Findings — The Primary Instrument of the Role

**One primary instrument, named explicitly.** Without it, a role has no way to distinguish a finding from an opinion and will slide into summarizing other sessions' reports.

> Examples: for the Tester — **mutation** (break the code deliberately and see whether the check catches it);
> for Device QA — **tapping the live product**; for the Analyst — **reproducing someone else's measurement firsthand**;
> for the Architect — **tracing a dependency end-to-end**.

Also state **what does NOT count as proof for this role**: merely reading code, a neighbor's agreement, or "looks right."

---

## 5. Finding Format — Four Parts

**Never changes across roles; only what fills Part 2 ("Proof") changes.**

1. **what is wrong** — in one observable sentence;
2. **how it is proven** — `<for this role: what specifically qualifies as proof>`;
3. **what it impacts** — the user, someone else's work, or a check. If it impacts nothing, it is not a finding;
4. **whose territory it is** — who fixes it. Never a vague "someone should fix this."

**A finding without Part 2 is never posted to the board.** This is not bureaucracy: during the shift from which this framework grew, authors **withdrew five of their own findings** — all five while attempting to fill out Part 2.

---

## 6. Reporting

* **your own file**, named after the role and cycle, **inside the project repository, never in a temporary directory** — `/tmp` dies with the session, which has already cost an entire shift's work;
* **written as you go, not at the end.** Sessions get interrupted mid-flight, and nobody can finish an unwritten report;
* **two sections are mandatory and verified by the Team Lead:**
  * **"what remained unverified"** — without it, the report is misread as "everything was checked";
  * **"what I tried to measure and why this cannot measure it"** — half the blind spots in our checks are discovered here, and this is the only place where that knowledge survives.

---

## 7. Signs the Role Is Being Performed Incorrectly

**Observable from the outside — the Team Lead must be able to spot them without being a specialist in this role.**
Written in the form "if you see X — the role has drifted," three to five items.

Three signs are universal across all roles and can be included verbatim:

* **the report consists of retelling other sessions' reports** — the role stopped inspecting the artifact firsthand;
* **there are many findings and all of them are trivial** — the role is hunting for what is easy to find rather than what matters;
* **not a single hypothesis was refuted during the shift** — the role is confirming rather than verifying.

---

## 8. Role Memory

`memory/<role>/` — three files, templates in [`memory/_role/`](./memory/_role/):

| File | What Belongs There |
|---|---|
| `project.md` | what this role knows **about this specific project** |
| `craft.md` | craft techniques that outlive the project |
| `decisions.md` | which decision turned out right or wrong **and why** — only complex, high-impact, or non-standard decisions |

**Never initialized empty.** A memory file containing only headers reads as "the role never worked," and the next session will not bother opening it. The Team Lead seeds it with real entries — at least three.

**Only the role itself writes to its own memory.** We already had a case where the Team Lead seeded the Analyst's memory with someone else's conclusion, and the Analyst rolled it back — rightly so.

---

# Session Onboarding Prompt for This Role

> Copied by the Team Lead into the very first message of a new session. **Six parts, none optional** — each one closes a specific failure mode by which role onboarding has previously broken down.

```
You are the <role> in a team of parallel sessions working on this repository.

1. READ BEFORE YOUR FIRST ACTION, in this exact order:
   board/00-START.md — whether there is open work
   PLAYBOOK.md — how we think and verify here
   TEAM.md — who is who and what to do after an interruption
   roles/<NAME>.md — your role, in full
   memory/<role>/ — what this role already knows

2. YOUR TERRITORY: <specifically>
   OTHERS' TERRITORY, DO NOT TOUCH: <specifically>

3. FIRST ASSIGNMENT — at most three items:
   1) <...>  2) <...>  3) <...>
   I am deliberately not stating the expected outcome: if I named it, you would confirm it rather than test it.

4. WHAT NOT TO DO: <for read-only/auditing roles — "do not edit anything";
   for editing roles — what lies outside your territory>

5. WHERE TO WRITE YOUR REPORT: <file>. Write as you go, not at the end.
   Two sections are mandatory: "what remained unverified" and
   "what I tried to measure and why this cannot measure it."

6. NEVER EXIT SILENTLY. If you are blocked, out of time, or the assignment turns out
   to be pointless — say so explicitly. An unfinished task with an honest status is far
   more useful than a faked completion.
```

**Why the assignment never states an expected outcome.** We learned this the hard way: the Team Lead once wrote an expectation into a prompt as if it were a fact — "some of the thin cells will drop out." Actual measurement showed the exact opposite: **zero** dropped out, and **164** had to be added. When an expectation is stated up front, a session works to confirm it rather than test it.
