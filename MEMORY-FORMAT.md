# `.fafm` — FAF Memory Format Specification

**Version:** 1.1
**Status:** Stable
**MIME Type:** `application/vnd.fafm+yaml`
**File Extension:** `.fafm`
**IANA Registration:** Registered 2026-05-13
**Last Updated:** 2026-05-21

---

## 1. Purpose

`.fafm` is the **FAF Memory format** — persistent, mutating memory for AI that
survives across sessions, devices, and model changes. It is the memory sibling
of the IANA-registered `.faf` context format (`application/vnd.faf+yaml`).

`.fafm` serves multiple **profiles** (see §3):
- **`voice`** — the original **Voice Memory Layer (VML)**: memory for voice
  agents. Reference impl: `grok-faf-voice`.
- **`knowledge`** — typed, cross-linked memory for knowledge/agent runtimes
  (e.g. Claude Code memory). Added in v1.1.

**Design Goals**
- Simplicity first — minimal cognitive load for implementers and users
- Human-readable and append-only by default
- One format, multiple profiles — never fork
- Soul-identifier-centric addressing
- Clear separation of concerns: `.faf` = project facts, `.fafm` = memories

*In the FAF family: `.faf` carries **facts** (what the project IS). `.fafm`
carries **memories** (what was said, decided, learned, remembered).*

---

## 2. Core Concepts

| Term          | Definition |
|---------------|----------|
| **Soul identifier** (`namepoint`) | Permanent identity/address for a memory store. Voice profile uses `@handle` format; other profiles MAY use namespaced forms (e.g. `@claude-code:wolfejam`). |
| **Profile**   | The memory shape/use-case discriminator (`voice` default, `knowledge`). See §3. |
| **Etch**      | Write operation that stores memory |
| **Recall**    | Read operation that retrieves relevant memory |
| **Session**   | One continuous interaction |

---

## 3. Profiles

`.fafm` is **one format with profiles.** The optional top-level `profile:` field
selects the use-case shape. **Absent ⇒ `voice`** (so every v1.0 document is an
implicit voice document — fully back-compatible).

| Profile | Use-case | Facts shape | Reference impl |
|---------|----------|-------------|----------------|
| `voice` *(default)* | Voice agents (VML) | simple — string or `{text, tags?}` | `grok-faf-voice` |
| `knowledge` | Knowledge/agent memory | rich objects (see §6); `text:` still required | `claude-fafm-sdk` *(forthcoming)* |

**Fork-line:** profiles share one core parser and the etch/recall/forget
semantics. A new profile is justified only if those operations become
semantically incompatible — which `voice` and `knowledge` are not.

---

## 4. File Structure

A `.fafm` file is a single YAML document:

```yaml
version: "1.1"
profile: "knowledge"          # OPTIONAL — absent ⇒ "voice"
namepoint: "@example"         # soul identifier
created: "2026-04-30T12:00:00Z"
last_etched: "2026-04-30T17:22:00Z"
retention: "forever"
index: []                     # OPTIONAL (knowledge) — one-line pointers

memory:
  facts: []                   # see §5 (voice: simple) / §6 (knowledge: rich)
  sessions: []                # reserved
  preferences: {}
  custom: {}
```

**Required Fields:** `version`, `namepoint`, `created`, `last_etched`, `memory`
**Optional Fields:** `profile`, `retention`, `index`, `preferences`, `custom`

---

## 5. Facts — the memory unit

`memory.facts` is the canonical container across all profiles. Voice consumers
read `.text` (or coerce a bare string); richer consumers read additional fields.

**Simple (voice default / recommended baseline):**
```yaml
facts:
  - "User prefers short answers"
  - "User's name is Alex"
```

**With optional tags:**
```yaml
facts:
  - text: "User prefers short answers"
    tags: ["style", "preference"]
```

In the `knowledge` profile, facts MAY be rich objects (§6). `text:` is always
required so any consumer can read the unit. A single `facts` array MAY mix bare
strings, `{text, tags?}` objects, and rich objects; parsers **MUST** handle all
three forms in the same array.

---

## 6. Knowledge Profile (v1.1)

When `profile: knowledge`, each fact MAY carry these fields. **All are optional
except `text`** — a knowledge document remains a valid v1.0 document.

```yaml
memory:
  facts:
    - text: "<the memory>"                 # REQUIRED
      id: "<slug>"                          # stable unique key
      type: "user|feedback|project|reference"
      priority: "ephemeral|standard|high|critical"
      tags: ["..."]
      links: ["<other-ids>"]                # cross-references → graph
      timestamp: "2026-05-21T00:00:00Z"
      source: "<provenance>"
      # --- reserved scale fields (all OPTIONAL; defined now to avoid a
      #     breaking revision later — anti memory-rot/poisoning) ---
      version_id: "<opaque>"                # entry revision / etag
      provenance: []                        # lightweight chain / parent refs
      parent_id: "<id>"                     # merge lineage
      derived_from: ["<ids>"]
      etched_by: "<human|ai|agent_id>"
      confidence_score: 0.0                  # 0.0–1.0
      verification_status: "unverified|verified|disputed"
      ttl: "<duration>"                      # or:
      decay_policy: "<name>"                 # forgetting / auto-prune
      conflict_metadata: {}                  # who-overwrote-what
      embedding_fingerprint: "<hash>"        # semantic dedup w/o vector DB
      signature: "<merkle|sig>"              # optional light provenance
```

**`index`** (optional top-level): one-line pointers for fast scanning, mirroring
a curated index (e.g. `MEMORY.md`-style):
```yaml
index:
  - "<id> — <one-line hook>"
```

### 6.1 Priority vocabulary (canonical)
`ephemeral | standard | high | critical`

Legacy `grok-faf-voice` `ScratchpadEntry` mapping:
`low → ephemeral` · `medium → standard` · `high → high` · (+ `critical`).
Implementations SHOULD ship a migration helper applying this mapping.

### 6.2 Entry types (knowledge)
`user` (about the person) · `feedback` (guidance on how to work) ·
`project` (initiative/state context) · `reference` (pointers to external systems).

These four are the **canonical knowledge types** for v1.1. Domain-specific
distinctions SHOULD use `tags` or `custom` rather than expanding the core type
set, keeping the taxonomy stable across implementations.

---

## 7. Voice Profile Behaviors (reference-impl reality)

The `voice` profile's reference implementation (`grok-faf-voice`, `FAFMemory`)
implements memory semantics richer than the baseline `facts: []`. Documented
here so the spec is the single source of truth (no spec↔impl drift).

- **Scratchpad** — in-session ephemeral entries (`value`, `priority`,
  optional `tag`) held before promotion.
- **Smart Merge** — at session end, an LLM-mediated decision per scratchpad
  entry: `promote` (→ soul) · `keep_ephemeral` (discard) · `merge_into`
  (combine). Decisions carry `rationale` + `confidence`.
- **Soul** — the durable, promoted memory (the persisted `.fafm`).
- **Session ledger** — audit trail + cross-session resumption.
- **Paralinguistic markers** — voice-specific (how the user spoke). Voice
  profile only; knowledge-profile parsers **MUST ignore** them.

Persistence (reference impl): MCP protocol at `https://mcpaas.live/mcp`
(Streamable HTTP), per-soul auth. Local default: `~/.grok-faf-voice/` (0600).

---

## 8. Retention & Forgetting

Top-level `retention`:
- `"forever"` (default — user-controlled) · `"30d"` / `"90d"` / `"1y"` ·
  `"session-only"`

Per-entry forgetting (knowledge): `ttl` / `decay_policy` (§6).

**Implementations MUST expose a `forget(namepoint, criteria)` method** supporting
deletion by entry ID, time range, or full wipe.

---

## 9. Core Operations

| Operation      | Description                              | Status |
|----------------|------------------------------------------|--------|
| `etch memory`  | Store fact(s) into a soul                | Shipped |
| `recall`       | Retrieve relevant memory for a session   | Shipped |
| `scratchpad`   | Hold in-session ephemeral entries        | Shipped (voice) |
| `smart-merge`  | Promote/keep/merge at session end        | Shipped (voice) |
| `etch session` | Store full session summary               | Planned |
| `forget`       | Delete by ID / range / full wipe         | Required (§8) |

---

## 10. Parser Requirements

- Implementations **MUST ignore unknown fields/keys** (forward/backward compat).
- Implementations **MUST support `string → {text: string}` coercion** for facts.
- Implementations **SHOULD validate** against `version` + `profile`.
- Parsers **MUST** use YAML safe-load (see §13).
- A v1.0 document MUST parse as valid v1.1. A v1.1 knowledge document MUST
  remain readable by a v1.0/voice consumer (via `.text`).

### 10.1 Validation
A machine-readable JSON Schema (`schemas/fafm.schema.json`) accompanies this
spec for validator tooling. A reference `fafm validate` command is planned in
the FAF CLI. Implementations SHOULD validate documents against the schema +
`version` + `profile` before etch.

---

## 11. Examples (both profiles)

**Voice (v1.0-compatible):**
```yaml
version: "1.1"
profile: "voice"
namepoint: "@demo"
created: "2026-04-30T12:00:00Z"
last_etched: "2026-04-30T12:00:00Z"
retention: "forever"
memory:
  facts:
    - "User prefers short answers"
    - text: "User's name is Alex"
      tags: ["personal"]
```

**Knowledge:**
```yaml
version: "1.1"
profile: "knowledge"
namepoint: "@claude-code:wolfejam"
created: "2026-05-21T00:00:00Z"
last_etched: "2026-05-21T00:00:00Z"
retention: "forever"
index:
  - "precision-is-power — named tiers > umbrella terms"
memory:
  facts:
    - text: "Replace lossy umbrella terms with named tiers + a quality assertion."
      id: "precision-is-power"
      type: "feedback"
      priority: "high"
      tags: ["copy", "doctrine"]
      links: ["no-made-up-numbers"]
      timestamp: "2026-05-20T00:00:00Z"
      source: "session note"
```

---

## 12. Relationship to `.faf`

```
.faf    →  Foundational Context Layer    (project IS — static, read once)
.fafm   →  FAF Memory                     (agent REMEMBERS — mutating, persisted)
```

Two formats. Two lifecycles. One ecosystem. `.faf` is read once and trusted as
canonical context. `.fafm` accretes over time — entries appended, summarized,
and selectively promoted into `.faf` when they become enduring facts.

Both share family branding, YAML philosophy, and MIT license. A `.fafm` document
remains valid YAML; `.faf`-only consumers safely ignore the `memory` block and
still extract identity from `namepoint` and top-level fields.

---

## 13. Security & Privacy Considerations

### Storage
- Local default: `0600` permissions; MCPaaS backend: encrypted at rest.
- Soul identifier (`namepoint`) is the sole addressing key.
- Memory is **never** deleted by voice/agent commands (UI-only for delete).

### User control
- Implementations **MUST** expose `forget(namepoint, criteria)` (ID / time range
  / full wipe).
- `.fafm` files **MUST NOT** contain secrets, API keys, or credentials.
- Files **MUST** be transmitted over authenticated, encrypted channels (TLS).

### Untrusted input (context poisoning, prompt injection)
`.fafm` files are written from AI/voice/agent interactions and **MUST** be
treated as untrusted user input. Implementations **MUST NOT** elevate `.fafm`
content to "System Instructions" or "Developer Directives" within prompt
hierarchies. Memory entries are a documented vector for context poisoning and
prompt injection. The optional `signature` / `provenance` fields (§6) MAY be
used to attest entry origin.

### YAML safe-load
Parsers **MUST** disable custom type construction (no `!!python/object`, etc.),
enforce alias-expansion limits (entity-expansion attacks), and enforce nesting
depth limits (stack overflow).

### Cross-context isolation
Implementations **SHOULD** scope memory consumption to the originating soul to
prevent cross-context influence between projects, agents, or users.

---

## 14. Versioning & Migration

- Semantic versioning in the `version` field. Breaking changes only on major bumps.
- Implementations remain forward-compatible with older minor versions.

**v1.0 → v1.1 migration:** v1.0 files are already valid v1.1 documents (no action
required). To enrich, add `profile: knowledge` and optional fields under
`memory.facts` (keeping `text:`). No migration tax; no breaking changes.

---

## 15. File Conventions

- **Extension:** `.fafm` · **Encoding:** UTF-8
- **Standard filename:** `project.fafm` (alongside `project.faf` at the project layer)
- **Identity-anchored deployment:** served from a soul-storage backend keyed by
  the soul identifier
- **Multiple `.fafm` documents per scope** permitted, distinguished by soul id

---

## 16. IANA Registration

**Type name:** application
**Subtype name:** vnd.fafm+yaml
**Status:** Registered 2026-05-13
**Required parameters:** None
**Optional parameters:** `version`
**Encoding considerations:** 8bit (UTF-8; binary content base64-encoded in YAML)
**Security considerations:** See §13
**Interoperability considerations:** See §12, §10
**Published specification:** This document; reference implementation at
https://github.com/Wolfe-Jam/grok-faf-voice
**Applications that use this media type:** Voice agents, LiveKit deployments,
Grok Voice SDKs, knowledge/agent memory runtimes
**Person & email for further information:** James Wolfe, team@faf.one
**Intended usage:** COMMON · **Restrictions:** None
**Author:** James Wolfe · **Change controller:** FAF Foundation

*Additive minor versions + optional in-document fields do not require re-filing
(media type + YAML structure unchanged; `version` field handles evolution).*

---

## 17. Reference Implementations

| Profile | Impl | Surface | Class / entry |
|---------|------|---------|---------------|
| `voice` | `grok-faf-voice` | PyPI · GitHub | `FAFMemory` — `etch`, `recall` |
| `knowledge` | `claude-fafm-sdk` *(forthcoming)* | private impl | (Claude memory-tool adapter) |

The `.fafm` **format** is open + registered (anyone may implement). Specific
premium implementations may be independently licensed.

---

## 18. Future Versions

- Cryptographic signing / provenance chains (partially reserved in §6)
- Selective sync / partial-load protocols for large souls
- Optional binary sibling (`.fafmb`) for scale/perf
- Distributed memory-service patterns (sharding, hot/cold tiers) at scale

---

## 19. Change History

| Version | Date       | Changes |
|---------|------------|---------|
| 0.1–0.6 | 2026-04-30/05-01 | Initial drafts → IANA-readiness (see git history) |
| 1.0     | 2026-05-01 | Released as stable spec for IANA registration |
| 1.1     | 2026-05-21 | **Profiles** (`voice` default, `knowledge`); `memory.facts` enrichment (knowledge: id/type/priority/links/timestamp/source + reserved scale fields); §7 Voice Profile Behaviors (spec↔impl reconcile: scratchpad/smart-merge/soul/ledger); §10 Parser Requirements (MUST-ignore-unknown, string→{text} coercion); canonical priority vocab + legacy mapping; soul-identifier framing for `namepoint`; IANA status Planned → Registered (2026-05-13); both-profile examples; migration note. Fully backward-compatible. |

---

**End of Specification.**
