# Sources — Primary Technical References by Discipline

This file exists so an incoming session **never starts by searching for where to search**. Open only the section matching your active role and application profile (`PROJECT.md` §1).

---

## 1. Reading & Contribution Rules

1. **Project documents win over external sources.** External references explain general standards; `PROJECT.md` and project ADRs define what has been decided here.
2. **Measure locally first.** Check existing project scripts and telemetry (`PROJECT.md` §3) before reaching for external heuristics.
3. **Primary authorities only:** official specifications, language/framework maintainers, or standards bodies (e.g., W3C, OWASP, MDN, official SDK docs). Blog summaries and *"Top 10"* roundups are excluded.
4. **Every entry states what specific question it answers**, who added it, and when.
5. **Disagreement between two authoritative sources is a finding**, not noise — escalate rather than silently guessing.
6. **Domain-specific sources** (industry specs, proprietary APIs, internal norms) live in `<project>/SOURCES-PROJECT.md`, not here.

---

## 2. Design, UI/UX, Accessibility & Motion

| Question | Primary Source |
| --- | --- |
| Component systems, states, elevation, and motion principles | **Material Design 3** — `m3.material.io` |
| Platform interaction patterns, gestures, navigation, and accessibility | **Apple Human Interface Guidelines** — `developer.apple.com/design/human-interface-guidelines` |
| Baseline contrast ratios, focus order, keyboard nav, ARIA | **WCAG 2.2** — `w3.org/TR/WCAG22` |
| Perceptual readability contrast (catches cases WCAG passes on paper but fail visually) | **APCA Contrast** — `git.apcacontrast.com` |
| Respecting OS reduced-motion preferences | **W3C Media Queries 5** — `w3.org/TR/mediaqueries-5/#prefers-reduced-motion` |
| Web performance, Core Web Vitals, font loading & subsetting | **web.dev** — `web.dev/vitals` · `web.dev/font-best-practices` |

> **No-copying rule:** Study interaction principles, never copy another product's visual composition. *Self-test: if a design recommendation cannot be justified without naming another app, it is copying.*

---

## 3. Web Frontend, Fullstack Backend, APIs & Security (`Prof-1` / `Prof-2`)

| Question | Primary Source |
| --- | --- |
| Web APIs, HTTP semantics, cookies (`HttpOnly`, `SameSite`), CORS, CSP | **MDN Web Docs** — `developer.mozilla.org` |
| API security: BOLA/IDOR, broken auth, mass assignment, SSRF, rate limits | **OWASP API Security Top 10** — `owasp.org/API-Security/` |
| Web AppSec: XSS, CSRF, injection, session management, security headers | **OWASP Cheat Sheet Series** — `cheatsheetseries.owasp.org` |
| Relational modeling, transactions, locking, indexes, `EXPLAIN ANALYZE`, RLS | **PostgreSQL Official Docs** — `postgresql.org/docs/current/` |
| TypeScript compiler options, strict mode, module resolution | **TypeScript Handbook** — `typescriptlang.org/docs/` |
| 3D WebGL lifecycle, geometry/material/texture disposal | **Three.js Docs** — `threejs.org/docs/` |

---

## 4. Mobile Applications (`Prof-3` — Android / iOS / Cross-Platform)

| Question | Primary Source |
| --- | --- |
| Android touch targets, TalkBack, font scaling, accessibility | **Android Accessibility** — `developer.android.com/guide/topics/ui/accessibility` |
| On-device storage limits, scoped storage, backup behavior | **Android Data & Files** — `developer.android.com/training/data-storage` |
| APK signing & upgrade certificate verification (`--print-certs`) | **apksigner Docs** — `developer.android.com/tools/apksigner` |
| Inspecting debug app storage (`adb shell run-as`; note `adb root` fails on release images) | **Android Debug Bridge (`adb`)** — `developer.android.com/tools/adb` |
| Store policies, declarations, release compliance | **Google Play Console Help** — `support.google.com/googleplay/android-developer` |
| React Native architecture & thread/UI-thread boundaries (when applicable) | **React Native Docs** — `reactnative.dev/architecture` |

---

## 5. Desktop Game Development & 3D (`Prof-4` — Godot 4 / C# / 3D Assets)

| Question | Primary Source |
| --- | --- |
| Scene architecture, `uid://` resources, signals, lifecycle, C#/.NET bindings | **Godot 4 Official Docs (C#)** — `docs.godotengine.org/en/stable/tutorials/scripting/c_sharp/` |
| 3D rendering optimization, Mesh LODs, lighting, import pipeline | **Godot 3D Performance & Assets** — `docs.godotengine.org/en/stable/tutorials/performance/` |
| `.glb` / `.gltf` materials, PBR textures, skeletal animation, node hierarchy | **Khronos glTF 2.0 Spec** — `registry.khronos.org/glTF/specs/2.0/glTF-2.0.html` |
| Safe JSON serialization in .NET (avoiding insecure deserializers) | **Microsoft `System.Text.Json` Docs** — `learn.microsoft.com/en-us/dotnet/standard/serialization/system-text-json/` |
