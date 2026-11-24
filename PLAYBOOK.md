# Team Playbook: Defect Shapes, Probe Failure Modes & Proven Techniques

> **A note on historical examples:** Concrete examples below are anonymized incidents drawn from real multi-session runs (primarily a cross-platform content/mobile codebase). They are kept **as structural patterns, not project-specific rules** — substitute your own domain; the defect shape stays identical.

**Read once on onboarding (~10 minutes).** Nothing here prescribes your specific task; this is what past runs paid dearly to learn and refuse to pay for again.

---

## 1. Three Questions That Catch More Bugs Than Any Checklist

1. ***"What shape would a defect have to take for this rule or test not to see it?"***
   Not *"where else are bugs?"*, but *"how could this still be broken while every check stays green?"*
   - *Case:* A gate checked character-for-character for leaked English words; leaks slipped past because of soft hyphenation (`mother- in- law`) → 35 defects under a green gate.
   - *Case:* A probe searched for distractors differing only by trailing `-e` and returned `0` because strings ended with `!` (`Mach!` vs `Mache!`) → 240 missed defects.
2. ***"Are they counting the right thing?"*** *(not just "is the arithmetic accurate?")*
   A working session proves it finished its task; it has no vantage point to question the metric it chose for itself.
3. ***"Who measures what this filter hid — and is the filter measuring itself?"***

---

## 2. Recurring Defect Shapes (For Recognition, Not Mechanical Checklists)

Use this table to **name** a pathology once something looks suspicious — never scan it mechanically as a checkbox list, which blinds you to novel defects.

| Shape | How It Manifests | Historical Example |
| --- | --- | --- |
| **Written, never read** | Pipeline/server writes a field; production app never queries it | Priority metadata written on 400+ records with `0` production callers |
| **Same thing inside out** | Screen/module writes state in one shape; consumer reads another location | Per-item map saved by screen, ignored by summary/review consumer |
| **Two axes sharing one name** | Same identifier (`level`, `status`, `id`) means two distinct domain concepts | Word difficulty vs. lesson tier diverged on 133 of 332 rows |
| **Invented entity** | Pipeline introduces an entity absent from the real domain; exceptions pile up around it | Special track treated as a proficiency level, spawning edge cases everywhere |
| **Attribute without fallback** | Derived from a single optional source; missing input fails silently as `0` | Track filter returned `0` items when source metadata layer was absent |
| **Ratchet without explanation** | Failure threshold raised silently without recording why underlying data changed | Baseline JSON bumped to green CI, later mistaken for intended norm |
| **Defect acting as a fuse** | A bug harms path A while silently preventing a worse failure in path B | Wrong ID lookup blocked half of updates while accidentally masking invalid scores |
| **Built, typed, never wired** | Compiles cleanly with full types — `0` runtime callers or unregistered test file | Helper function or test script green in isolation, never invoked by runner/app |
| **Guard on the wrong axis** | Both guard and bug are real, but live on orthogonal axes and never intersect | Policy checked non-empty arrays while emptiness arrived via missing object keys |

### The #1 Most Frequent Shape: The Law of the Neighboring Variable

> **A fix is applied to one variable and missed on its sibling living in the same file — often on the very next line. Both have reassuring comments that were true *before* the fix.**

- *Case:* Fix made `score` survive exit, missed sibling `answeredState` → users re-earned rewards repeatedly.
- *Case:* New filter subtracted `track` in subtitle count, forgot sibling `locale` → card promised 22 items, delivered 6.

**Rule:** Whenever you fix or audit a variable/field, inspect every sibling living next door in the same file or state object.

**Meta-pattern:** *Guards stand where defects are loud, and are missing where defects are silent.*

---

## 3. How Our Own Probes & Tests Lie

1. **Suspiciously round numbers (`0 of N`, `N of N`):** Almost always a broken probe, not spotless data.
2. **Huge numbers (`>70%` flagged):** Almost always the right answer to the wrong question — inspect 3–5 real records immediately.
3. **Zero hits without a positive control proves nothing:** Feed the detector a deliberately broken input to prove it can fire at all.
4. **Gate weaker than the code it guards:** Gate checked 5 inputs while builder iterated over 6 — passing uninspected territory silently.
5. **Mirror gate:** Score improves in exact proportion to how aggressively the filter drops records.
6. **Harmful gate:** Making the check green requires degrading the product (e.g., collapsing distinct items to game a repetition ratio).
7. **Unstated blind spots:** A probe that does not print its own blind spots will be misread next month as proof of total absence.
8. **Ratchets ignore improvements:** They fail only on regression; improvements sit unprotected until baselines are tightened.
9. **Composite identity loss:** `new Map(items.map(x => [x.id, x]))` silently overwrites collisions when true identity is composite (e.g., `orgId|projectId|id`).
10. **ASCII `\b` regex trap in JS:** `\b` matches only `[A-Za-z0-9_]` — silently returning `0` on Unicode/umlauts/Cyrillic.
11. **False positives hurt more than misses:** Read actual matched records before escalating a count.
12. **Transpiler test runners skip typechecking:** Jest/Vitest via Babel/SWC can pass while `tsc --noEmit` fails. Always run both.
13. **Form ≠ meaning:** Automated scripts validate syntax/structure, never semantic correctness or UX clarity.
14. **Token math ≠ rendered pixels:** Semi-transparent colors over gradients/blur can pass token contrast (7.0) while failing real pixel contrast (2.5).
15. **Incompatible numeric scales:** Never compare two metrics directly without checking their scale (`43..100` vs `6..10`).
16. **Single-source green lies:** Confirm critical counts via a **second independent path** (e.g., UI output vs. raw DB query).

---

## 4. High-ROI Verification Techniques

* **Measure before editing:** Simulate rewrites/migrations against existing data before mutating anything.
* **Mutation check (test fails on old code):** Revert the fix (or pass broken input) and confirm the test turns red (`exit != 0`).
* **Second independent path:** Re-derive critical numbers from a completely different entry point; repeating the same script twice is not verification.
* **Targeted tests prove local intent; only full suites prove non-regression.**
* **Verify input reachability** before filing a bug on dead code.
* **Record "verified clean (and why)"** in the same detail as defects so future sessions do not redo the work.
* **Distinguish two species of "dead fields":** optional fields recording human decisions vs. required schema fields — check both type constraints and institutional history before deleting.
* **Write a "List of Doubts" on handoff:** Listing the 2–3 forks where you felt unsure often reveals hidden bugs simply by writing them down.

---

## 5. Environment & Workflow Traps

* **`/tmp` is volatile `tmpfs`:** Reports and patches saved there vanish on reboot. Keep work inside the working tree or durable session artifacts.
* **`git log` shows merged history, not live work:** Always check `git status` across all worktrees and file `mtime`.
* **Never hardcode absolute paths in reusable scripts:** In multi-worktree setups, hardcoded paths silently measure the wrong worktree — always use `process.cwd()`.
* **Never pipe a build command without checking its real exit code:** `build_cmd | tail` exits with `tail`'s `0` unless `set -o pipefail` or `$PIPESTATUS` is checked.
* **Never run checks mid-edit** (yours or a neighbor's) — produces phantom reds from mixed old/new artifacts.
* **Stagger heavy operations across worktrees:** Parallel installs, full test runs, container builds, or emulators can OOM the host machine.
* **Check artifact freshness on live testbenches:** Always verify build hash/version before debugging a bug that may come from a stale deployment, cache, or APK.

---

## 6. Maintaining This File

* **New universal pattern or probe failure:** Add via Team Lead on the same day, backed by a real incident/measurement (never abstract "best practices").
* **Verified factual error in existing text:** Fix directly in passing, mark the prior wording inline with a date, and tell the Team Lead (`PROCEDURES.md` GP-13).
* **Do not duplicate role docs or project-specific facts** (`PROJECT.md` §10 holds project-specific traps). If this file grows too long to read in ~10 minutes, split by discipline rather than diluting it.
