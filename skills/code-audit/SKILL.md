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

Field-tested engineering system for **starting/planning projects**, **maintaining live cross-session documentation**, and **auditing codebases** across **Web**, **Fullstack/Backend**, **Mobile/Android**, and **Desktop GameDev / 3D**.

## Skill & Role File Map (Always Use Absolute Paths)
* **Main Skill**: `~/.gemini/config/skills/code-audit/SKILL.md`
* **Cross-Project Craft Memory**: `~/.gemini/config/skills/code-audit/memory/craft.md`
* **Per-Project Memory Directory**: `~/.gemini/config/skills/code-audit/memory/projects/` (`COLONY-REALMS.md`, `lernoroo.md`, etc.)
* **Project Passport Template (Procedure P-0)**: `../../templates/PROJECT.md`
* **Multi-Session Board Templates (Optional)**: `../../templates/board/00-START.md` · `../../roles/LEAD-PROCEDURES.md`
* **Historical Case Studies**: `~/.gemini/config/skills/code-audit/references/PLAYBOOK.md`
* **Deep Role Playbooks (Read when executing a specific role lens or planning stage)**:
  - **Architect**: `../../roles/ARCHITECT.md`
  - **Cybersecurity**: `../../roles/SECURITY.md`
  - **Analyst**: `../../roles/ANALYST.md`
  - **Designer (UI/UX & 3D)**: `../../roles/DESIGNER.md`
  - **Tester (Gate Engineer)**: `../../roles/TESTER.md`
  - **Device / Runtime QA**: `../../roles/DEVICE.md`
  - **Junior (Completeness `N of N`)**: `../../roles/JUNIOR.md`
  - **Executor & Team Lead**: `../../roles/EXECUTOR.md` · `../../roles/LEAD.md`

---

## 0. Three Operating Modes & Guardrails

### Which Mode to Run
* **Mode 1 — Project Bootstrap & Planning (`P-0 Bootstrap & Architecture/Security-by-Design`)**:
  Triggered when starting a new app, onboarding an existing app, or planning a major feature:
  1. Read `../../templates/PROJECT.md`, `ARCHITECT.md`, `SECURITY.md`, and `DESIGNER.md`.
  2. Define the application profile (`Prof-1..Prof-4`), clean-core layer boundaries, data ownership, security threat model (`Zero-Leaks`, AuthN/RLS/BOLA or save-file safety), UI states, and verification gates **before writing code**.
  3. Create/update `<project>/PROJECT.md` (in the project root for incoming sessions) AND `~/.gemini/config/skills/code-audit/memory/projects/<project>.md` (persistent skill memory). If running parallel sessions, also initialize `<project>/board/00-START.md` per `LEAD-PROCEDURES.md` (Procedure `P-0`).
* **Mode 2 — Feature Development with Live Documentation (`Build & Live Handover`)**:
  Triggered when implementing features under `ai-team` / `code-audit`:
  1. Read `<project>/PROJECT.md` and `memory/projects/<project>.md` first so you never violate product invariants or layer boundaries.
  2. Implement changes across all required legs/siblings (Section 3.A) + write/update the test gate and verify it fails on broken input (Mutation Check).
  3. Update `<project>/PROJECT.md` (if architecture, gates, or generators changed) and `memory/projects/<project>.md` so the next session has an exact, up-to-date map.
* **Mode 3 — Full or Role-Targeted Audit / Review (`Audit & Review`)**:
  Triggered on audit, code review, or verification requests:
  - If the user asks for a **full application audit**, inspect the project through **all applicable Role Lenses** (1. Analyst, 2. Architect, 3. Security, 4. Designer, 5. Tester, 6. Device/Runtime QA, 7. Junior-Completeness).
  - If the user asks for a **specific role** (e.g. Architect + Security), load those full playbooks from `../../roles/` and run Steps 1–6 deeply for those roles.

### Anti-Harm & Hygiene Guardrails
1. **Read-Only First During Audits (Do Not Fix Mid-Audit)**: Fixing the first bug you see destroys audit independence (*"the fixer ends up reviewing their own work"*). In Mode 3, complete Steps 1–5 in strictly **read-only / measurement mode** and present the full report before editing production code.
2. **Proportionality**: Match audit depth to change risk — do not run a heavy full-repo audit on a one-line text tweak unless asked.
3. **Application-Type Isolation**: Select ONLY the Application-Type Module(s) in Section 4 that match the target project (`Web`, `Fullstack/Backend`, `Mobile`, or `Desktop GameDev/3D`).
4. **Resource & Disk Hygiene**:
   - Never run multiple heavy test suites or builds (`pnpm test`, `dotnet test`, Gradle) in parallel.
   - Never create `.claude/worktrees` or leave temporary probe scripts in the project repository. Write one-off probe scripts only in the session `scratch/` directory (`<appDataDir>/brain/<conversation-id>/scratch/`), never in `/tmp` (volatile `tmpfs`).
5. **Independent Subagent Review**: When auditing code written in the current session, delegate the read-only audit via `invoke_subagent` (`research` or `self`) so the reviewer has fresh context and is not defending its own edits.

---

## 1. Step-by-Step Execution Algorithm (6 Steps)

### Step 1: Load Memory, Application Type & Role Lens ("Entry & Memory")
1. Read `~/.gemini/config/skills/code-audit/memory/craft.md` and `~/.gemini/config/skills/code-audit/memory/projects/<project-name>.md` (if present).
2. Check `git status` and `git diff` (`git log` shows only committed state, not live uncommitted files).
3. Select the active **Role Lens(es)** (Section 2 — reading the corresponding full playbook from `../../roles/<ROLE>.md` when deep domain checks are needed) and **Application-Type Profile(s)** (Section 4).
4. Ask: **"Which seams (between modules/layers) or matrix cells has nobody looked at yet?"**

### Step 2: Baseline Measurement, Zero-Leaks & Gate Inventory ("Measure Before")
1. Treat every number or claim in docs/reports/memory as unverified until reproduced by a command. If a recorded number in memory no longer reproduces, mark it `OBSOLETE` in Step 6.
2. Run the **Zero-Leaks Security Check** (Section 3.B): verify no secrets, `.pem` keys, or `SERVICE_ROLE_KEY` are exposed in git status, client bundles, or logs.
3. Check that test files and gate scripts are actually registered in `package.json` / `.sln` / CI runners (an unlisted test file is 0% coverage).
4. Run checks sequentially without masking exit codes (never run `gradlew ... | tail` without `set -o pipefail`).

### Step 3: Seam, Architecture, Security & Neighbor Inspection
Ask: **"What shape would a defect or attack have so that the current rule or test fails to see it?"**
Inspect target areas against:
* **Role Lenses** (Section 2)
* **Universal Defect Shapes, Architectural Laws & Zero-Leaks Rules** (Section 3)
* **Application-Type Module(s)** (Section 4)

### Step 4: Probe & Mutation Verification ("Verify the Probe")
1. **Suspect Round & Huge Numbers**:
   - `0 of N` or `N of N` almost always means a broken probe/regex, not clean data. Feed a **known-broken input** to prove the detector can fail.
   - Huge numbers (`3015 of 4094`) mean the probe is answering the wrong question — inspect 3–5 sample records before reporting.
2. **Second Independent Path**: Confirm critical counts from two independent sources (e.g., UI showcase vs. database/registry).
3. **Mutation Check**: Temporarily break the guarded condition (or run against pre-fix data) and confirm the test/gate turns **red (`exit != 0`)**.

### Step 5: Structured Report Output
Format the audit response strictly using the 4-block schema in Section 5.

### Step 6: Update Project & Craft Memory ("Record Experience")
Update `~/.gemini/config/skills/code-audit/memory/projects/<project-name>.md` or `craft.md` following Section 6.

---

## 2. Role Lenses (7 Specialized Perspectives from `ai-team`)

Apply the specific lens requested by the user (reading its full playbook in `../../roles/`), or combine **Analyst + Architect + Security + Tester** for a comprehensive audit:

| Role Lens | Core Question | What It Checks & How It Proves |
|---|---|---|
| **1. Analyst** ([ANALYST.md](../../roles/ANALYST.md)) | *"Are they measuring the right thing?"* | Checks whether metrics/code solve the right domain problem; inspects module seams and unopened matrix cells. Proves with reproducible commands and 2 independent paths. |
| **2. Architect** ([ARCHITECT.md](../../roles/ARCHITECT.md)) | *"Is everything in its right layer and how are boundaries wired?"* | Checks the 5 Universal Architectural Laws (Section 3.A): field readers/writers in prod, clean core dependency direction, single source of truth, import/route weight, and cancellation/cleanup lifecycle. |
| **3. Security** ([SECURITY.md](../../roles/SECURITY.md)) | *"Where are user data, keys, or the system exposed?"* | Enforces Zero-Leaks (Section 3.B) and profile-specific AppSec: BOLA/IDOR & RLS, server-side authority, injection/XSS/SSRF defense, API cost/rate limits, mobile bridge/keystore, and safe save-file deserialization. |
| **4. Designer** ([DESIGNER.md](../../roles/DESIGNER.md)) | *"What does the user actually see and understand?"* | Checks all **5 screen states** (loading, empty, populated, error, unavailable), **exit-and-return** mid-flow, error feedback, token/palette consistency, contrast (APCA/WCAG on real pixels), thumb reach ($\ge 44\text{ px}$), and 3D scene/HUD readability. |
| **5. Tester** ([TESTER.md](../../roles/TESTER.md)) | *"What do tests actually prove and where are they silent?"* | Audits the test layer itself: unlisted test files, gates weaker than builders, mirror gates, and mandatory mutation testing (break code -> test must fail). |
| **6. Device / Runtime QA** ([DEVICE.md](../../roles/DEVICE.md)) | *"Does the live product work, not just the test suite?"* | Verifies live execution: Android emulator (`run35`), browser, or live Godot window (`2560x1440`). Checks upgrade-over-existing-data without wipe, orientation/resolution changes, and full user/gameplay loops. |
| **7. Junior — Completeness** ([JUNIOR.md](../../roles/JUNIOR.md)) | *"Was the decision applied EVERYWHERE (0 omissions)?"* | When a decision/fix is already made, enumerates all $N$ occurrences/locales/screens/models and verifies every single one is updated (`N of N` listed explicitly). Stops immediately if a case requires new judgment. |

---

## 3. Universal Laws: Defects, Architecture & Zero-Leaks (All Projects)

### A. Seven Universal Defect & Architectural Laws
1. **Law of the Neighboring Variable (#1 Most Frequent)**:
   - When a fix touches one variable/field/method (`levelCount`, `score`, track filter, save field), **immediately inspect sibling variables/methods** in the same file or state object (`runsAt`, `answered` flag, locale filter, load field).
2. **Field Lifecycle: Built & Unconnected / Written, Never Read**:
   - Trace `Producer -> Storage/Contract -> Consumer in PRODUCTION code (not just tests)`. A function/test with `0` prod callers or a field written by the pipeline and never read by the app is a defect.
3. **Clean Core Dependency Direction**:
   - Domain logic and simulation cores must never import UI components, routers, HTTP frameworks, or Godot presentation nodes (`Node3D`/`Control`). Dependencies flow inward: `UI/Scene/Controller -> Application Service -> Domain Core`.
4. **Single Source of Truth & Two Axes with One Name**:
   - Never manually sync two fields when one can be derived from the other. Never confuse two domain axes sharing one name (`level` of a word vs. `level` of a lesson; key count vs. rendered item count; incompatible scales `6..10` vs `43..100`).
5. **Blast Radius, Route Weight & Cancellation Lifecycle**:
   - Check that barrel imports (`index.ts`) do not pull megabytes of data into every screen, and verify that exiting a screen mid-operation cancels timers/requests (`AbortController`/`CancellationToken`) and unsubscribes listeners/signals.
6. **Guard on the Wrong Axis / Mirror Gate**:
   - A gate checks policy on one dimension while empty/broken state enters via missing fields on another dimension, or the gate measures the output of its own filter.
7. **Defect as a Fuse & Dead Field Types**:
   - Before removing "weird" logic or "unused" fields, check whether another module relies on it to prevent a worse failure, and distinguish between a required schema field vs. an optional field recording a design decision.

### B. Universal Zero-Leaks Security Checklist (Every Project)
1. **No Secrets in Code or Git**: Check `.gitignore` and scan for private keys (`BEGIN PRIVATE KEY`, `.pem`, `.keystore`), `SERVICE_ROLE_KEY`, `DATABASE_URL`, `OPENAI_API_KEY`, `ELEVENLABS_API_KEY`, Stripe/Clerk secrets.
2. **No Server Secrets in Client Bundles**: Verify that server keys never use client prefixes (`NEXT_PUBLIC_`, `VITE_`, `EXPO_PUBLIC_`, `PUBLIC_`) and are never transitively imported into browser/mobile bundles.
3. **No PII/Tokens in Prod Logs or Error Responses**: Confirm `console.log`/loggers do not print auth tokens, passwords, or full user storage dumps, and `500` API responses do not leak stack traces or SQL structure to clients.
4. **Clean Release Artifacts (`.apk`, `.pck`, `dist/`)**: Ensure release builds exclude `.env`, debug flags (`android:debuggable="false"`), test backdoors, and raw asset archives (`my_models/`).

---

## 4. Application-Type Audit Modules (Select by Project Type)

### Profile A (`Prof-1`): Web Frontend & Static/Content Apps (`lernoroo-web`, `3d-portfolio`, `confidently-online-landing`)
* **Typecheck + Test Parity**: Babel/SWC test runners (`pnpm test`) do NOT check TypeScript types. Always run both `tsc --noEmit` (`pnpm typecheck`) and `pnpm test`.
* **Composite Identity Loss**: `new Map(items.map(x => [x.id, x]))` silently overwrites records when true identity is composite (`course|track|id`).
* **Unicode & `\b` Regex Trap**: In JS, `\b` is ASCII-only (`[A-Za-z0-9_]`). Regexes using `\b` silently return `0` matches on non-ASCII scripts and German umlauts (`ä`, `ö`, `ü`, `ß`).
* **WebGL / `Three.js` Memory & UI States**: Ensure `dispose()` is called on geometries, materials, textures, and renderers on unmount; check all 5 UI states and pixel contrast over gradients/blur.

### Profile B (`Prof-2`): Fullstack, Backend API & Database Apps (`exam-prep`, `geo-rpg-prototype` with Supabase/Cloudflare/Node)
* **Layer Architecture & Contract Parity**: Verify clean separation (`Controller/Route -> Service/Use-Case -> Repository/DB`), matching DTO schemas across client/server/SQL, and Expand-and-Contract DB migrations.
* **AuthN, BOLA / IDOR & Row-Level Security (RLS — #1 API Risk)**:
  - Verify tokens are cryptographically verified (`jwt.verify`, never `decode` alone) and cookies use `HttpOnly; Secure; SameSite`.
  - On every endpoint and every Supabase `public` table, test: *"Can User A read/modify User B's data by passing User B's `id`?"* Confirm `ENABLE ROW LEVEL SECURITY` and policies checking `auth.uid() = user_id`, and ensure user routes never use `service_role` to bypass RLS.
* **Zero Trust to Client & Race Conditions**: Server must compute scores/XP/currency/access itself (never trust client payload fields like `score` or `role`), validate inputs with strict schemas (`zod` stripping unknown fields to block Mass Assignment), and use transactions / atomic updates to prevent double-spend race conditions.
* **Injection, SSRF & Denial-of-Wallet Defense**:
  - Parameterized SQL only; sanitize `dangerouslySetInnerHTML` / `javascript:` URLs; whitelist hosts/paths on server fetches/file reads.
  - Enforce **Rate Limiting**, payload size caps, and timeouts on paid AI/audio/email endpoints, and verify HMAC signatures on webhooks.

### Profile C (`Prof-3`): Mobile & Android Emulator (`lernoroo-mobile`, `exam-prep/apps/mobile`, AVD `run35`)
* **Stale APK / Bundle Trap**: Verify `npx cap sync` / bundle packaging and fresh APK installation on `run35` (check install timestamp or `base.apk` md5) so you never test a stale build.
* **Irreversible Storage Migration (Install-Over-Old)**: User progress keys must never be renamed without a migration. Test installing the new build **over** an existing populated install without `pm clear`.
* **Mobile AppSec (Keystore, WebView Bridge & Deep Links)**:
  - Store auth tokens in `Keystore`/`Keychain`/`EncryptedSharedPreferences`, never plaintext `localStorage`.
  - Check `capacitor.config` `allowNavigation` (never expose native bridge to external domains), enforce `usesCleartextTraffic="false"`, verify `apksigner verify --print-certs`, and check `intent-filter` deep-link hijacking.
* **Viewport, Safe Areas & Soft Keyboard**: Check portrait + landscape, small screens, gesture insets, thumb reach ($\ge 44\text{ dp}$), exit-and-return mid-lesson, and ensure the soft keyboard never covers inputs/buttons. Launch `run35` with `-gpu host` if SwiftShader crashes.

### Profile D (`Prof-4`): Desktop GameDev & 3D (`COLONY-REALMS` on Godot 4 Mono C#, `geo-rpg-prototype`, `3d-portfolio`)
* **Simulation Core vs. Godot Presentation Separation**: Keep simulation rules, economy, combat, and `WorldSnapshot` in pure C# classes decoupled from `Node3D`/`Control` so simulation ticks are deterministic and testable headlessly.
* **C# Build vs. Godot Scene Integrity**: Green `dotnet build` / `dotnet test` does NOT prove Godot scenes load. After renaming/moving C# scripts, classes, or nodes, verify `.tscn` / `.tres` `ext_resource` paths, `uid://` IDs, `GetNode<T>()` paths, `[Export]` fields, and connected signals.
* **4-Leg Simulation & Safe Save Deserialization (`WorldSnapshot`)**:
  - Audit all 4 legs on any state change: `1. Init/Spawn -> 2. Simulation Tick -> 3. Save/Load Serialization & Validation -> 4. HUD/View (UnitsView)`.
  - **Save-File Security**: Never use unsafe deserializers (`BinaryFormatter`, `TypeNameHandling.All`) or load untrusted `.tres`/`.res` (which can execute embedded scripts). Validate all loaded bounds (no negative resources, `NaN`/`Infinity` coordinates, invalid enums, or huge array sizes causing OOM/crash on load).
* **Frame Budget, Lifecycle & 3D Tech Art Hygiene**:
  - Check `GodotObject.IsInstanceValid()` across async/signal boundaries; avoid per-frame allocations/LINQ/`FindChild` inside `_Process`/`_PhysicsProcess`; use object pooling.
  - Before wiring Meshy/Blender `.fbx`/`.glb` assets into gameplay scenes, check polycount/LODs, texture sizes, scale/pivot consistency, collision shapes, silhouette readability against terrain, and HUD `mouse_filter` raycast blocking.

---

## 5. Mandatory Audit Report Schema (4 Blocks)

### 1. Findings (4 Mandatory Parts per Finding)
* **What Is Observed (Location & Vector)**: `file:line` (or `route/table/scene`) and the exact symptom, architectural drift, or attack vector.
* **How It Was Measured (Proof)**: exact command, query, import graph, output/count, or captured frame.
* **Consequence (Cost / Risk)**: data leak, privilege escalation, lost progress, frame stutter, state desync, or tech debt.
* **Defect Shape & Fix**: pattern/vulnerability name and concrete fix + how to verify it via mutation.

### 2. Verified Clean (Checked & Clean, and Why)
* List checked seams/files/endpoints/cells that are **not** defects and why (including dated intentional design/architecture decisions).

### 3. Broken / Rejected Probes (Negative Instrument Results)
* Document any metric, regex, or script that gave a misleading number during the audit (*what was measured -> why the probe lied -> what replaced it*).

### 4. What Was NOT Verified (Blind Spots)
* Explicitly state what this audit did **not** cover (e.g., *"Verified C# compilation and `WorldSnapshot` validation, but did not verify live scene rendering in the Godot window"*).

---

## 6. Memory Protocol (`memory/craft.md` & `memory/projects/<project>.md`)

Memory lives in `~/.gemini/config/skills/code-audit/memory/` so project git repositories stay 100% clean.

1. **Where to write**:
   - True across all projects -> `~/.gemini/config/skills/code-audit/memory/craft.md`
   - Specific to one project -> `~/.gemini/config/skills/code-audit/memory/projects/<project-name>.md`
2. **Entry Format**:
   ```markdown
   ### Short Reference Title
   **Status:** active · YYYY-MM-DD
   **What:** one line — rule, hazardous seam, trust boundary, intentional decision, or rejected probe.
   **Why / Incident:** measurement or incident from which this was derived.
   **Outcome / How to Apply:** concrete action to take in practice.
   ```
3. **Deprecation Rule**: Never silently erase outdated entries; mark them `**Status:** OBSOLETE since YYYY-MM-DD — <what changed and why>`.
