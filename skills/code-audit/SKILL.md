---
name: code-audit
description: >-
  Multi-stack project bootstrap, architecture/security planning, live documentation, and
  deep code/UI/security/test-gate audit methodology distilled from AI-Team. Use when starting
  or planning a project ("bootstrap with ai-team", "plan architecture", "initialize project"),
  developing features with strict architectural/security rules, or auditing/reviewing apps
  across Web Frontend, Fullstack/Backend+DB, Mobile/Android, and Desktop GameDev / 3D
  (Godot 4 Mono / C# / Three.js). Triggers on requests like "check code", "run audit",
  "review", "why are tests green when a bug exists", "check with code-audit", "check with ai-team",
  "ai-team", "start with ai-team", "find hidden bugs", "review as architect",
  "review as designer", "check security", "verify completeness".
---

# Multi-Stack Architecture, Security, Bootstrap & Audit (`code-audit` / `ai-team`)

Portable, field-tested engineering skill distilled from **AI-Team** for **starting/planning projects**, **maintaining live documentation**, and **auditing codebases** across **Web**, **Fullstack/Backend**, **Mobile**, and **Desktop GameDev / 3D**.

## Charter & Role Map (Relative to `ai-team/` Root)
* **Lite Starter Template**: `../../templates/AGENTS.md`
* **Project Passport Template (Procedure P-0)**: `../../templates/PROJECT.md`
* **Multi-Session Board Templates (Optional)**: `../../templates/board/00-START.md` · `../../roles/LEAD-PROCEDURES.md`
* **Defect Shapes & Probe Traps**: `../../PLAYBOOK.md`
* **Deep Role Playbooks (load on demand for a specific role lens)**:
  - **Architect**: `../../roles/ARCHITECT.md`
  - **Security**: `../../roles/SECURITY.md`
  - **Analyst**: `../../roles/ANALYST.md`
  - **Designer (UI/UX & 3D)**: `../../roles/DESIGNER.md`
  - **Tester (Verification Engineer)**: `../../roles/TESTER.md`
  - **Runtime / Device QA**: `../../roles/DEVICE.md`
  - **Junior (Completeness `N of N`)**: `../../roles/JUNIOR.md`
  - **Executor & Team Lead**: `../../roles/EXECUTOR.md` · `../../roles/LEAD.md`
* **Optional Persistent Memory**: store cross-session notes either in `<project>/memory/` (versioned with the project) or in your local agent skill memory directory (`<skill-dir>/memory/craft.md` and `<skill-dir>/memory/projects/<project>.md`), keeping the `ai-team` framework repo 100% clean.

---

## 0. Three Operating Modes & Guardrails

### Which Mode to Run
* **Mode 1 — Project Bootstrap & Planning (`P-0 Bootstrap & Architecture/Security-by-Design`)**:
  1. Read `../../templates/PROJECT.md`, `../../templates/AGENTS.md`, `ARCHITECT.md`, `SECURITY.md`, and `DESIGNER.md`.
  2. Define the application profile (`Prof-1..Prof-4`), clean-core layer boundaries, data ownership, threat model (`Zero-Leaks`, AuthN/AuthZ/IDOR or save-file safety), UI states, and verification gates **before writing code**.
  3. Create `<project>/AGENTS.md` and `<project>/PROJECT.md` (plus `<project>/board/00-START.md` only if running multi-session Full Fleet Mode).
* **Mode 2 — Feature Development with Live Documentation (`Build & Live Handover`)**:
  1. Read `<project>/PROJECT.md` and `AGENTS.md` first so you never violate product invariants or layer boundaries.
  2. Implement vertical slices cleanly across all sibling variables/layers (Section 3.A) + add/update tests and verify each test fails on broken input (Mutation Check).
  3. Update `<project>/PROJECT.md` (or `docs/PROJECT_STATUS.md`) if architecture, commands, or contracts changed.
* **Mode 3 — Full or Role-Targeted Audit / Review (`Audit & Review`)**:
  - For a **full application audit**, inspect the codebase through all applicable Role Lenses (Analyst, Architect, Security, Designer, Tester, Runtime QA, Junior-Completeness).
  - For a **targeted role review** (e.g., Architect + Security), load those full playbooks from `../../roles/` and run Steps 1–6 deeply.

### Anti-Harm & Hygiene Guardrails
1. **Strictly Read-Only During Audits:** Never fix the first bug mid-audit — whoever fixes code loses independence and reviews their own assumptions. Complete Steps 1–5 read-only and report first.
2. **Proportionality:** Match audit depth to risk — do not run a full-repo audit on a one-line text fix unless requested.
3. **Profile Isolation:** Apply **only** the Application-Type Module(s) in Section 4 matching the target project (`Prof-1..Prof-4`).
4. **Resource & Repo Hygiene:**
   - Never run multiple heavy builds or full test suites (`pnpm test`, `dotnet test`, Gradle, Docker builds) concurrently.
   - Never leave temporary probe scripts inside the target repository or volatile `/tmp`.
5. **Independent Subagent Review:** When auditing code written in the current session, delegate read-only review to a fresh subagent or independent CLI reviewer so it is not defending its own edits.

---

## 1. Step-by-Step Execution Algorithm (6 Steps)

1. **Entry & Scope:** Check `git status` and `git diff` (`git log` omits uncommitted work). Select active Role Lens(es) and Profile(s). Identify unopened seams and matrix cells.
2. **Baseline & Zero-Leaks:** Reproduce all cited numbers with live commands; run Zero-Leaks checks (Section 3.B); confirm test files & gates are actually registered in runners/CI; never mask build exit codes with pipes (`cmd | tail`).
3. **Seam, Architecture, Security & Sibling Inspection:** Ask *"What shape would a defect or attack have so current rules/tests miss it?"* Apply Sections 2, 3, and 4.
4. **Probe & Mutation Verification:**
   - Distrust round (`0 of N`, `N of N`) and huge (`>70%`) numbers — feed a known-broken input and read actual records.
   - Confirm critical counts via a second independent path.
   - Prove guards work via mutation (temporarily break the condition → check must exit non-zero).
5. **Structured Report Output:** Report using the 4-block schema in Section 5.
6. **Persist Lessons:** Record durable cross-project or project-specific insights in memory using Section 6 (marking stale entries `OBSOLETE`, never erasing silently).

---

## 2. Role Lenses (7 Specialized Perspectives)

| Role Lens | Core Question | Proof Instrument |
| --- | --- | --- |
| **1. Analyst** (`ANALYST.md`) | *"Are they counting the right thing?"* | Reproducible commands, seam checks, 2 independent paths |
| **2. Architect** (`ARCHITECT.md`) | *"Is everything in its right layer?"* | Import graph, field prod readers, bundle/query cost |
| **3. Security** (`SECURITY.md`) | *"Where are data, keys, or APIs exposed?"* | Attack/data-loss path, config/RLS/IDOR proof, bundle scan |
| **4. Designer** (`DESIGNER.md`) | *"What does the user see & experience?"* | 5 screen states, live flows, measured contrast, a11y, screenshots |
| **5. Tester** (`TESTER.md`) | *"What do checks actually prove?"* | Runner inventory + mutation checks (break code → red) |
| **6. Runtime / Device QA** (`DEVICE.md`) | *"Does the live product work?"* | Live browser/device/window walkthrough + filled matrix |
| **7. Junior — Completeness** (`JUNIOR.md`) | *"Applied EVERYWHERE (`N of N`)?"* | Exhaustive place list verified 100%; stops on ambiguity |

---

## 3. Universal Laws: Defects, Architecture & Zero-Leaks

### A. Seven Universal Defect & Architectural Laws
1. **Law of the Neighboring Variable (#1 Most Frequent):** Whenever a fix updates one variable/field/filter, immediately inspect sibling variables in the same file/model.
2. **Field Lifecycle (Written, Never Read / Built, Never Wired):** Trace `Producer → Storage/Contract → Production Consumer`. Zero production callers or an unregistered test file is a defect.
3. **Clean Core Dependency Direction:** Domain logic/simulation cores never import UI, routers, HTTP frameworks, or engine presentation nodes (`UI/Controller → Service → Domain Core`).
4. **Single Source of Truth & Two Axes with One Name:** Never manually sync derived state; never confuse distinct concepts sharing an identifier (`level`, `status`, `id`) or incompatible numeric scales.
5. **Blast Radius & Cancellation Lifecycle:** Prevent barrel files (`index.ts`) from pulling heavy assets/server code into unrelated routes; cancel in-flight requests/timers (`AbortController`/`CancellationToken`) and unsubscribe listeners on exit.
6. **Guard on Wrong Axis / Mirror Gate:** Watch for checks guarding one dimension while broken state enters via another, or gates measuring their own filter output.
7. **Defect as a Fuse & Dead Field Types:** Before deleting "unused" logic/fields, verify nothing relies on it as an accidental guard, and distinguish required schema fields from optional human-decision metadata.

### B. Universal Zero-Leaks Security Checklist (Every Project)
1. **No Secrets in Code or Git:** Verify `.gitignore` covers `.env*` (except empty `.env.example`), `*.pem`, `*.key`, `*.keystore`; scan tracked files for private keys, DB URLs, service-role tokens, and API secrets.
2. **No Server Secrets in Client Bundles:** Server secrets must never use client prefixes (`NEXT_PUBLIC_`, `VITE_`, `EXPO_PUBLIC_`, `PUBLIC_`) or be transitively imported into browser/mobile bundles.
3. **No PII/Tokens in Logs or Error Responses:** Loggers must not print auth headers/cookies/passwords; `500` responses return safe error codes + `requestId`, never stack traces or raw SQL.
4. **Clean Release Artifacts:** Verify packaged artifacts (`.apk`, `.aab`, `.pck`, `dist/`) exclude `.env`, debug flags (`debuggable="false"`), debug backdoors, and raw asset warehouses.

---

## 4. Application-Type Audit Modules (Select Only Active Profile)

### `Prof-1`: Web Frontend & Static/Content Apps
* Run both static typecheck (`tsc --noEmit`) and test runner (Babel/SWC runners skip types).
* Watch for composite identity loss (`new Map(items.map(x => [x.id, x]))`) and JS ASCII-only `\b` regex traps on Unicode/non-ASCII text.
* For WebGL/`Three.js`, verify explicit `dispose()` on unmount; check all 5 UI states and real pixel contrast over gradients/blur.

### `Prof-2`: Fullstack, Backend API & Databases
* Verify strict server layers (`Route/Controller → Service → Repository`), end-to-end `Client ↔ API ↔ DB` DTO contracts, and Expand-and-Contract migrations.
* **AuthN, BOLA/IDOR & Multi-Tenancy (#1 API Risk):** Cryptographically verify tokens/sessions server-side; enforce `HttpOnly; Secure; SameSite` cookies; derive `userId`/`organizationId` solely from verified server context; test cross-user/cross-tenant ID substitution on every endpoint; verify RLS policies where used.
* **Zero Trust & Concurrency:** Compute sensitive values server-side; validate bodies with strict schemas stripping unknown fields (anti–Mass Assignment); protect multi-step writes/quotas with DB transactions and idempotency.
* **Injection, SSRF, Paid APIs & AI Safety:** Parameterized SQL only; sanitize raw HTML/URLs; allowlist server fetches/paths; enforce rate limits, payload limits, timeouts, spending caps, webhook signature verification, and server-enforced confirmation for AI write actions.

### `Prof-3`: Mobile Applications (Android / iOS / Cross-Platform)
* Confirm installed build freshness (artifact hash/timestamp) before testing so stale bundles never mislead debugging.
* Test irreversible upgrade-over-existing-data migrations without wiping storage (`adb install -r`, no `pm clear`).
* Verify secure token storage (`Keystore`/`Keychain`), locked-down WebView bridge allowlists, `usesCleartextTraffic="false"`, release signing certs (`apksigner`), deep-link hijack defenses, and portrait/landscape/keyboard layouts.

### `Prof-4`: Desktop Games & 3D Simulations (Godot 4 / C# / Engines)
* Keep simulation core decoupled from scene/presentation nodes for deterministic headless testing.
* Verify compiled code against scene/resource references (`.tscn`/`.tres`, `uid://`, node paths, exported fields, signals) — builds alone miss broken scene links.
* Audit 4-leg state parity (`Init/Spawn → Tick → Save/Load + backward compatibility → HUD/View`) and reject unsafe deserializers (`BinaryFormatter`, untrusted text resources); bounds-check loaded save values against `NaN`/`Infinity`/OOM crashes.
* Check frame-loop allocations/GC spikes, instance validity across async boundaries, 3D asset scale/pivot/LODs/collisions, terrain silhouette readability, and HUD input blocking.

---

## 5. Mandatory Audit Report Schema (4 Blocks)

1. **Findings (4 Parts Each):** (a) Location & Vector (`file:line` / route / table / scene) · (b) Reproducible Proof (command/query/output/frame) · (c) Concrete Impact/Risk · (d) Defect Shape, Fix & Mutation Verification.
2. **Verified Clean (and Why):** Checked areas confirmed clean, including dated intentional decisions.
3. **Broken / Rejected Probes:** Any misleading check/metric discarded during the audit (*what → why it lied → replacement*).
4. **What Was NOT Verified (Blind Spots):** Explicitly state every uninspected layer, viewport, or runtime surface.

---

## 6. Memory Entry Protocol

Store cross-project patterns in `craft.md` and project-specific invariants/seams in `projects/<project>.md`:

```markdown
### Short Reference Title
**Status:** active · YYYY-MM-DD
**What:** one line — rule, hazardous seam, trust boundary, or rejected probe.
**Why / Incident:** measurement or incident from which this was derived.
**Outcome / How to Apply:** concrete action to take in practice.
```

Never silently erase outdated entries — mark them `**Status:** OBSOLETE since YYYY-MM-DD — <what changed and why>`.
