# `.faf` Context Ingestion — Contract Specification

**Version:** 0.2 — sealed with @grok (2026-07-09)
**Status:** Open standard, co-sketched in public
**Media type:** `application/vnd.faf+yaml` (IANA-registered) · compiled form `.fafb`
**Companion specs:** `.faf` → SPECIFICATION.md · `.fafa` → AGENT-FORMAT.md · `.fafm` → MEMORY-FORMAT.md
**Origin:** Proposed by @grok (X, 2026-07-08); co-sketched openly.

---

**What this is** — the *read*: a `.faf`, ingested into one canonical Agent Context object. Provider-agnostic.

**What this isn't** — the format, the scoring engine, or a transport. It composes with MCP and AGENTS.md; competes with none.

---

## 1. Purpose

The minimal, provider-agnostic **contract** for *ingesting* a `.faf`: reading it into a canonical **Agent Context** object, so any agent — Grok, Claude, Cursor, the next one — orients instantly (stack, commands, guardrails, provenance) with no re-explaining.

The `.faf` stays the single source of truth. The object is the **view** a consumer reads it into — never a second source.

## 2. Why this exists

Every MCP server, AGENTS.md generator, and runtime re-invents the same read: *how does an agent learn a repo?* The object model gets respecified endlessly; the neutral **contract** stays unowned.

`.faf` owns the format (IANA). This owns the **read** — one shape every consumer trusts. Provider-agnostic; composes with MCP and AGENTS.md workflows, competes with none.

## 3. Core concepts

| Term | Definition |
|------|------------|
| **Source** | A `.faf` / `.fafb` — file, git ref (`faf git owner/repo`), or URL. The single source of truth. |
| **Agent Context** | The canonical object a consumer reads the source into. Minimal, stable, provider-agnostic. |
| **Consumer** | Anything that ingests: MCP server, AGENTS.md generator, agent runtime. |
| **Projection** | An Agent Context mapped onto a surface — live (§6) or sticky (§7). |
| **Provenance** | Where it came from — git ref + media type. Required. |
| **Readiness** | The `faf score` tier. Observable; optional. |

## 4. The Agent Context object

A consumer reads a `.faf` into this shape — minimal, portable YAML. Field names are the contract's; each maps to the FAF engine beneath.

```yaml
# The canonical Agent Context object — the stable view a consumer reads a .faf into.
# The .faf remains the single source of truth; this is never a second source.
agent_context:
  spec: "faf-context-ingestion/0.2"
  source:
    media_type: "application/vnd.faf+yaml"   # IANA-registered — the neutral anchor
    faf_ref: "git:owner/repo@<commit>"       # a .faf/.fafb path, a git ref, or a URL
    faf_version: "3.0"                        # the .faf format series
    generated_at: "2026-07-08T12:00:00Z"

  six_ws:                    # who/what/why/where/when/how  (maps to .faf human_context)
    who: "..."
    what: "..."
    why: "..."
    where: "..."
    when: "..."
    how: "..."
  goal: "single-line human intent"

  stack:                     # canonical labeled slots — consistent across every surface
    main_language: "TypeScript"
    runtime: "Bun"
    # ... the full canonical slot set; each slot carries its display label

  commands:
    install: "bun install"
    test: "bun test"
    build: "bun run build"
  definition_of_done:        # the checkable finish line — how an agent KNOWS it is done
    - "lint exits 0"
    - "tests pass"

  guardrails:                # three-tier  (maps to .faf ai_instructions)
    always:    ["read files", "run the tests"]
    ask_first: ["dependency installs", "migrations"]
    never:     ["force-push", "commit secrets"]

  provenance:
    git: "owner/repo@<commit>"

  readiness:                 # observable, from faf score — optional for basic ingestion
    score: 100
    tier: "trophy"
```

## 5. Rules

- **Single source.** Everything derives from the `.faf`. Surface facts; never invent.
- **Provenance required.** Every object carries its source (git ref + media type).
- **Consistent labels.** Slots carry display labels; every surface renders alike.
- **Provider-agnostic.** One object serves Grok, Claude, Cursor, the next — unchanged.
- **Non-destructive.** Emitted files preserve hand-written content.
- **Readiness observable, not required.** The score tier travels when present; ingestion doesn't need it.
- **Forward-compatible.** Unknown fields ignored, never rejected.

## 6. Projection — live surfaces

A **projection** maps the object onto a consumer's surfaces — **deterministic and provider-agnostic**: same object, same projection, `.faf` the single source, guardrails preserved.

**Four surfaces** — every consumer has each:

| Surface | What it is | Projected from |
|---------|------------|----------------|
| **Orientation** | the opening context an agent reads before acting | `six_ws` + `goal` + `stack` |
| **Actions** | the callable operations and their success criteria | `commands` + `definition_of_done` |
| **Policy** | what is free, gated, or forbidden | `guardrails` (always / ask_first / never) |
| **Memory** | the base every session and sub-agent shares | `provenance` + `readiness` |

**Rules:**

1. **Deterministic** — same object, same surfaces, every time. No model discretion.
2. **Single source** — reads the object only; adds no fact the `.faf` lacks.
3. **Guardrails enforced, not narrated** — always/ask_first/never become enforceable controls (prompt policy + tool preconditions), not prose an agent waves past.
4. **Non-destructive** — file surfaces keep hand-written content; the projection owns one block.
5. **Labels travel** — slots keep their labels; the fact renders alike everywhere.
6. **Provenance travels** — every surface traces to its `.faf` ref.

### Worked examples

Same four surfaces, two providers — provider-agnosticism proven, not asserted *(Grok row co-sketched with @grok)*:

| Surface | Grok — via `grok-faf-mcp` | Claude — via `claude-faf-mcp` |
|---------|---------------------------|-------------------------------|
| **Orientation** | inject `six_ws` + `goal` + `stack` as the opening context template | write the same block into `CLAUDE.md` + the SessionStart context |
| **Actions** | `commands` as callable tool schemas; `definition_of_done` as each tool's success criteria | `commands` as MCP tools; `definition_of_done` as the done-check |
| **Policy** | `always` / `ask_first` / `never` → system policy + tool preconditions | `always` / `ask_first` / `never` → `CLAUDE.md` rules + tool preconditions |
| **Memory** | pin `provenance` + `readiness` in a shared session slot | persist `provenance` + `readiness` via the `.fafm` memory layer |

Both are live proof points. Cursor, Copilot, and the rest map the same four surfaces to their own primitives. **The mappings differ; the contract does not.**

### The orientation block — worked template

The Orientation surface, concrete — a template rendered *only* from the object. Grok injects it as the prompt's opening; Claude writes it to `CLAUDE.md` + SessionStart. Same shape.

```text
# Orientation — <project>   (generated from project.faf @ <ref> · do not edit)
<goal>

Who    <who>          Where  <where>
What   <what>         When   <when>
Why    <why>          How    <how>

Stack  <main_language> · <runtime> · <framework> · …   (labeled slots)
```

**Deterministic:** same object, same block — no discretion, no invention.

## 7. Projection — sticky surfaces (files)

Projection, one mechanism, two modes. §6 is the **live projections** — surfaces read at runtime (prompt, tools, memory). This is the **sticky projections** — persisted surfaces: the instruction files on disk. A file is a projection that *sticks*: same object, same rules, committed instead of held. **Live vs sticky** — both faithful casts of one source.

> **Define once, project everywhere.** One `project.faf`; every surface — live or on disk — is a faithful projection of it.

**The file family — one object, every surface:**

| File | Consumer |
|------|----------|
| `AGENTS.md` | the open standard every agent reads |
| `CLAUDE.md` | Claude / Claude Code |
| `.cursorrules` | Cursor |
| `GEMINI.md` | Gemini |
| `.github/copilot-instructions.md` | GitHub Copilot |

Each is the *same* context in the shape its consumer expects — never re-authored, never drifting.

**A conformant file projection is:**

1. **Deterministic** — same object, same file, every time.
2. **Sourced, not invented** — every line traces to a field; nothing to hallucinate from.
3. **Auditable** — every line traces to a field, so a reviewer or linter checks the file against the object, line by line. Provable, not claimed.
4. **Non-destructive** — one marked block; surrounding hand-written content survives.
5. **Labeled + provenance-marked** — slots keep their labels; a quiet marker records the source and how to refresh.
6. **Current by re-projection** — change the source, re-project, the file follows. No hand-edit, no drift.

**Reference generator:** `faf export --agents` / `export --all` — shipped. Human field guide: [faf.one/agents](https://faf.one/agents). The projection *rules* are open; a generator's detection and scoring are its own moat.

### Worked example — `project.faf` → `AGENTS.md`

Which object field becomes which section:

| Object field | `AGENTS.md` section |
|--------------|---------------------|
| `six_ws` + `goal` | the orientation line + overview |
| `commands` | `## Setup & build` · `## Run the tests` |
| `definition_of_done` | `## Definition of Done` |
| `guardrails` | `## Guardrails` (Always / Ask-first / Never) |
| `stack` | the labeled stack line |
| `provenance` | the `<!-- faf -->` marker + a "regenerate" note |

The full rendered file (~35 lines, every line sourced) is live at [faf.one/agents](https://faf.one/agents). The same object also emits `CLAUDE.md`, `.cursorrules`, `GEMINI.md`, and `copilot-instructions.md`.

## 8. Prototype evidence

The contract describes what FAF already ships:

- **`faf-cli`** — `export --agents`, `git owner/repo`, `score`, `diff`, `sync`: ingestion + projection, end to end.
- **`grok-faf-mcp`** — URL-based ingestion + live context tools. The Grok proof point.
- **Per-group guides** (Grok · Claude · Bun) — tailored consumption of one object.
- IANA media type · 100,000+ downloads · deterministic Trophy scoring.

## 9. Out of scope

- **Scoring rules and weights** — in faf-cli; not the ingestion contract.
- **`.fafb` binary internals** — see BINARY-FORMAT.md.
- **Discovery, hosting, auth, registry** — out of band.

## 10. Status

- **v0.1 sealed** (2026-07-08) — the object (§4) + live projection (§6), with @grok on X.
- **v0.2 sealed** (2026-07-09) — sticky projection, the file layer (§7).

Open under `.faf`'s MIT + IANA terms. Provider mappings and feedback via issues and discussions.

*Ingest once. Orient any agent. Re-explain never.*
