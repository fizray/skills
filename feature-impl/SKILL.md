---
name: feature-impl
description: >
  Guide a developer and AI agent through implementing a new feature from a vague
  or informal request. Use this skill whenever someone says "bos minta fitur X",
  "implement feature", "tambah fitur", "bikin fitur baru", "ada request fitur",
  or any time a new feature needs to be built into an existing codebase. This
  skill produces a focused context file, identifies relevant code, and structures
  a clear implementation plan — minimizing wasted token usage and AI
  misunderstandings.
---

# Feature Implementation Skill

A skill for turning vague feature requests into structured, AI-executable
implementation plans — with minimal token waste and maximum clarity.

---

## PHASE 0 — BEFORE YOU TOUCH ANY CODE

This phase runs entirely in conversation. No file reading yet.

### Step 0.1 — Clarify the request

When the user gives a vague feature request, ask ONLY these questions
(skip any that are already clear from context):

```
1. Apa yang user/end-user bisa LAKUKAN dengan fitur ini?
   (describe behavior, not technical solution)

2. Apa yang BERUBAH dari kondisi sekarang?
   (new screen? new button? new API? new DB column?)

3. Ada contoh konkret cara pakainya?
   (misal: "user klik X, lalu Y muncul, lalu Z tersimpan")

4. Ada batasan / hal yang TIDAK boleh berubah?
   (misal: jangan ubah tabel X, harus backward compatible)

5. Prioritas: Speed vs Quality?
   (quick & dirty dulu, atau langsung production-ready?)
```

**Do NOT proceed to Phase 1 until you have answers to at least 1, 2, and 3.**

### Step 0.2 — Restate the feature in your own words

After clarifying, write a 3–5 sentence plain-language summary:

```
FEATURE SUMMARY
---------------
[What it does, from the user's perspective]
[What parts of the system are likely involved]
[What success looks like]
```

Ask the developer: *"Ini yang dimaksud? Ada yang salah atau kurang?"*

Do not proceed until the developer confirms.

---

## PHASE 1 — SURGICAL FILE DISCOVERY

Goal: find ONLY the files that matter. Do NOT read everything.

### Step 1.1 — Hypothesis-first approach

Before reading any files, write down your hypothesis:

```
LIKELY AFFECTED AREAS
---------------------
[ ] Frontend   → [guess which screen/component]
[ ] Backend    → [guess which controller/service/model]
[ ] Database   → [guess which table/migration needed]
[ ] Mobile     → [guess which screen/widget]
[ ] API        → [guess which endpoint changes or is new]
[ ] Config     → [any env vars, feature flags?]
```

### Step 1.2 — Read strategically, not exhaustively

Use this priority order. Stop when you have enough context:

**Round 1 — Routing files only (cheapest)**
- Backend: `routes/`, `api.php`, `web.php`
- Frontend: `router/`, `routes.js`, `pages/`
- Mobile: `lib/routes.dart`, `router.dart`

**Round 2 — Entry points for the relevant feature**
- Read ONLY the controller/component/screen that handles the closest
  existing feature (the one most similar to what you're building)

**Round 3 — Data layer (only if needed)**
- Read the relevant Model and Migration files
- Read the relevant Vuex store / Provider / service ONLY if state
  management is involved

**Round 4 — Tests (only if they exist and are relevant)**
- Read 1–2 existing test files for the similar feature to understand
  expected behavior patterns

> ⚠️ STOP RULE: If you've read more than 15 files and still feel lost,
> stop and report to the developer what's unclear. Do not keep reading.

### Step 1.3 — Generate the context file

After file discovery, create a file called `FEATURE_CONTEXT.md` in the
project root (or a `/.ai/` folder if it exists).

Use this exact template:

```markdown
# Feature Context: [Feature Name]
_Generated: [date]_

## Feature Summary
[3–5 sentence plain description from Phase 0]

## Relevant Files
| File | Role | Why Relevant |
|------|------|--------------|
| path/to/file.ext | Controller / Component / Model / etc | [1 sentence] |

## Relevant Code Snippets
For each relevant file, paste ONLY the relevant function/method/class —
not the entire file.

### [filename]
\`\`\`[language]
// Only the relevant part
\`\`\`

## Data Flow (existing)
[Describe how the closest existing feature works, step by step]
1. User does X
2. Frontend calls Y
3. Backend does Z
4. Returns W

## What Needs to Change
- [ ] New file needed: [path] — [purpose]
- [ ] Modify existing: [path] — [what changes]
- [ ] New DB migration: [table] — [columns]
- [ ] New API endpoint: [METHOD /path] — [purpose]

## Open Questions
- [ ] [anything unclear that the developer needs to answer]

## Constraints & Rules
- [things that must NOT change]
- [conventions to follow from the existing code]
```

---

## PHASE 2 — IMPLEMENTATION PLANNING

### Step 2.1 — Break into atomic tasks

Convert the "What Needs to Change" list into ordered tasks.
Each task must be:
- **One file or one function** — never multiple files in one task
- **Independently testable** — can be verified before moving on
- **Sequenced correctly** — database first, then backend, then frontend

Format:

```
IMPLEMENTATION TASKS
--------------------
[ ] Task 1: Create migration for [table/column]
    File: database/migrations/xxxx_xxxx.php
    Test: php artisan migrate runs without error

[ ] Task 2: Update [Model] with new relationship/fillable
    File: app/Models/[Model].php
    Test: Model can be instantiated, relationship works

[ ] Task 3: Create/update [Controller method]
    File: app/Http/Controllers/[Controller].php
    Test: API returns correct response for happy path

[ ] Task 4: Update [Vue component]
    File: src/components/[Component].vue
    Test: Component renders, emits correct events

[ ] Task 5: [Flutter screen/widget]
    File: lib/features/[feature]/[screen].dart
    Test: Widget builds, shows correct state
```

### Step 2.2 — Brainstorm blockers (optional, when developer is stuck)

If the developer says *"aku stuck"* or *"bingung gimana caranya"*, use this:

```
BRAINSTORM HELPER
-----------------
The problem: [restate what the developer is stuck on]

Option A — [simplest approach]
  Pro: [why it works]
  Con: [tradeoff]
  How: [1–3 sentence implementation sketch]

Option B — [alternative approach]
  Pro: [why it works]
  Con: [tradeoff]
  How: [1–3 sentence implementation sketch]

Option C — [creative/unconventional approach, if relevant]
  Pro: ...
  Con: ...
  How: ...

Recommendation: Option [X] because [reason], especially given
[relevant constraint from context].
```

---

## PHASE 3 — EXECUTION RULES

When writing actual code, follow these rules to minimize errors:

### Rule 1 — One task at a time
Complete Task 1 fully before starting Task 2.
After each task: *"Task 1 done. Mau lanjut Task 2?"*

### Rule 2 — Show diff, not full file
When modifying an existing file, show ONLY what changed:

```
// BEFORE (line 45–52):
[old code]

// AFTER:
[new code]
```

Don't rewrite the entire file unless creating a new one.

### Rule 3 — Flag assumptions explicitly
If you assumed something that wasn't confirmed, say it:

```
⚠️ ASUMSI: Aku asumsiin kolom `user_id` udah ada di tabel `orders`.
   Kalau belum, perlu tambah migration dulu.
```

### Rule 4 — Never break existing features
Before modifying a file, check: *"Does any existing feature depend on
the part I'm changing?"* If yes, flag it before changing.

### Rule 5 — Minimal footprint
Only create/modify what's in the task list. No refactoring,
no "improvements", no style changes unless asked.

---

## PHASE 4 — HANDOFF

When all tasks are complete, generate a summary:

```
IMPLEMENTATION SUMMARY
----------------------
Feature: [name]
Status: Complete / Partial (list what's missing)

Files Created:
- [path] — [purpose]

Files Modified:
- [path] — [what changed]

How to Test:
1. [step]
2. [step]
3. Expected result: [X]

Known Limitations / Follow-up needed:
- [anything left out intentionally]
- [edge cases not handled]
```

---

## QUICK REFERENCE — Trigger phrases

Use this skill when the developer says:
- "bos minta fitur..."
- "ada request baru..."
- "implement / tambah / bikin fitur..."
- "gimana caranya bikin X..."
- "aku stuck di fitur..."
- "tolong bantu implement..."
