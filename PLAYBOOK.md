# Team Playbook: Patterns, Techniques, and Orientation

**Established by the owner on 2026-08-29.**

> "When a new session starts, I don't want the participant acting like a raw newcomer... Maybe we can
> record patterns and regularities — not only about the project itself, but also about searching for
> information online and across other sources, orienting oneself outside the project."
> — Owner, 2026-08-29

> **A note on examples.** Every case here comes from a real shift on a single project (a mobile app
> with educational content). They are included **not as project-specific rules**, but as shapes: they
> show what a defect of a given kind actually looks like in the wild. As you read, substitute your
> own domain — the subject changes, the shape does not.

**Read once upon entering the team, before your role document — it takes ten minutes.** Nothing here
tells you what specific task to execute; this is what the team paid dearly to learn and refuses to
pay for again.

Three neighboring documents so you don't have to hunt for them: [`TEAM.md`](./TEAM.md) — who is who;
[`SOURCES.md`](./SOURCES.md) — where to study by discipline; **this file** — how to think.

---

## 1. One Question That Yielded More Than All Other Techniques Combined

> **"What shape would a defect have to take for this rule not to see it?"**

Not "where else are there defects," but "how could this still be wrong." Within twenty-four hours,
**three sessions arrived at this question independently**, and it produced every major finding:

* English leakage arrives **mangled by hyphenation** (`mother- in- law`), while the gate compared
  character-for-character → 35 English words inside Polish glosses **with a green gate**;
* a probe for "distractors differing only by a trailing `-e`: zero" returned zero because the strings
  ended with `!`: `Mach!` and `Mache!` do not differ in their last character → 240 defective items in
  a category declared clean;
* a concept's CEFR level turned out to be chosen by whatever **preserved cell counts**, rather than
  what the reference grammar stated → 104 items placed at the wrong level.

**The second most powerful question — when reviewing someone else's work:** not "are the numbers
accurate," but **"are they counting the right thing."** A working session proves that it completed
its assigned task; it has no vantage point from which to doubt the object of measurement it chose
for itself.

**The third — when reviewing a new filter:** "who measures what this filter hid, and is the filter
measuring itself?"

---

## 2. Recurring Defect Shapes

This table is **for recognizing a shape in an existing measurement, not for mechanical checklist
scanning**. Scanning down a checklist blinds you to novel problems; the table exists so that once
something looks off, you can name its shape quickly and apply the right fix.

| Shape | What it looks like | Example |
|---|---|---|
| **field is written, nobody reads it** | pipeline populates it, the app never queries it | `overlayPriority` on 412 items; `qa` on 5,852; `route.count` on 11,736 |
| **the same thing inside out** | screen writes data, the storefront never exposes it | `fragen` wrote a per-item map, the review sprint never read it |
| **two axes sharing one name** | "level" of a lexeme vs. "level" of a registry item are different concepts | 133 rows out of 332; "one key" vs. "one exercise item" |
| **invented entity** | exceptions accrete around it, each locally valid in a world where the entity exists | `B2-Beruf` treated as a proficiency level |
| **attribute without a fallback branch** | derived from a single source; its absence fails silently | English `business` track: the lexeme layer is literally absent, 0 of 6,212 |
| **ratchet without an explanation** | baseline raised, reason never recorded | any `content/qa/*.json` |
| **defect acting as a fuse** | a bug in one place causes harm there, but silently prevents a crash somewhere else — and nobody knows about its second job | looking up by exercise ID where a concept ID is stored |
| **level assigned by numbers** | level chosen to preserve a metric rather than to match the reference standard | `verb.passiv.perfekt` placed at B1 while its material is B2 |
| **mechanism built but never wired up** | indistinguishable in code from a working feature: it exists, compiles, is fully typed — and has zero callers | `cellServingStatus` (explains WHY a cell is empty) — 0 calls vs. 22 for neighboring `isCellServed`; or a test that is written, green, and never invoked by any runner |
| **guard on the axis along which the defect never arrives** | both the guard and the defect are real — they simply never meet | "no drill may be empty" is guarded by policy, while emptiness actually came from missing fields |

**The Neighboring Variable Law — the single most frequent shape here, three instances in one shift:**

> **A fix was applied to one variable and not applied to its neighbor living in the same file, often
> on the very next line. And each variable has a comment explaining WHAT and WHY was done — but only
> for itself.**

* the fix "score survives exit" was applied to **score** without touching **answered state** — the
  item shows up as unanswered, and XP is awarded repeatedly;
* plan 133 was accounted for in `levelCount` and missed in `runsAt` — the label "works at the nearest
  level" is **never** displayed;
* the row subtitle subtracted the **track** but forgot to subtract the **locale** — the card promises
  22 exercises, the lesson serves 6.

**How to hunt for it:** whenever you find a fix on a variable, look at **who lives next door and
whether that neighbor was born in the same old world**. The neighbor's comment will look reassuringly
deliberate — because it *was* true before the fix.

**The ninth shape is the most insidious, and it has a telltale sign:** *in all the other shapes
somebody forgot something; here, nobody forgot anything*. You find it with a different question:
**"through which door would a defect enter that this guard cannot see?"**

And the meta-pattern sitting above it: **guards stand where a defect would be loud, and are missing
where a defect is silent.** The German track is protected not by extra care, but by the fact that
losing it immediately breaks visible screens.

---

## 3. How Our Own Probes Fail

The most common trouble here is **not a code bug, but having no valid way to measure**. In a single
shift this happened four times — and all four were one underlying shape, not four separate mistakes.

**Cheap checks that have paid for themselves every time:**

1. **A round number — check the probe.** "348 out of 348" or "0 out of 5,110" is almost always a
   broken probe, not a property of the data.
2. **A huge number — check the question.** "3,015 out of 4,094" is almost always the right answer to
   the wrong question. Twice in one shift, a number like that was thrown out the moment someone read
   the actual rows.
3. **A zero result — prove the probe can see a defect at all.** Feed it a deliberately broken input.
   "Zero hits" without a positive control means "the probe didn't fire," not "the data is clean."
4. **Recall matters more than precision.** A rule can be logically sound and still catch zero:
   excluding single-word matches was well-justified for cognates, and yielded **zero recall** where
   every real leak happened to be a single word.
5. **A gate must never be weaker than the code it guards.** The gate checked five locales while the
   builder iterated over six — the gate issued a pass for territory it never inspected.
6. **A gate that measures what it filters is a mirror.** Telltale sign: its score improves in exact
   proportion to how aggressively the filter drops items.
7. **A gate whose satisfaction causes harm is a defective gate.** Three times in one shift: a
   repeatability metric improved because one question was split into four; a bundle-weight ratchet
   penalized a code comment that the bundler strips anyway.
8. **A gate that does not state its blind spots will be read a month later as proof of absence.**
   Write the probe's boundaries inside the gate's own output/code, not in a README next door.
9. **A ratchet never notices improvement** — it only fails on regression. Any improvement sits
   unprotected until someone tightens the baseline.
10. **The join key must match the identity enforced by the artifact itself.** The generator declares
    `course|track|id` in plain sight; two sessions joined by `id` alone and both got wrong answers —
    `new Map(items.map(x => [x.id, x]))` on a pool with composite identity **silently drops** records,
    and neither TypeScript nor unit tests complain. Thirty IDs appeared twice.
11. **`\b` in JavaScript is an ASCII word boundary.** A regex like `\b[Cyrillic word]\b` **never**
    matches any Cyrillic string and prints an honest-looking zero. Three times in a single shift, a
    probe returned a plausible number instead of failing loudly.
12. **A false positive is more dangerous than a miss.** A probe that prints a plausible "dirty" is
    worse than one that prints a plausible "clean": a miss sits quietly, whereas a false finding
    burns teammates' hours and travels upward as established fact. In one shift a session caught four
    such probes of its own, and the only one that made it all the way into a note to the owner was
    of this exact kind — "26 of 100 hints name the wrong grammatical person" when only one case was
    genuine. **Before handing off a finding, read the actual items, not just the count.**
13. **A green gate is a statement about what it reads, and nothing more.** The locale gate reads the
    live pool and was green; the storefront test compares the pool against the storefront and was
    red. Checking a branch with "content gates" gave all-green while the storefront lagged behind by
    49 items.
    **And what caught it was not a third test runner, but computing the same quantity from a second,
    independent source.** Disagreement between two sources of the same number is stronger than any
    single gate.
14. **A green `pnpm test` says nothing about types, and vice versa.** Jest runs through Babel and
    does not typecheck: two new files passed `pnpm test` green twice while `pnpm typecheck` found
    seven errors in them. Run both before committing.
15. **One field name, two meanings — indistinguishable by name.** `concept.level` in `verbtenses` is
    an executable floor for serving items (`atLeastLevel`); in the other four pools it is passive
    metadata. Editing "the same field" changes runtime behavior in one pool and does nothing in four.
16. **A locale gap is the sum of three completely different things that look identical as a number:**
    not yet written · **intentionally withheld under rule P5** · served to the user, but broken.
    "21 of 65" looks the same in all three cases. The second case is an executed design decision, and
    "fixing" it hands an unsolvable exercise back to the learner: one hundred `pattern@B1` items were
    withheld because only a single verbatim answer is accepted and those locales have no disambiguating
    instruction. **Any locale-coverage probe must distinguish these three buckets, or the first person
    who reads the report will start "fixing" bucket two.**
17. **Token-level contrast measurements are systematically more optimistic than the actual screen.**
    `tokenContrast`/`tokenDistance` composite a semi-transparent fill over `surface`, whereas on a
    live screen both fills sit over a page gradient and specular highlight that **pull them closer
    together**. A pair scoring 7.0 on tokens scored **2.5 on actual pixels** against a 2.3
    just-noticeable-difference threshold. Token math measures the upper bound of distinguishability,
    not what the user's eye receives.
18. **Two quantities are not comparable just because both are numbers.** Comparing `overlayPriority`
    directly against `priorityScore` produced "all 412 are lower, median −85" and an alarming
    headline — when in fact the two live on different scales (43..100 vs. 6..10). Checking how the
    sources were defined saved the day; checking whether the output looked plausible did not. Third
    instance of that shape in the same shift.
19. **Form is not meaning.** Field presence, string length, and character matches are easy to measure;
    semantic accuracy cannot be measured by script at all. More on this in §5.

---

## 4. Techniques That Paid Off

* **Measure before editing.** Anything that rewrites generator inputs is simulated against the old
  input first: how many items were added, removed, or altered. Three times, this stopped an already
  approved change before it did damage.
* **Second independent path.** Not "I double-checked my script," but "I arrived via a different route
  and got the exact same number." Agreement between two independent paths is proof; running the same
  path twice is not. That is how the number 332 was confirmed from both ends — starting from the UI
  screen and starting from the content directory.
* **A test that fails on the old code.** Verify by mutation: revert the fix — the test must turn red.
  Pointing a new gate at the old builders produced 243 findings and `exit=1` — only after that does
  its green status mean anything.
* **A targeted test run proves nothing about the neighbors.** It shows your new code does what you
  intended; only the full suite shows what else you broke along the way.
* **Ask the territory owner before calling their work a defect.** Half of all "defects" vanish with
  a single question.
* **Verify input reachability.** Before reporting a bug in a code branch, check whether any live flow
  can actually reach it. Dead code is not a runtime defect.
* **Document "not a defect" in the same detail as a finding.** Writing down "this area is clean, and
  here is why" saves the next shift the exact same hour of investigation.
* **"Dead fields" come in two distinct species, and they cannot be removed the same way.** An optional
  field that records a **human decision** (`overlayPriority`) is the only trace of that decision;
  deleting it destroys institutional memory without breaking the build. A required field that
  describes **data shape** (`route.count`) cannot be removed casually at all: `tsc` fails immediately.
  Always check both axes — whether the type requires it and what kind of knowledge it holds.
* **The reference solution often already exists in a neighboring locale.** Spanish collapsed "drill"
  and "practice" into one word while Polish kept them distinct — the fix reduced to aligning Spanish
  with the Polish distinction, turning a matter of taste into a verifiable rule. Before inventing a
  standard from scratch, check whether the problem was already solved next door.
* **List of your own doubts.** Not "please review everything," but "here is fork A vs. B where I was
  unsure." Writing that list **discovers defects by the sheer act of writing it**: while listing
  ambiguities, an author realized that Spanish `plantilla` means both "template" and "workforce."

---

## 5. What Cannot Be Checked by Machine at All

**We do not have — and never will have — an automated probe for translation accuracy; we only have
probes for translation form.**

A green locale gate means "the structural form is intact" and will **never** mean "the translation is
accurate." `Panne -> "podział"` is caught by no script: the word is not English, it collides with
nothing, its morphology is spotless — it simply means the wrong thing ("division" instead of
"breakdown").

The same holds for naturalness: a string can be grammatical, non-empty, free of obvious calques — and
still phrased in a way no native speaker would ever think or say. That requires a fluent human reader,
not a script.

**Practical consequence:** when a role reports "locales are green," it means structural form only. If
the recipient hears "the translations are good," the fault lies with how the report was phrased, not
with the recipient.

---

## 6. Orientation Outside the Project: How to Search and What to Trust

**Search order — top to bottom, and it usually stops at step two:**

1. **Check whether we already measure it locally.** Roughly thirty `pnpm report:*` and `check:*`
   scripts exist in the repo. Three times in one shift, an existing tool turned out to be measuring
   **the wrong object**, rather than "we needed to write a new tool."
2. **Our own project documents.** Content rules, quality requirements, plans, decision logs. An
   external source says what is customary in general; our documents say what has already been decided
   here, and they win.
3. **Primary external source** — the specification, official documentation, or publication by the
   body that defines the standard. Curated list by discipline — [`SOURCES.md`](./SOURCES.md).
4. **None of the above** — then the question is a product decision, not a reference lookup, and goes
   to the owner.

**How to tell a primary source from a secondary retelling:**

* a primary source has a **standard owner** — the organization that defines the thing rather than
  merely describing it;
* it explicitly states **what it does not cover**. Retellings never do;
* it is **dated and versioned**;
* its title does not start with "Best" or end with a number.

**Three rules about external sources, paid for on this project:**

1. **An external source answers "what is customary"; we decide based on what is measured here.** A
   corpus-frequency threshold imported from outside turned out to be **inverted** for our domain:
   `analysis` sits at rank #756 while `coffee` sits at #1395, so a gate built on corpus rank would
   have kicked `coffee` out of A1 and let `proposal` in.
2. **Disagreement between two authoritative sources is a finding, not noise.** For 32 German lemmas,
   Wiktionary lists both auxiliaries (`haben` and `sein`) while our lexicon stores only one. **Never
   strip a distractor when authoritative sources directly contradict each other** — escalate it as a
   decision, not a silent edit.
3. **An external source never grants permission.** It cannot override content rules or an owner
   decision. "Industry standard" is not an argument against a project invariant.

**And the owner's rule, stronger than any external source:** we **do not copy other apps**. You may
consider a general principle, never someone else's implementation. Test: **if you cannot explain a
recommendation without naming another app, it is copying, not engineering.**

---

## 7. Environment Traps — Things That Cost Hours

* **`/tmp` is `tmpfs`.** Anything a session writes to scratch disappears on reboot. Writing reports
  incrementally inside the working tree is the only real durability mechanism, not bureaucracy.
* **`git log` answers "what was merged," not "what was done."** A shift can look dead in commits and
  alive in the filesystem: last commit at 22:36, file modifications continuing until 23:54.
* **A probe with a hardcoded absolute path** dutifully prints numbers from **someone else's working
  tree** — and we run nine of them. Any script promoted into `scripts/` must use `process.cwd()`.
* **Never run verification suites while a working tree is actively being edited** — yours or a
  neighbor's. The test runner will read new code against half-written data and fail with a phantom red.
* **`gradlew ... | tail` returns the exit code of `tail`**, not of the Gradle build.
* **A stale APK on the device** masquerades as a code bug. Always compare the `md5` of the installed
  `base.apk`.
* **A second package installed on the emulator intercepts deep links** — always invoke `am start`
  with an explicit `-p <package>`.
* **The emulator crashes with `SIGSEGV` in `swiftshader`** — launch it with `-gpu host`.
* **Test suites are heavy operations too.** A single `pnpm test` spawns workers taking 1.5–2 GB each,
  and across nine working trees, 28 concurrent Jest processes from three trees exhausted RAM to zero
  and killed the run via OOM signal. Only **one** full test suite runs at a time; everyone else runs
  targeted tests scoped to their surface.
* **Never run heavy operations concurrently.** Nine working trees, each with its own `node_modules`
  and `android/build` on the same disk partition — running them together is what crashed the host
  machine and killed nine sessions at once.

---

## 8. How This File Is Maintained

**By every participant, on the same day. New patterns go through the Team Lead; factual inaccuracies
are fixed directly.** The fork depends on the nature of the edit, not the rank of the editor:

| What | How | Why |
|---|---|---|
| **a new pattern or technique** | via the Team Lead | only the Team Lead sees whether it recurs across sessions or already sits two lines above |
| **a verified inaccuracy in existing text** — a number no longer reproduces, a file is gone, a rule contradicts its neighbor | **fix it yourself**, naming the incident and marking the superseded wording inline | `TEAM.md` §6 and P-8.7: self-correction is standard practice, not overstepping. Silently ignoring an error is worse |
| **a rule that constrains you on your current task** | propose it; someone else makes the edit | here you are an interested party (D-5) |

> **Previous wording (prior to 2026-09-03): "by every participant, through the Team Lead, on the same
> day"** — with no carve-out for factual corrections. Superseded because it contradicted P-8.7 and
> the "fix it myself" branch of diagram D-5: the exact same correction to this file was simultaneously
> permitted and forbidden. Incident: the 2026-09-03 audit found a reference in `DIAGRAMS.md` to a
> non-existent step of P-8 along with two stale counts — all of them factual corrections of the
> second kind, where requiring Team Lead bottlenecks means they never get fixed at all.

An entry belongs here if and only if:

* **it will recur** — it is a structural pattern, not a one-off fluke;
* **it cost real time** — not "nice to know," but "we paid for this";
* **it is verifiable** — backed by a concrete example and number, not memory.

What **does not** belong here: repeating what already lives in a role document; a rule without the
incident it came from; or anything specific to a single run — that belongs in the run's decision log.

**Sign that this file has decayed:** people stop reading it end-to-end because it grew too long. When
that happens, split it by discipline instead of diluting it further.
