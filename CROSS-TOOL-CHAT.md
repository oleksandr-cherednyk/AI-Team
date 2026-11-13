# Cross-Tool Session Communication — File-Based Messaging

**Needed because live messaging does not work across every tool.** Sessions running inside the same
tool can often talk to each other directly; sessions across **different** tools generally cannot.
Every tool can read and write files.

**The second reason this exists:** file-based correspondence **survives disconnects**. A live message
vanishes when the session dies; a file stays on disk, so the next session sees what was being
discussed and where work stopped.

---

## 1. Where Messages Live — The Key Architectural Decision

> **Messages live OUTSIDE the repository, in a shared directory on disk.**

**Why not inside the repository.** Parallel sessions usually work in **separate working trees**
(`git worktree`) of the same repository. A file written inside one working tree **is invisible in
another** until it goes through commit, push, merge, and pull. A communication channel placed inside
the repository turns into **talking to yourself**, and nobody notices: messages go out, no replies
come back, and everyone assumes the other person is busy.

**Verify this with a live test, not by reasoning:** create a second working tree, write a message in
the first, and check from the second. If you see nothing, the channel is broken.

```
<shared directory outside all working trees>/
├── README.md                           where you are and where the rules live
├── general/<YYMMDD-HHMM>-<session>.md  general channel: one message = one file
├── roster/<session>.md                 who is doing what — ONE file per session
└── <topic>/<YYMMDD-HHMM>-<session>.md  dedicated discussion thread
```

| | Where | Why |
|---|---|---|
| **rules** | **inside the repository** | part of the standard: versioned, survives the machine, visible on clone |
| **messages** | **outside the repository** | requires **immediate** visibility across all working trees |
| **knowledge from chat** | **back into the repository** | once agreed, move it to the board or the run summary |

**What this gives you all at once:** synchronous visibility without git · **zero merge conflicts** ·
crash survival.
**What it costs:** chat history is not versioned and stays on the machine. That is deliberate —
**correspondence is ephemeral**.

---

## 2. Session Name

```
<tool>-<role>-<tag>
claude-lead-46 · codex-executor-a1 · claude-tester-8c
```

**Including the tool name is mandatory:** it tells others whether they can reach you via native
live messaging or only through files.

---

## 3. One Rule from Which Everything Else Follows

> **Nobody writes to someone else's file. One writer — one file. Always.**

Anyone may read anything; you may edit only your own files. Therefore: the general channel is a
**directory**, not a single file; each message is a separate file with a timestamp in its filename;
chronological order comes from filename sorting.

**Three header fields are mandatory:** `from` · `to` (`all` or session name) · `subject`. Without
them, messages cannot be found by search.

---

## 4. What `roster/<session>.md` Holds

The single place where a session **overwrites its own file**: what task it took · what it is doing
right now · whether it needs the shared test rig · what it is waiting on.

**Eliminates half the status questions between sessions** and, if a session crashes, shows exactly
where it stopped.

---

## 5. Mandatory Rules

| # | Rule |
|---|---|
| 1 | **Write only to your own files.** Never edit or delete someone else's file |
| 2 | **One message — one file.** Do not append to your old message: others have already read it |
| 3 | **Include the three header fields** (`from`, `to`, `subject`) |
| 4 | **Write to the shared directory, not your working tree.** A file inside your tree is invisible to everyone else |
| 5 | **Keep your `roster/` file fresh.** A stale roster is worse than none: others make decisions based on it |
| 6 | **Blocked or finished — write it down.** Silence is indistinguishable from "still working" |

## 6. Prohibited

* **editing or deleting someone else's file** — including "just fixing a typo";
* **making Team Lead decisions inside chat.** If two sessions agree to redraw a territory boundary,
  that is a proposal, not a decision;
* **leaving permanent knowledge in chat.** Once decided, move it to the board or documentation;
* **addressing `to: all` when the message concerns only one session.**

## 7. Conduct — What Rules Alone Cannot Enforce

* **always reply, even if the answer is "I don't know" or "not my territory."** An unanswered
  message **blocks someone else's work**, and the sender has no way of knowing why;
* **write so that someone who has never opened your code can understand it**;
* **state what happened first, then what you think about it**;
* **name what you did not verify** — your counterpart is building on your words right now.

## 8. Limitations — What This Channel CANNOT Do

* **there are no push notifications.** A file will not knock on your door; check the channel at
  natural checkpoints: when picking up a task · after finishing a slice · when blocked on a question ·
  before merging · before claiming a shared device or test rig;
* **this is not real-time chat.** Between "wrote" and "read" lies anywhere from a few minutes to an
  entire run. **Urgent matters do not go here**;
* **writing to the wrong path is the same as not writing at all.** Symptom: you wrote a message and
  **nobody replies**. Check the **file path**, not your teammate's patience.

---

## 9. Cleanup — A Rule, Not a Habit

Every message is a file, and left alone nobody feels obliged to clean them up. It is the exact same
failure mode that accumulates stale branches, stray directories, and dead lists.

| What | When | Who |
|---|---|---|
| **topic thread** | question is resolved **and both sides said "closing"** | any participant |
| **general channel** | older than two runs | Team Lead at the end of the shift |
| **`roster/<session>`** | session is closed and its work is merged | Team Lead |

**Never silently delete an unanswered message.** An unanswered message is not trash; it is an
**open question**: move it to the board first, then remove the file.

---

## 10. Onboarding Block for a Session Without Live Messaging

Insert this block **in full** into the session's prompt; verifying that "the communication channel
is named" is part of the pre-dispatch checklist (`LEAD-PROCEDURES.md`).

```
TEAM COMMUNICATION IS FILE-BASED — you do not have live messaging.

MESSAGE DIRECTORY (outside the repository, shared across all working trees): <path>
RULES (inside the repository, 5-minute read): <path to this file>

Your session name in chat: <tool>-<role>-<tag>

BEFORE YOUR FIRST ACTION:
  1. create roster/<your-name>.md — what you took, what you are doing, whether you need the shared rig
  2. read all of general/ and any message marked "to: all" or "to: <you>"

TO SEND A MESSAGE — create a NEW file, never edit existing ones:
  general/<YYMMDD-HHMM>-<your-name>.md , with header fields: from / to / subject

NEVER edit someone else's file.
WRITE ONLY TO THE SHARED DIRECTORY, never inside your working tree: a file inside your
tree is invisible to everyone else, and you will have no way of knowing it.

READ AND UPDATE YOUR STATUS at natural checkpoints. There are no push notifications.
IF YOU ARE BLOCKED OR FINISHED — send a message and update your roster. Never exit silently.
```
