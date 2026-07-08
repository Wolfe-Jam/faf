# `.faf` Context Ingestion — Contract Specification

**Version:** 0.1 (Working Draft)
**Status:** Draft — co-sketched in the open
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

## 6. Projection — first reference implementation: Grok

The **projection rules** map the Agent Context onto a provider's surfaces while keeping the `.faf` as the single source and preserving guardrails. The first reference implementation is **Grok**, via `grok-faf-mcp` — the live proof point.

| Agent Context | Grok surface |
|---------------|--------------|
| `six_ws` + `goal` + `stack` | the orientation block of the prompt / context template |
| `commands` + `definition_of_done` | tool schemas — callable actions plus the finish line |
| `guardrails` (always / ask-first / never) | prompt policy + tool preconditions |
| `readiness` | a live `faf_score` / `faf_validate` tool |
| `provenance` | a pinned session memory slot, so parallel sub-agents share one base |

Other providers (Claude, Cursor, …) implement the **same contract** against their own surfaces. **Grok is first; the contract stays neutral.**

*The general (provider-agnostic) projection rules are the next piece to co-sketch — Grok surfaces here are the first worked example.*

## 7. Prototype evidence

This contract describes what the FAF toolchain already does:

- **`faf-cli`** — `export --agents`, `git owner/repo`, `score`, `diff`, `sync`: ingestion and projection already run end to end.
- **`grok-faf-mcp`** — URL-based ingestion + live context tools. The Grok proof point.
- **Per-group guides** (Grok · Claude · Bun) — tailored consumption of the same object.
- IANA media type · 100,000+ downloads across the FAF ecosystem · deterministic Trophy scoring.

## 8. Out of scope for v0.1

- **Emission templates** — rendering AGENTS.md / CLAUDE.md from the object (a later piece).
- **Scoring rules and weights** — implemented in faf-cli; not part of the ingestion contract.
- **`.fafb` binary internals** — see BINARY-FORMAT.md.
- **Discovery, hosting, auth, registry** — out of band.

## 9. Status & collaboration

v0.1 is a **working draft**, co-sketched in the open with @grok (X, 2026-07-08). Provider mappings and feedback are welcome via issues and discussions. `.faf` is MIT-licensed and IANA-registered; this contract is open under the same terms.

*Ingest once. Orient any agent. Re-explain never.*
