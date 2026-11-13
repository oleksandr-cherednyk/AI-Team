# Sources — Where to Learn Across Every Discipline

**Established by the owner on 2026-08-29.**

> "Team members must inform you about high-quality sources they find. For example, when a Designer
> spins up in a new session, they should immediately know where to go to study and research
> information on a specific problem."
> — Owner, 2026-08-29

> "This applies to all team members. That way we build up a body of 'experience' to improve
> orientation, learning, and inspiration for the team." — Owner, 2026-08-29

This file exists so that **a new session never starts by searching for where to search**. It opens
its section and immediately knows where to go for its question.

**It is shared across all roles, not "just for the Designer."** Every role contributes what it
finds, and every role reads what it needs — so the team accumulates knowledge that does not die with
the session. A session lives for hours; this file lives across runs.

---

## 1. How to Read This File

Start with **your own section**, not the whole file. Sections are organized by discipline, and
within each section by question.

Every entry states **not "good website," but "what question it answers."** A source without that
label is useless: a month later, nobody will remember why it was put here.

**And the cardinal rule of reading:** an external source answers "what is standard practice," while
our decision is made based on **what we measured in our own system**. An external source is not
permission. This project already had a case where a corpus frequency threshold taken from an external
list was **inverted** for our problem: `analysis` ranks at #756 while `coffee` is at #1395, so a gate
built on that list would have evicted `coffee` from A1 while letting `proposal` through.

---

## 2. How to Contribute — Every Member's Responsibility

**Found a high-quality source? Tell the Team Lead.** The Team Lead adds it; you can propose the
exact row.

Three requirements for every entry — all three are mandatory:

1. **Primary authority.** A specification, official documentation, or publication by the organization
   that actually defines the thing. **Blog summaries, "Top 10 Trends" roundups, and forum answers are
   not sources.** There is only one exception: when no primary specification exists (for instance, de
   facto engineering practice without a formal spec) — in which case mark that explicitly;
2. **What question it answers** — in one specific line. Not "about design," but "what contrast ratio
   is sufficient and why there isn't just a single threshold";
3. **Who added it and when** — so six months later we know whom to ask how it proved useful.

**What not to put here:** sources nobody has actually read; "just in case" links; and sources from
which we already extracted a one-off project decision — those move into the document where the
decision is recorded, and stay here only if we will need to consult them again.

---

## 3. Design, UI/UX, Motion, Color

| Question | Source |
|---|---|
| Components, states, behavior, motion — as a coherent system rather than a bag of tricks | **Material Design 3**, `m3.material.io` |
| Platform expectations, gestures, navigation, accessibility | **Apple Human Interface Guidelines**, `developer.apple.com/design/human-interface-guidelines` |
| What contrast is sufficient and why there is more than one threshold | **WCAG 2.2**, `w3.org/TR/WCAG22` |
| Contrast that models **readability**, not just luminance ratio | **APCA**, `git.apcacontrast.com` — needed where WCAG passes a pair on paper that is still hard to read on screen |
| Android accessibility: touch target sizes, TalkBack, font scaling | **Android accessibility**, `developer.android.com/guide/topics/ui/accessibility` |
| Animation in our stack: what runs on the UI thread and what does not | **React Native Reanimated**, `docs.swmansion.com/react-native-reanimated` |
| `prefers-reduced-motion` and what "disabling motion" actually means | **W3C MediaQueries 5**, `w3.org/TR/mediaqueries-5/#prefers-reduced-motion` |

**Owner's rule, stronger than any external source:** we **do not copy other apps**. You may consider
a principle or interaction pattern, never the implementation or visual composition. Test: **if a
recommendation cannot be explained without naming another app, it is copying, not a design decision.**

---

## 4. Architecture — Mobile and Web

| Question | Source |
|---|---|
| What the React Native New Architecture provides, what it costs, and where thread boundaries lie | **React Native**, `reactnative.dev/architecture` |
| What an SDK version changes and what it still lacks | **Expo SDK changelog**, `expo.dev/changelog` |
| Storage behavior and limits on Android | **Android data & files**, `developer.android.com/training/data-storage` |
| Store requirements, country fields, age rating questionnaires | **Play Console Help**, `support.google.com/googleplay/android-developer` |
| Web performance and what to measure | **web.dev**, `web.dev/performance` · **Core Web Vitals**, `web.dev/vitals` |
| Web fonts and font subsetting | **web.dev/font-best-practices** |

---

## 5. Project Domain — Maintained Separately

**Sources specific to a project's domain do not belong in this file:** internal standards,
dictionaries, industry specifications, reference manuals. Those live in `SOURCES-PROJECT.md` next to
`PROJECT.md` and are versioned with the project.

**The format is identical** (primary authority · what question it answers · who added it and when),
and one rule from there belongs here because it is universal:

> **Disagreement between two authoritative sources is a finding, not a nuisance.**
> In the original project, two reference authorities disagreed on the norm for 32 entries while our
> internal data stored only one option. The takeaway worth keeping: **never silently edit content
> when authoritative sources directly contradict each other** — escalate that as a decision for the
> owner, not a silent fix.

---

## 6. Data, Storage, Migrations

| Question | Source |
|---|---|
| On-device storage behavior and limits | **Android data & files**, `developer.android.com/training/data-storage` |
| Reading storage from a debug build | `adb shell run-as <package> cat databases/...` — **`adb root` is denied on a release image**, and nothing can work around that |
| APK signing and upgrade compatibility | **apksigner**, `developer.android.com/tools/apksigner` — always verify with `--print-certs`, never assume |

## 7. Before Searching Outside

Check whether we already measure it locally: there are roughly thirty `pnpm report:*` and `check:*`
scripts — see the `scripts` section of `package.json`. **Three times in a single shift, an existing
tool turned out to be measuring the wrong object**, rather than "we lacked a tool."

Search order, signs of a trustworthy source, and what an external source cannot authorize —
[`PLAYBOOK.md`](./PLAYBOOK.md) §6. Environment traps and lessons paid for — §7 of the same file.

---

## 8. Game Development (Godot 4 Mono / C# / 3D) and Fullstack / Backend

| Question | Source |
|---|---|
| Scene architecture, resources (`uid://`), signals, node lifecycle, and C# API differences in Godot 4 | **Godot Engine 4 Official Documentation (C# / .NET)**, `docs.godotengine.org/en/stable/tutorials/scripting/c_sharp/` |
| 3D rendering optimization, Mesh LODs, lighting, and 3D scene import pipeline in Godot 4 | **Godot 3D Performance & Import**, `docs.godotengine.org/en/stable/tutorials/performance/` · `tutorials/assets_pipeline/` |
| `.glb` / `.gltf` format specification (materials, PBR textures, skeletal animation, node hierarchy) | **Khronos glTF 2.0 Specification**, `registry.khronos.org/glTF/specs/2.0/glTF-2.0.html` |
| API security, object-level authorization checks (BOLA/IDOR), and server-side validation | **OWASP API Security Top 10**, `owasp.org/API-Security/` |
| Row-Level Security (RLS) policies and separation between `anon` and `service_role` keys | **Supabase Row Level Security & Auth Docs**, `supabase.com/docs/guides/database/postgres/row-level-security` |

---

## 9. Our Own Documents — Read Before External Sources

An external source tells you **what is generally done**. Project documents tell you **what has
already been decided here**, and they take precedence.

| What | Where |
|---|---|
| Content and quality rules, gates, territories, generators, baselines | `PROJECT.md` — **the single project-specific anchor** |
| Team structure and roles | [`TEAM.md`](./TEAM.md) |
| Recurring patterns and techniques | [`PLAYBOOK.md`](./PLAYBOOK.md) |
| What this role already knows | `memory/<role>/` |
| What work is open right now | `board/00-START.md` |

**Project-specific sources** — internal norms, dictionaries, domain specifications — are maintained
in `SOURCES-PROJECT.md` alongside `PROJECT.md`. Only sources valid across projects stay here.
