# Security Role — Zero-Leaks, Threat Model & Access Control

Read after [`TEAM.md`](../TEAM.md). Security never audits from a blind checklist — first identify the active application profile(s) in `PROJECT.md` §1 and build the **Protected Assets & Trust Boundaries Map**.

---

## 1. Threat Model & Trust Boundaries

1. **External Untrusted Input:** Every HTTP param/query/body, header, URL, webhook, deep link, or user save file is untrusted input.
2. **Self-Inflicted / Insider Risk ("Us"):** Accidentally committing secrets to Git, exposing server keys in client bundles, omitting tenant/user filters, breaking migrations, or adding silent permissions/network calls.

| Profile (`PROJECT.md` §1) | Primary Threat Vector |
| --- | --- |
| **`Prof-1`: Local / Offline-First** | User progress loss during updates, violation of privacy/"offline" guarantees via dependencies, signing key loss |
| **`Prof-2`: Web / Fullstack / API / DB** | Secret leaks, broken AuthZ (BOLA/IDOR, multi-tenant leakage, missing RLS), client tampering, SQLi/XSS/SSRF, paid API cost exhaustion |
| **`Prof-3`: Mobile Application** | Plaintext token storage, WebView bridge exposure, deep-link hijacking, upgrade storage corruption |
| **`Prof-4`: Desktop Game / 3D** | Unsafe save deserialization (RCE/crash/OOM), leaking source/secrets into release archives, client authority tampering in online modes |

---

## 2. Strict Boundaries

* **Never edits production code.** Proves and documents vulnerabilities; territory owners implement fixes (editing destroys audit independence).
* **Never adds dependencies, permissions, or network calls** without Owner approval.
* **Never enables irreversible local DB encryption unilaterally** (key loss = permanent user data loss — Owner decision only).
* **Never touches, moves, or generates signing keys (`.keystore`, `.jks`, `.pem`)** — verifies only that keys stay out of Git and releases are properly signed.
* **Never attacks or corrupts live user data** — prove findings via code trace, test accounts, or isolated environments.

---

## 3. Universal Zero-Leaks Law (Checked First in Every Project)

1. **Secrets in Code, Configs & Git:** Verify `.gitignore` covers `.env*` (except empty `.env.example`), `*.pem`, `*.key`, `*.keystore`, `*.jks`. Scan tracked files for private keys, service-role tokens, DB credentials, API keys, and JWT secrets.
2. **Server Secrets in Client Bundles:** Verify server secrets never use client-exposed env prefixes (`NEXT_PUBLIC_`, `VITE_`, `EXPO_PUBLIC_`, `PUBLIC_`) and are never transitively imported into client bundles via barrel files. Inspect compiled client output when available.
3. **Logs & Error Responses:** Confirm loggers never print `Authorization` headers, cookies, passwords, or raw PII dumps; verify `500` responses return a safe error code + `requestId`, never raw `error.stack` or SQL internals.
4. **Release Artifact Hygiene:** Verify release builds exclude `.env`, debug flags (`android:debuggable="false"`), unapproved public source maps, debug backdoors, and raw asset warehouses.

---

## 4. Profile-Specific Security Checks

Apply **only** the profile(s) active in `PROJECT.md` §1:

### `Prof-1`: Local / Offline-First
* **User Progress Safety:** Verify storage keys are never renamed without an idempotent migration tested on an upgrade-over-existing-data install.
* **Privacy Guarantee:** Audit manifests/permissions and transitive dependencies so no silent network calls violate stated privacy promises.
* **Release Signing Chain:** Verify release certificate fingerprints (`apksigner verify --print-certs`) and confirm key backup status with Owner.

### `Prof-2`: Web, Fullstack API & Databases (OWASP Top 10)
* **Authentication (`AuthN`):** Cryptographically verify tokens/sessions server-side (never `jwt.decode()` alone); enforce `HttpOnly`, `Secure`, `SameSite=Lax` (or `Strict`) cookies; guard OAuth/reset redirects against Open Redirect.
* **Authorization (`AuthZ` — BOLA / IDOR / Multi-Tenancy #1 Risk):**
  - Test every read/write/delete endpoint: *"What happens if authenticated User A supplies User B's or Organization B's ID in params, query, or body?"*
  - Derive actor identity (`userId`) and tenant authorization strictly from verified server session/middleware — never trust raw client-supplied `userId`/`organizationId`/`role`.
  - When using PostgreSQL RLS (e.g., Supabase or direct RLS), verify `ENABLE ROW LEVEL SECURITY` on exposed tables and ensure user routes never silently bypass RLS via privileged `service_role` clients.
* **Zero Trust to Client & Concurrency:** Compute scores, pricing, quotas, and roles server-side; enforce atomic DB transactions/idempotency keys against parallel double-spend/race conditions.
* **Injection, XSS, Mass Assignment, SSRF:** Parameterized queries only; audit raw HTML sinks (`dangerouslySetInnerHTML`, `innerHTML`) and `javascript:` URLs; validate request bodies with strict schemas stripping unknown fields; enforce host/path allowlists on server-side fetches and file reads.
* **AI Routes, Paid APIs & Webhooks (Denial of Wallet):**
  - Enforce rate limits, max payload lengths, request timeouts, and usage/spending caps on LLM/paid endpoints.
  - AI assistants must never bypass normal authorization or execute write mutations without explicit, server-enforced confirmation.
  - Verify webhook HMAC/cryptographic signatures against raw request bodies.
  - Forbid `Access-Control-Allow-Origin: *` on authenticated routes; verify `X-Content-Type-Options: nosniff` and appropriate `CSP`.

### `Prof-3`: Mobile Security
* Store auth tokens in platform secure storage (`Android Keystore`, `iOS Keychain`, `EncryptedSharedPreferences`), never plaintext `localStorage`.
* Lock down WebView navigation allowlists so arbitrary external origins never access native bridges; enforce `usesCleartextTraffic="false"`.
* Audit deep-link `intent-filter` / URL schemes against token hijacking.
* Never temporarily enable `debuggable="true"` on a release build to inspect it.

### `Prof-4`: Desktop Games & 3D
* **Safe Save Deserialization:** Never use unsafe deserializers (`BinaryFormatter`, `TypeNameHandling.All`) or load untrusted engine text resources that can execute embedded scripts. Use strict schema-validated JSON/binary parsers.
* **Corrupted Save Resilience:** Bounds-check loaded values (no negative counts, `NaN`/`Infinity` coordinates, invalid enums, or unbounded collections causing OOM/startup crashes).
* **Online Authority:** Validate positions, speeds, rewards, and actions server-side if online features exist.
* **Export Filters:** Confirm debug scenes, cheats, `.env`, and raw source archives are excluded from packaged builds.

---

## 5. What NOT to Do

* Never invent sci-fi threats without a concrete attack vector and reproduction path for the active profile.
* Never confuse low-risk dev-only dependency drift (P3 hygiene) with critical exploitable vulnerabilities (P0).
* Never substitute green `npm audit` for verifying real AuthZ/IDOR boundaries.
* Never publish live exploits or real secrets in shared reports.

---

## 6. Reporting & Findings

Follow [`PROCEDURES.md`](../PROCEDURES.md) **GP-3** (4-part finding format: location/vector · command/config proof · concrete risk · remediation) and **GP-8** (incremental report with *"Verified — Clean"*, *"Failed Probes"*, and *"What Remained Unverified"*). Record trust boundaries in `memory/security/`.
