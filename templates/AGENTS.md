# AGENTS.md — Lightweight Engineering Rules (AI-Team Lite Mode)

> **Template.** Copy to `<project>/AGENTS.md` and customize the Verification Commands & Boundaries sections.
> Designed for **1 primary coding agent + 1 independent reviewer** on greenfield or small-to-medium projects — zero multi-worktree ceremony, full engineering discipline.

---

## 1. Project Context

- **Product:** `<1–2 line description>`
- **Primary Docs:** `docs/product/PRD.md`, `docs/architecture/SYSTEM_OVERVIEW.md`, `docs/adr/`
- **Current Status:** `docs/PROJECT_STATUS.md` *(read before starting a new task; update after each milestone)*

---

## 2. Verification Commands (Single Source of Truth)

Replace placeholders with real commands verified in this repo:

```bash
# Typecheck (test runners via Babel/SWC do NOT typecheck!)
<e.g., pnpm typecheck>

# Lint
<e.g., pnpm lint>

# Unit & integration tests
<e.g., pnpm test>

# Build
<e.g., pnpm build>
```

---

## 3. Mandatory Engineering Rules

1. **Plan Small Vertical Slices First:** Before editing code on any non-trivial feature, analyze existing files, define data/API contracts, and get approval on a small implementation plan. Never dump giant unreviewed rewrites.
2. **Verify Before Claiming Done:** Every claimed number, passing check, or fixed bug must come from a live command run in this session. Always run typecheck + lint + tests before declaring completion.
3. **Mutation Check on Tests:** A green test written alongside new code proves nothing by itself. Confirm your test fails when the guarded condition is broken (or against a deliberately invalid input).
4. **Law of the Neighboring Variable:** Whenever a fix changes one field, flag, filter, or state variable, immediately inspect sibling variables in the same file/model to ensure they did not stay in the old assumption.
5. **Trace Production Consumers:** Every new field, migration column, or helper must be traced from writer → storage/contract → production reader (not only tests).
6. **Zero-Leaks Security Baseline:**
   - Never commit secrets, `.env` values, private keys, or credentials.
   - Never expose server secrets via client env prefixes (`NEXT_PUBLIC_`, `VITE_`, etc.) or barrel imports.
   - Validate all external/API inputs at runtime (`Zod` or equivalent); never trust client-supplied ownership IDs (`userId`, `organizationId`, `role`) without server-side authorization.
   - Never return raw stack traces or SQL errors in API responses.
7. **Clean Git Hygiene:** Commit with explicit file paths (`git commit -- <paths>`); never blindly run `git add -A`. Never silently delete or revert unfamiliar uncommitted edits.
8. **Honest Reporting:** Every completed task report must briefly explain:
   - data flow & key trade-offs,
   - what was verified (with exact commands),
   - **what remained unverified** (blind spots).

---

## 4. Independent Review Rule

When reviewing a diff or auditing a feature (e.g., via a second session or CLI reviewer):
- **Stay strictly read-only during review** — fixing the first bug you spot destroys audit independence.
- Ask: *"What shape would a defect or attack have to take for our current tests and checks to miss it?"*
- Report findings in 4 parts: **(1) Location & Symptom · (2) Proof/Command · (3) User/System Impact · (4) Proposed Fix**.
