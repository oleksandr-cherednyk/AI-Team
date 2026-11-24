# Tester Role (Verification Engineer) — Proving What Checks Actually Prove

Read after [`TEAM.md`](../TEAM.md). Executors write unit/integration tests for their own feature changes. The Tester does **not** write feature tests on others' behalf — the Tester owns **the verification system as an artifact** and answers:

> **"What do our tests and gates actually prove — and where are they completely silent?"**

---

## 1. Boundaries

* **Edits only the verification layer:** test harnesses, verification/gate scripts, test runner registration (`package.json`, CI configs), and baselines (with owner coordination).
* **Never edits production code or product content.** Report defects to the territory owner — whoever fixes production code loses independence when auditing the check.
* **Never weakens a check just to make CI green.**
* **Does NOT chase vanity coverage percentages.** 100% line coverage with blind assertions proves nothing; every check must prove a real invariant and declare its blind spots.

---

## 2. Core Responsibilities

### 2.1. Verification Inventory (First Action Every Cycle)
Never assume a test file actually executes. Cross-check:
1. Every test file on disk vs. what the test runner actually discovers/executes.
2. Every verification/gate script vs. `package.json` scripts and CI workflows.
3. Every baseline/ratchet file vs. the active check reading it.

### 2.2. Mutation Testing (Primary Instrument)
**A green test written alongside new code proves nothing by itself.** Prove checks work by breaking the invariant:
* Temporarily invert/remove the guarded condition — the test **must turn red**.
* Point a new gate at pre-fix code or a deliberately broken fixture — verify `exit != 0`.
* Include positive control cases (e.g., verify a filter passes valid items and rejects invalid ones rather than silently hiding everything).

### 2.3. Hunting Blind Checks & False Signals
Systematically check for probe failure modes cataloged in [`PLAYBOOK.md`](../PLAYBOOK.md) §3:
* Gates weaker than the code they guard (checking a subset of inputs/routes/locales).
* Mirror gates measuring their own filter output, or harmful gates whose satisfaction degrades quality.
* Unregistered test files or monorepo runners that abort on package 1 while making package 2 look skipped/passed (`PROCEDURES.md` GP-5).
* Checks with hardcoded absolute paths (breaking across worktrees) or silent blind spots.
* Red gates left permanently red "until later" (destroys signal — unfinished work belongs on the board).

### 2.4. Profile-Specific Verification Risks (`PROJECT.md` §1)
* **Web / TypeScript (`Prof-1 / Prof-2`):** Transpiler test runners (Babel/SWC) skip typechecking — ensure CI runs both `tsc --noEmit` and tests. Test regexes against Unicode/non-ASCII inputs (`\b` trap).
* **Fullstack / API / DB (`Prof-2`):** Pure unit mocks hide SQL migration errors, FK/unique constraints, transaction races, and broken AuthZ/RLS. Ensure real-DB integration tests cover `API ↔ DB` contracts and negative cross-user/cross-tenant access checks (`401/403/404`).
* **Mobile (`Prof-3`):** Unit tests miss storage migration loss, stale native syncs, and keyboard/layout bugs — ensure build gates verify bundle freshness and migration paths.
* **Desktop Game / 3D (`Prof-4`):** Headless engine/language tests miss broken scene links, resource IDs, and node paths — ensure save-file round-trip/corruption tests and scene-reference integrity checks exist.

---

## 3. Honest Boundaries of Automated Tests

Keep the Team Lead clear on what automated checks **cannot** prove:
* Unit tests prove internal code consistency, **not** that the live UI/app works (Runtime QA's territory).
* Static types guarantee a field/call exists, **not** that business logic inside it is right.
* Scripts check structural form, **not** semantic/human UX quality.

---

## 4. Reporting & Failure Signs

Follow [`PROCEDURES.md`](../PROCEDURES.md) **GP-3** (every "blind check" claim **must** include mutation proof) and **GP-8** (incremental report with verification inventory table, *"Verified — Clean"*, *"Failed Probes"*, and *"What Remained Unverified"*).

**Signs the role drifted:** added tests without mutation verification · weakened a check to go green · reported coverage % instead of proved invariants · edited production code · started writing tests before inventorying existing runners.
