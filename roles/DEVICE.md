# Device / Runtime QA Role — Detailed Guide

Read after [`TEAM.md`](../TEAM.md). There is only one Device QA session per cycle — **because there is only one emulator**, not because there is little work.

---

## 1. Why This Role Is Separate

**Green tests prove code consistency, not that the application actually works.** The owner's requirement, verbatim: tasks must be completed so that **the application is in a working state**.

The cost was proven with a hard number: 4,711 passing unit tests failed to catch that **in landscape orientation the system keyboard replaces the entire screen**, forcing the user to type answers blindly. That can only be found by tapping the live product, and by nothing else.

And the second rule derived on this project: **an empty cell in the verification matrix is the most fertile hunting ground. Three out of three newly opened cells yielded a finding.** A cell sits empty not because there is nothing to see there.

**Boundary with the Designer.** Device QA verifies that a mechanic **works**; the Designer verifies that it is **understandable and appropriate**. Same screen, two different questions, zero duplication.

---

## 2. The Matrix — The Primary Instrument

Work is driven by the `trainer × verification column` matrix, not by inspiration. The columns are things that can break independently: cold start, completing a full lesson, exiting and returning mid-lesson, switching levels, switching courses, landscape orientation, tablet layout, foldable layout, upgrading over an older build, locale switching, dark theme, accessibility.

Rules for maintaining the matrix:

* **a cell is either closed completely or left open.** "Glanced at it briefly" is an open cell;
* **every closed cell is recorded with its result**, including "zero defects" — otherwise the next shift will waste time checking it all over again;
* **empty cells are tackled before suspicious ones.**

---

## 3. The Irreversible Scenario — The Only One You Cannot Redo Casually

Installing a new build **on top of an older build, without `pm clear`**, to observe storage migration on live user data. Everything else can be repeated; a botched live upgrade in the wild cannot.

The procedure refined on this project:

1. clean install of the **old** build whose missing features/schema are known (record the exact version number and commit hash);
2. accumulate real progress manually — touching every key shape, not just one;
3. **confirm the pre-migration state with a screenshot**, never by assumption;
4. install the new build **as an update** (`adb install -r`), without clearing data;
5. inspect not only "are the keys present," but also **their values**: totals, shards, migration manifest;
6. capture a post-migration screenshot.

**Traps already paid for:**

* **`run-as` requires `android:debuggable`** — internal storage can only be inspected on a debug build. The **new** (post-upgrade) side must be debuggable; the pre-upgrade side can be anything as long as its state is known;
* **`adb root` fails on release system images** — do not waste time trying it;
* **never edit export settings just to make inspection easier**: modifying the build to inspect it changes the very thing you are verifying;
* **`gradlew … | tail` returns the exit code of `tail`, not the build.** Run without a pipe or check `$PIPESTATUS` / `$?` directly;
* **verify the MD5 of the installed `base.apk` against your local build.** Once, "the wrong library version is running" turned out to be a stale APK on the emulator, costing an entire day;
* **if the emulator crashes with `SIGSEGV` in `swiftshader`** — launch it with `-gpu host`.

---

## 4. The Emulator Is a Shared Resource

**"The emulator is free" is someone's assumption, not the actual state of the device.** On 2026-08-28, two sessions shared an emulator, one session's `pm clear` landed right in the middle of the other's test run, and it was nearly reported as a catastrophic data-loss bug. Only checking `logcat` before filing the report saved the shift.

* **claim the emulator only after asking the Team Lead**, even if `adb devices` looks idle;
* **read `logcat` before drawing conclusions**, not after;
* **never run heavy operations concurrently**: run Gradle builds and the emulator one at a time, and never in parallel with someone else's `pnpm install`. Concurrent heavy runs crashed the host machine when nine working trees each had their own `node_modules` and `android/build` on the same disk partition.

### 4.1. Adapting the Testbench for Desktop Games and Fullstack Apps (`PROJECT.md` §1)

* **For Desktop Games on Godot 4 Mono / C# (Prof-4):**
  - A headless `dotnet test` run proves C# math, but **does not prove that the scene works**. Device / Runtime QA launches the live game window and walks through the full loop: *Scene Start → Build / Train / Issue Command → Save (`WorldSnapshot`) → Load Save → Verify HUD and FPS*.
  - Verify both fullscreen and windowed modes at the target resolution (`2560x1440`), UI scaling, and zero errors in the Godot log when closing the scene.
* **For Fullstack and Web Applications with a Backend (Prof-2):**
  - Verify the live `Client → API → DB` loop in a real browser/app, behavior under slow network conditions and `401/403/429/500` responses, absence of race conditions on double-clicking submit buttons, and state persistence across page reloads (`F5`).

---

## 5. What to Do with Findings

**Never edit learning content.** If you find a content defect, hand it to the Team Lead for the content workstream to fix. If you find a mechanical defect, follow the agreed boundary: fix trivial UI wiring yourself if authorized, or hand larger defects to the territory owner.

**Finding format:** screen, device, orientation, theme, locale, reproduction steps, **screenshot**, and what specifically the learner/user is prevented from doing. "Looks wrong" is not a finding.

**Explicitly record what is NOT a finding.** Example from a real cycle: active round keys did not carry over during migration — because `SKIPPED_SHAPES` excludes them intentionally, since an in-progress round is ephemeral display state rather than saved progress. The session checked the code before filing and avoided a false alarm. **Recording a verified "not a defect" saves the next shift the exact same hour.**

---

## 6. Reporting

* your own report file (location in `PROJECT.md` §8), written **as you go**, not at the end and **never in a temporary directory**;
* screenshots saved in `shots/`, with filenames naming the screen, theme, and state;
* **the matrix in the report is always kept up to date** — it is the authoritative map of what has been verified;
* the section "what remained unverified" is mandatory: which columns were never opened and why.

**And right alongside it — "what I tried to measure and why this cannot measure it."** This section was introduced on 2026-08-29 at the Analyst's suggestion, derived from three cases in a single shift: she discarded three large numbers — 3,015 out of 4,094, `ruleSteps` 3,058 out of 3,058, and the completely unusable separator "Russian as a control." None of the three is a finding, and all three save the next session a wasted pass: **without a record saying "this meter does not work and here is why," someone will build it again.** The cost is one paragraph; the benefit was measured three times in a single shift.

**A negative result about an instrument is just as valid a result as a finding.** Write down: what you measured, with what, why it does not answer the question, and what remains unmeasurable in principle.

---

## 7. Signs the Role Is Being Performed Incorrectly

* device acceptance is declared complete without a filled-in matrix;
* "checked it, everything works" without a cell-by-cell breakdown;
* the irreversible upgrade scenario was tested only once or without recording the exact versions of both builds;
* the emulator was claimed without asking the Team Lead;
* a discovered content defect was edited on the spot;
* a conclusion was drawn from console text output rather than the command's exit code.
