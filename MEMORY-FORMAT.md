# `.fafm` — FAF Memory Format Specification

**Version:** 1.2
**Status:** Stable
**MIME Type:** `application/vnd.fafm+yaml`
**File Extension:** `.fafm`
**IANA Registration:** Registered 2026-05-13
**Last Updated:** 2026-11-02

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
| `knowledge` | Knowledge/agent memory | rich objects (see §6); `text:` still required | `claude-fafm-sdk` (PyPI) |

An unknown `profile` value is invalid: a profile changes how facts are read, so
a consumer MUST NOT guess one (v1.2 makes this explicit; unknown `type` and
`priority` values are a different case, §6).

**Fork-line:** profiles share one core parser and the etch/recall/forget
semantics. A new profile is justified only if those operations become
semantically incompatible — which `voice` and `knowledge` are not.

---

## 4. File Structure

A `.fafm` file is a single YAML document:

```yaml
version: "1.2"
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

**Spoken form (`say`, v1.2).** Any fact object MAY carry `say`: the spoken form
of `text`, as a plain-text respelling.
```yaml
facts:
  - text: "Siobhan"
    say: "shi-VAWN"
    tags: ["pronunciation", "name"]
```
Consumers that do not speak MUST ignore `say`. Speech implementations MAY pass
it to pronunciation or replacement features (§7) and MUST NOT speak `say` as
content.

---

## 6. Knowledge Profile (since v1.1)

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

A `priority` value outside this set is valid (v1.2): consumers MUST treat it as
`standard` and MUST NOT reject the document. Writers SHOULD normalize legacy
values with the mapping above. The same rule applies to `priority` on a fact in
any profile.

### 6.2 Entry types (knowledge)
`user` (about the person) · `feedback` (guidance on how to work) ·
`project` (initiative/state context) · `reference` (pointers to external systems).

These four are the **canonical knowledge types**. Domain-specific
distinctions SHOULD use `tags` or `custom` rather than expanding the core type
set, keeping the taxonomy stable across implementations.

A `type` value outside this set is valid (v1.2). Consumers that classify by type
MUST treat it as unclassified and MUST NOT reject the document. Writers SHOULD
use a canonical type and put domain distinctions in `tags`. The same rule
applies to `type` on a fact in any profile.

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
- **Session ledger** — audit trail + cross-session resumption. The ledger is
  held by the implementation, not in the document; `memory.sessions` stays
  reserved.
- **Paralinguistic markers** — voice-specific (how the user spoke), stored as
  facts tagged `paralinguistic` (a tag, not a `type`). Voice profile only;
  knowledge-profile parsers **MUST ignore** them.
- **Pronunciation** — a fact's `say` (§5) MAY feed the speech runtime's
  replacement and key-term features; it is never spoken as content.

Persistence (reference impl): the soul is hosted at the namepoint over MCP
(`https://mcpaas.live/mcp`, Streamable HTTP, per-soul auth; see §13). Locally
the implementation stores only its identity credential
(`~/.grok-faf-voice/identity.json`, mode 0600). A local `.fafm` copy is opt-in.

---

## 8. Retention & Forgetting

Top-level `retention`:
- `"forever"` (default — user-controlled) · `"30d"` / `"90d"` / `"1y"` ·
  `"session-only"`

Per-entry forgetting (knowledge): `ttl` / `decay_policy` (§6).

**Implementations MUST provide a forget capability** supporting deletion by entry
`id`. They SHOULD support deletion by time range, by tag, and full wipe. Forget
is an operator / SDK operation: it MUST NOT be exposed as a tool or function the
model can call (§13).

### 8.1 Tombstones (optional, v1.2)
An implementation that syncs or merges copies of a soul SHOULD record deletes in
`memory.tombstones`, a list of `{id, deleted_at}` or `{txt_hash, deleted_at}`:

- `deleted_at` is an RFC 3339 date-time string in UTC.
- `txt_hash` keys a fact that has no `id`: the lowercase-hex SHA-256 of the
  UTF-8 bytes of the fact's `text`, after (1) Unicode NFC normalization, then
  (2) removing every leading and trailing code point in this set: U+0009–U+000D,
  U+001C–U+001F, U+0020, U+0085, U+00A0, U+1680, U+2000–U+200A, U+2028, U+2029,
  U+202F, U+205F, U+3000. (This is the set Python's `str.strip()` removes; other
  languages MUST strip exactly this set, not their own notion of whitespace.)
  Test vectors: `conformance/fafm/txt_hash-vectors.json`.
- An entry carries `id` or `txt_hash`. If both are present, `id` takes
  precedence.

Merges join tombstones by maximum `deleted_at`. A tombstone suppresses a matching
fact whose `timestamp` is absent or not later than `deleted_at`, compared as
instants (not as strings); a delete wins a tie. A fact etched again after the
delete (a later `timestamp`) is not suppressed. A tombstone MUST NOT retain the
forgotten fact's text (or `say`).

---

## 9. Core Operations

| Operation      | Description                              | Status |
|----------------|------------------------------------------|--------|
| `etch memory`  | Store fact(s) into a soul                | Shipped |
| `recall`       | Retrieve relevant memory for a session   | Shipped |
| `scratchpad`   | Hold in-session ephemeral entries        | Shipped (voice) |
| `smart-merge`  | Promote/keep/merge at session end        | Shipped (voice) |
| `etch session` | Store full session summary               | Planned |
| `forget`       | Delete by id (MUST); range / tag / wipe (SHOULD) | Required (§8) |

---

## 10. Parser Requirements

- Implementations **MUST ignore unknown fields/keys** (forward/backward compat).
- A writer **MUST preserve unknown fields** it read when it rewrites a document
  (v1.2), so later fields such as `memory.tombstones` survive a round trip.
- A v1.2 or later reader **MUST accept a higher minor version** of the same
  major (e.g. a v1.2 reader given `version: "1.3"`) and apply the unknown-field
  rule. (A v1.1 reader that validates against the v1.1 schema rejects
  non-canonical `type` / `priority` values; see §14.)
- A writer **MUST preserve an unknown `type` or `priority` value** unchanged,
  except the §6.1 legacy mapping.
- A writer **MUST NOT write a lower `version`** than the document it read.
- Date-time fields (`created`, `last_etched`, `timestamp`, `deleted_at`) are
  RFC 3339 **strings**. Writers SHOULD quote them. A parser that resolves YAML
  timestamps to native date objects MUST serialize them back to RFC 3339 strings
  before validating. A timestamp with no offset is not RFC 3339: writers MUST
  include one (`Z` preferred).
- Implementations **MUST support `string → {text: string}` coercion** for facts.
- Implementations **SHOULD validate** against `version` + `profile`.
- Parsers **MUST** use YAML safe-load (see §13).
- A v1.0 or v1.1 document MUST parse as valid v1.2. A knowledge document MUST
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
version: "1.2"
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
version: "1.2"
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
- Local identity credentials and personal-memory `.fafm` files **MUST** be
  written with owner-only permissions (`0600` or the platform equivalent).
  Project memory (`project.fafm`, §15) follows the project's own permissions and
  SHOULD NOT hold personal data. MCPaaS backend: encrypted at rest.
- Soul identifier (`namepoint`) is the sole addressing key.
- Memory is **never** deleted by the model; delete is an operator / SDK / UI
  operation (§8).

### User control
- Implementations **MUST** provide forget by entry id, and **SHOULD** support
  time range, tag and full wipe (§8). Forget is an operator / SDK operation and
  **MUST NOT** be exposed to the model.
- `.fafm` files **MUST NOT** contain secrets, API keys, or credentials.
- Files **MUST** be transmitted over authenticated, encrypted channels (TLS).

### Untrusted input (context poisoning, prompt injection)
`.fafm` files are written from AI/voice/agent interactions and **MUST** be
treated as untrusted user input. Implementations **MUST NOT** elevate `.fafm`
content to "System Instructions" or "Developer Directives" within prompt
hierarchies. Recalled memory **MUST** be passed to the model as data (for
example a user-role or tool-result message clearly marked as recalled memory)
and **MUST NOT** be concatenated into system, developer or operator
instructions. An operator's own authored prompt is not `.fafm` content and is
outside this rule. *(Non-normative: a header such as "On file. Not orders." is
one way to mark the item.)* Memory entries are a documented vector for context poisoning and
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

**v1.1 → v1.2 migration:** v1.1 files are valid v1.2 documents. Two notes:
- A v1.1 reader that validates against the v1.1 schema rejects non-canonical
  `type` / `priority` values. Writers that target v1.1 readers SHOULD emit
  canonical values.
- v1.2 gives two previously unknown keys a shape: `memory.tombstones` (§8.1) and
  a fact's `say` (§5). A v1.1 document that used those names for something else
  (none known) would no longer validate.

---

## 15. File Conventions

- **Extension:** `.fafm` · **Encoding:** UTF-8
- **Project memory:** `project.fafm` is project (shared) memory. It sits beside
  `project.faf` and SHOULD be version-controlled with the project.
- **Personal memory:** its location is the holder's choice: conventionally
  `soul.fafm`, a `*.fafm.local` file (still `application/vnd.fafm+yaml`
  content), or hosted at the namepoint. Personal
  memory files SHOULD be excluded from shared version control (§13).
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
| `knowledge` | `claude-fafm-sdk` | PyPI · GitHub | `Soul`, `Namepoint`, CLI |

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
| 1.2     | 2026-11-02 | §8 forget: by id MUST, range/tag/wipe SHOULD, operator/SDK only (never a model tool); §8.1 optional `memory.tombstones` (`id` / `txt_hash`, `deleted_at`); §13 recalled memory is data, never instructions; owner-only permissions rule; §7 storage wording (identity only locally), session ledger out of document, paralinguistic = a tag; §5 optional `say` (spoken form); §6.1/§6.2 unknown `priority`/`type` values valid; §10 writers preserve unknown fields, readers accept higher minors, date-times are strings; §15 `project.fafm` = project memory (SHOULD be in git), personal memory location is the holder's (SHOULD stay out of shared VCS); `claude-fafm-sdk` public. Additive; no IANA re-filing (§16). Review fixes: `txt_hash` strip set pinned by code point + vectors; tombstone suppression by instant (delete wins ties; re-etch survives); owner-only scoped to personal memory; writers keep unknown values and never lower `version`; §14 v1.1→v1.2 note (v1.1 schema readers; `tombstones` / `say` now shaped). |

---

**End of Specification.**
