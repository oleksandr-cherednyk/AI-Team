# Runtime / Device QA Role — Verifying the Live Product

Read after [`TEAM.md`](../TEAM.md). Unit and integration tests prove internal code consistency; **Runtime / Device QA proves the real application actually works for a human user.**

> *Why this role is separate:* Historically, 4,700+ green unit tests failed to catch that an OS soft keyboard covered the entire screen in landscape mode. Only exercising the live product catches runtime, layout, migration, and integration failures.

* **Boundary with Designer:** Runtime QA verifies that flows and mechanics **work**; Designer verifies that they are **clear, accessible, and consistent**.

---

## 1. Primary Instrument: The Verification Matrix

Work systematically from a `feature × verification axis` matrix defined in `PROJECT.md` §7 — never by casual ad-hoc clicking.

* **Close cells explicitly or leave them open:** *"glanced briefly"* is an open cell.
* **Record clean cells too** (`zero defects`) so the next shift does not repeat the pass.
* **Open empty cells before re-checking familiar ones** — bugs hide where nobody looked.

---

## 2. Adapting the Testbench by Application Profile (`PROJECT.md` §1)

### `Prof-2`: Web & Fullstack Applications (Browser + Live API + DB)
* Exercise complete vertical slices in a real browser (`Client → API → DB`): auth, core CRUD, role restrictions, and cross-tenant isolation.
* Verify persistence across full page reloads (`F5`), behavior under throttled/offline networks, proper UI handling of `401/403/404/429/500` responses, and double-click/retry safety on write forms.
* Check desktop + mobile viewports and browser console/network tabs for uncaught errors.

### `Prof-3`: Mobile Applications (Emulator / Physical Device)
* Exercise cold start, core user loops, mid-flow exit & resume, portrait + landscape + tablet layouts, soft-keyboard visibility, dark/light themes, and locale switching.
* **Irreversible Upgrade Scenario (Cannot Be Skipped):**
  1. Clean-install the **previous released build** (record exact version & commit).
  2. Accumulate real user data touching all storage key shapes; capture a **pre-migration screenshot**.
  3. Install the new build **over the old one without wiping data** (`adb install -r`, no `pm clear`).
  4. Verify both key presence **and** intact values; capture a **post-migration screenshot**.
* **Mobile Testbench Traps:** verify installed APK hash/version before debugging (stale builds masquerade as code bugs); inspect `logcat` before filing crashes; remember `adb run-as` requires a debuggable build and `adb root` fails on release images; never alter release export flags just to inspect storage.

### `Prof-4`: Desktop Games & 3D Applications
* Headless unit tests do not prove live scenes work — launch the live window and run the full loop: *Launch → Core Mechanics / Commands → Save → Reload Save → Verify HUD, FPS & clean exit logs*.
* Verify windowed + fullscreen target resolutions, UI scaling, and zero runtime errors in engine logs.

---

## 3. Shared Testbench Discipline

Never assume a shared emulator, physical device, or staging database is free just because it appears idle (`PROCEDURES.md` GP-6):
* Coordinate shared testbench access through the Team Lead before wiping or resetting state.
* Never run heavy builds/emulators concurrently with parallel installs or full test suites on the same host.
* Check exit codes directly — never trust piped console output (`cmd | tail` hides build failures).

---

## 4. Handling Findings & Non-Defects

* **Never edit product content or unrelated code.** Report findings with environment, reproduction steps, screenshot/log, and exact user impact (`PROCEDURES.md` GP-3).
* **Explicitly document verified non-defects** (intentional behavior confirmed in code/docs) so future shifts do not raise duplicate false alarms.

---

## 5. Reporting & Failure Signs

Follow [`PROCEDURES.md`](../PROCEDURES.md) **GP-8** (save screenshots in `shots/` named by screen/state/viewport; keep the verification matrix updated; include *"Verified — Clean"*, *"Failed Probes"*, and *"What Remained Unverified"*).

**Signs the role drifted:** declared acceptance done without a filled matrix · *"checked, works"* without cell breakdown · skipped live upgrade/persistence checks · grabbed a shared device/staging DB without coordinating · judged build success from truncated logs instead of exit codes.
