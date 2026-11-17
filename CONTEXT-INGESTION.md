# `.faf` Context Ingestion — Contract Specification

**Version:** 0.2 (2026-07-09)
**Media type:** `application/vnd.faf+yaml` (IANA-registered) · compiled form `.fafb`
**Companion specs:** `.faf` → SPECIFICATION.md · `.fafa` → AGENT-FORMAT.md · `.fafm` → MEMORY-FORMAT.md
**Credit:** Drafted with Grok in public, July 2026.

---

## 1. Purpose

This is the minimal, provider-agnostic **contract** for *ingesting* a `.faf`: reading it into a canonical **Agent Context** object that gives an agent the project's stack, commands, guardrails, and provenance.

The `.faf` stays the single source of truth. The object is the **view** a consumer reads it into — never a second source.

## 2. Scope

This contract defines the read. It does not define the format, the scoring engine, or a transport. It works alongside MCP and AGENTS.md.

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
- **Provider-agnostic.** One object serves every provider, unchanged.
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

Non-normative. Two example mappings of the same four surfaces:

| Surface | Grok — via `grok-faf-mcp` | Claude — via `claude-faf-mcp` |
|---------|---------------------------|-------------------------------|
| **Orientation** | inject `six_ws` + `goal` + `stack` as the opening context template | write the same block into `CLAUDE.md` + the SessionStart context |
| **Actions** | `commands` as callable tool schemas; `definition_of_done` as each tool's success criteria | `commands` as MCP tools; `definition_of_done` as the done-check |
| **Policy** | `always` / `ask_first` / `never` → system policy + tool preconditions | `always` / `ask_first` / `never` → `CLAUDE.md` rules + tool preconditions |
| **Memory** | pin `provenance` + `readiness` in a shared session slot | persist `provenance` + `readiness` via the `.fafm` memory layer |

Other consumers map the same four surfaces to their own primitives. **The mappings differ; the contract does not.**

### The orientation block — worked template

The Orientation surface as a template, rendered *only* from the object. Every consumer renders the same shape.

```text
# Orientation — <project>   (generated from project.faf @ <ref> · do not edit)
<goal>

Who    <who>          Where  <where>
What   <what>         When   <when>
Why    <why>          How    <how>

Stack  <main_language> · <runtime> · <framework> · …   (labeled slots)
```

## 7. Projection — sticky surfaces (files)

Projection has two modes. §6 covers **live projections**: surfaces read at runtime (prompt, tools, memory). This section covers **sticky projections**: instruction files persisted on disk. A file projection follows the same object and the same rules, and is committed instead of held in a session.

**The file family:**

| File | Consumer |
|------|----------|
| `AGENTS.md` | the open standard every agent reads |
| `CLAUDE.md` | Claude / Claude Code |
| `.cursorrules` | Cursor |
| `GEMINI.md` | Gemini |
| `.github/copilot-instructions.md` | GitHub Copilot |

Each file carries the same context in the shape its consumer expects.

**A conformant file projection is:**

1. **Deterministic** — same object, same file, every time.
2. **Sourced, not invented** — every line traces to a field.
3. **Auditable** — a reviewer or linter can check the file against the object, line by line.
4. **Non-destructive** — one marked block; surrounding hand-written content survives.
5. **Labeled + provenance-marked** — slots keep their labels; a quiet marker records the source and how to refresh.
6. **Current by re-projection** — change the source, re-project, the file follows. No hand-edit, no drift.

**Reference implementation:** `faf export --agents` (AGENTS.md) and `faf export --all` (every format) in faf-cli. Human field guide: [faf.one/agents](https://faf.one/agents).

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

## 8. Implementations

Tools that implement this contract are listed in [IMPLEMENTATIONS.md](IMPLEMENTATIONS.md).

## 9. Out of scope

- **Scoring rules and weights** — in faf-cli; not the ingestion contract.
- **`.fafb` binary internals** — see the [FAFb specification](https://github.com/Wolfe-Jam/faf-rust/blob/main/crates/faf-fafb/BINARY-FORMAT.md).
- **Discovery, hosting, auth, registry** — out of band.

## 10. Status

- **v0.1** (2026-07-08) — the object (§4) and live projection (§6).
- **v0.2** (2026-07-09) — sticky projection, the file layer (§7).

Open under `.faf`'s MIT and IANA terms. Send provider mappings and feedback through issues and discussions.
