# `.faf` Context Ingestion — Contract Specification

**Version:** 0.1 locked (2026-07-08) · 0.2 in draft — persisted-surface projection (§7)
**Status:** Co-sketched in the open with @grok
**Applies to:** `application/vnd.faf+yaml` (IANA-registered) and its compiled form `.fafb`
**Companion specs:** `.faf` → SPECIFICATION.md · `.fafa` → AGENT-FORMAT.md · `.fafm` → MEMORY-FORMAT.md
**Last Updated:** 2026-07-08
**Origin:** Proposed by @grok (X, 2026-07-08); co-sketched openly.

---

## 1. Purpose

Define the minimal, provider-agnostic **contract** for *ingesting* a `.faf` — reading it into a canonical **Agent Context** object — so any agent (Grok, Claude, Cursor, or a future runtime) gets instant, reliable orientation (stack, commands, guardrails, provenance) with **no re-explaining, ever**.

This is the *ingestion* side only: how a consumer reads a `.faf` to bootstrap context. *Emission* — rendering a perfect AGENTS.md / CLAUDE.md from the object — is a separate, later piece.

The `.faf` remains the **single source of truth**. The Agent Context object is the stable **view** a consumer reads it into — never a second source.

## 2. Why this exists

Agent tooling keeps re-solving one problem — "how does an agent learn a repo?" — and every MCP server, AGENTS.md generator, and runtime invents its own read. The object model gets respecified repeatedly; the canonical, provider-neutral *contract* is left unowned.

`.faf` already owns the **format** (IANA-registered). This contract owns the **read**: one shape every consumer can rely on. Provider-agnostic by design — it composes with MCP, AGENTS.md workflows, and agent runtimes, and competes with none.

## 3. Core concepts

| Term | Definition |
|------|------------|
| **Source** | A `.faf` (or compiled `.fafb`) — a local file, a git ref (`faf git owner/repo`), or a URL. The single source of truth. |
| **Agent Context** | The canonical object a consumer reads the source into. Stable, minimal, provider-agnostic. |
| **Consumer** | Anything that ingests: an MCP server, an AGENTS.md generator, an agent runtime. |
| **Projection** | The mapping of an Agent Context onto a provider's surfaces (prompt, tool schema, memory). See §6. |
| **Provenance** | Where the context came from — a git ref plus the media type. Required. |
| **Readiness** | The observable `faf score` tier. Optional for basic ingestion. |

## 4. The Agent Context object

A consumer reads a `.faf` into this shape. It is a minimal, portable YAML projection — field names are the contract's own; each maps to the FAF engine underneath.

```yaml
# The canonical Agent Context object — the stable view a consumer reads a .faf into.
# The .faf remains the single source of truth; this is never a second source.
agent_context:
  spec: "faf-context-ingestion/0.1"
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

## 5. Rules (v0.1)

- **Single source of truth.** Everything derives from the `.faf`. A consumer surfaces detected facts — it never invents.
- **Provenance required.** Every object carries its source (git ref + media type).
- **Consistent labels.** Stack slots carry their display labels, so every surface renders the same way.
- **Provider-agnostic.** The same object serves Grok, Claude, Cursor, and future agents unchanged.
- **Non-destructive downstream.** Emitted files (AGENTS.md, CLAUDE.md, …) preserve hand-written content.
- **Readiness is observable, not required.** The `faf score` tier travels when present; basic ingestion does not depend on it.
- **Forward-compatible.** Unknown fields are ignored, never rejected — a later field cannot break an earlier reader.

## 6. Deterministic Projection Rules (v0.1)

A **projection** maps the Agent Context object onto a consumer's surfaces. The rules are **deterministic and provider-agnostic**: the same object yields the same projection, on any provider, with the `.faf` as the single source and guardrails preserved.

**The four surfaces** — every consumer has some form of each:

| Surface | What it is | Projected from |
|---------|------------|----------------|
| **Orientation** | the opening context an agent reads before acting | `six_ws` + `goal` + `stack` |
| **Actions** | the callable operations and their success criteria | `commands` + `definition_of_done` |
| **Policy** | what is free, gated, or forbidden | `guardrails` (always / ask_first / never) |
| **Memory** | the base every session and sub-agent shares | `provenance` + `readiness` |

**The rules:**

1. **Deterministic** — the same Agent Context object projects to the same surfaces every time. No model discretion in the mapping.
2. **Single source** — a projection reads from the object (which reads from the `.faf`); it never adds a fact the `.faf` does not carry.
3. **Guardrails enforced, not narrated** — `always` / `ask_first` / `never` map to enforceable controls (prompt policy *and* tool preconditions), never to prose an agent can wave past.
4. **Non-destructive** — where a surface is a file, hand-written content survives; the projection maintains only its own block.
5. **Labels travel** — stack slots keep their display labels, so the same fact renders identically on every surface.
6. **Provenance travels** — every projected surface can trace back to the `.faf` ref it came from.

### Worked examples

The same four surfaces, two providers — provider-agnosticism *proven*, not asserted *(Grok row co-sketched with @grok)*:

| Surface | Grok — via `grok-faf-mcp` | Claude — via `claude-faf-mcp` |
|---------|---------------------------|-------------------------------|
| **Orientation** | inject `six_ws` + `goal` + `stack` as the opening context template | write the same block into `CLAUDE.md` + the SessionStart context |
| **Actions** | `commands` as callable tool schemas; `definition_of_done` as each tool's success criteria | `commands` as MCP tools; `definition_of_done` as the done-check |
| **Policy** | `always` / `ask_first` / `never` → system policy + tool preconditions | `always` / `ask_first` / `never` → `CLAUDE.md` rules + tool preconditions |
| **Memory** | pin `provenance` + `readiness` in a shared session slot | persist `provenance` + `readiness` via the `.fafm` memory layer |

Both are live proof points. Cursor, Copilot, and future agents implement the same four surfaces against their own primitives. **The mappings differ per provider; the contract does not.**

### The orientation block — worked template

The Orientation surface made concrete: a provider-agnostic template rendered *only* from the object. Grok injects it as the prompt's opening context; Claude writes it into `CLAUDE.md` + SessionStart; the shape is identical.

```text
# Orientation — <project>   (generated from project.faf @ <ref> · do not edit)
<goal>

Who    <who>          Where  <where>
What   <what>         When   <when>
Why    <why>          How    <how>

Stack  <main_language> · <runtime> · <framework> · …   (labeled slots)
```

**Deterministic:** the same Agent Context object always renders this same block — no model discretion, no invention.

## 7. Projection to persisted surfaces — the files

Projection is one mechanism. §6 covered the **live** surfaces (prompt, tools, memory) an agent reads at runtime. This section covers the **persisted** surfaces — the instruction files written into the repo. A file is simply a projection that *sticks*: same object, same rules, on disk instead of in the session.

> **Define once, project everywhere.** One `project.faf`; every surface — live or on disk — is a faithful projection of it.

**The file family — one object, every surface:**

| File | Consumer |
|------|----------|
| `AGENTS.md` | the open standard every agent reads |
| `CLAUDE.md` | Claude / Claude Code |
| `.cursorrules` | Cursor |
| `GEMINI.md` | Gemini |
| `.github/copilot-instructions.md` | GitHub Copilot |

Each is the *same* context, projected into the shape its consumer expects — never re-authored, never drifting apart.

**A conformant file projection is:**

1. **Deterministic** — the same object renders the same file every time.
2. **Sourced, not invented** — every line traces to a field in the object. A projection reflects its source; it has nothing to hallucinate *from*. (This is the structural answer to the auto-generated-slop finding — a projection cannot pad.)
3. **Non-destructive** — the projection maintains one marked block; hand-written content around it survives untouched.
4. **Labeled + provenance-marked** — slots carry their display labels; a quiet marker records the source and how to refresh.
5. **Current by re-projection** — change the source, re-project, the file follows. No hand-edit, no drift.

**Reference generator:** `faf export --agents` / `export --all` — shipped. The human field guide to a good `AGENTS.md` lives at [faf.one/agents](https://faf.one/agents). The projection *rules* are open and provider-neutral; a generator's own detection and scoring stay its own concern — and its own moat.

## 8. Prototype evidence

This contract describes what the FAF toolchain already does:

- **`faf-cli`** — `export --agents`, `git owner/repo`, `score`, `diff`, `sync`: ingestion and projection already run end to end.
- **`grok-faf-mcp`** — URL-based ingestion + live context tools. The Grok proof point.
- **Per-group guides** (Grok · Claude · Bun) — tailored consumption of the same object.
- IANA media type · 100,000+ downloads across the FAF ecosystem · deterministic Trophy scoring.

## 9. Out of scope

- **Scoring rules and weights** — implemented in faf-cli; not part of the ingestion contract.
- **`.fafb` binary internals** — see BINARY-FORMAT.md.
- **Discovery, hosting, auth, registry** — out of band.

## 10. Status & collaboration

**v0.1 is locked** — the object (§4) + live projection (§6), sealed with @grok on X, 2026-07-08. **§7 — persisted-surface projection (the files) — is the next layer,** drafted here for review. Provider mappings and feedback welcome via issues and discussions. `.faf` is MIT-licensed and IANA-registered; this contract is open under the same terms.

*Ingest once. Orient any agent. Re-explain never.*
