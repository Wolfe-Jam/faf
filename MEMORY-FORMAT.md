# `.fafm` — Voice Memory Layer Format Specification

**Version:** 1.0
**Status:** Stable
**MIME Type (proposed):** `application/vnd.fafm+yaml`
**File Extension:** `.fafm`
**IANA Registration:** Planned
**Last Updated:** 2026-05-01

---

## 1. Purpose

`.fafm` is the **Voice Memory Layer (VML)** format. It provides persistent, mutating memory for voice agents that survives across sessions, devices, and model changes.

It is the voice-specific sibling of the IANA-registered `.faf` format (`application/vnd.faf+yaml`).

**Design Goals**
- Simplicity first — minimal cognitive load for implementers and users
- Human-readable and append-only by default
- Namepoint-centric addressing
- Clear separation of concerns: `.faf` = project facts, `.fafm` = agent memories

*In the FAF family: `.faf` carries **facts** (what the project IS). `.fafm` carries **memories** (what was said, decided, remembered).*

---

## 2. Core Concepts

| Term          | Definition |
|---------------|----------|
| **Namepoint** | Permanent identity/address (`@handle`) for memory storage |
| **Etch**      | Write operation that stores memory |
| **Recall**    | Read operation that retrieves relevant memory |
| **Session**   | One continuous voice interaction |

---

## 3. File Structure

A `.fafm` file is a single YAML document with the following structure:

```yaml
version: "1.0"
namepoint: "@example-user"
created: "2026-04-30T12:00:00Z"
last_etched: "2026-04-30T17:22:00Z"
retention: "forever"

memory:
  facts: []           # string | { text, tags? }
  sessions: []        # reserved
  preferences: {}
  custom: {}
```

**Required Fields:**
- `version`
- `namepoint`
- `created`
- `last_etched`
- `memory`

**Optional Fields:**
- `retention`
- `preferences`
- `custom`

---

## 4. Facts — Two Allowed Forms

**Simple (recommended default):**
```yaml
facts:
  - "User prefers short answers"
  - "User's name is Alex"
```

**With optional tags (advanced):**
```yaml
facts:
  - text: "User prefers short answers"
    tags: ["style", "preference"]
  - text: "User's name is Alex"
    tags: ["personal"]
```

---

## 5. Retention Policy

Top-level field with the following allowed values:

- `"forever"` (default — user-controlled)
- `"30d"`, `"90d"`, `"1y"`
- `"session-only"`

**Implementations MUST expose a `forget(namepoint, criteria)` method** supporting deletion by entry ID, time range, or full wipe.

---

## 6. Core Operations

| Operation     | Description                              | Status     |
|---------------|------------------------------------------|------------|
| `etch memory` | Store fact(s) into namepoint             | Shipped    |
| `recall`      | Retrieve relevant memory for session     | Shipped    |
| `etch session`| Store full session summary               | Planned (v0.2) |
| `etch all`    | Continuous background capture            | Planned (v0.3+) |

---

## 7. Minimal Valid Example

```yaml
version: "1.0"
namepoint: "@demo"
created: "2026-04-30T12:00:00Z"
last_etched: "2026-04-30T12:00:00Z"
retention: "forever"
memory:
  facts: []
```

---

## 8. Relationship to `.faf`

```
.faf    →  Foundational Context Layer    (project IS — static, read once)
.fafm   →  Voice Memory Layer (VML)      (agent REMEMBERS — mutating, persisted)
```

Two formats. Two lifecycles. One ecosystem.

`.faf` is read once and trusted as the project's canonical context. `.fafm` accretes over time as the agent and user interact, with entries appended, summarized, and selectively promoted into `.faf` when they become enduring facts.

Both share the same family branding, YAML philosophy, and MIT license. `.fafm` schema **extends** `application/vnd.faf+yaml` — a `.fafm` document remains a valid YAML file. Consumers that only understand `.faf` will safely ignore the `memory` block and still extract identity from `namepoint` and top-level fields. Consumers that understand `.fafm` gain access to accumulated memory.

---

## 9. Security & Privacy Considerations

### Storage

- Local default: `~/.grok-faf-voice/identity.json` (0600 permissions)
- MCPaaS backend: encrypted at rest
- Namepoint is the sole addressing key
- Memory is **never** deleted by voice commands (UI-only for hide/archive/delete)

### User control

- Implementations **MUST** expose a `forget(namepoint, criteria)` method supporting deletion by entry ID, time range, or full wipe.
- `.fafm` files **MUST NOT** contain secrets, API keys, or credentials.
- Files **MUST** be transmitted over authenticated, encrypted channels (e.g., TLS).

### Untrusted input (context poisoning, prompt injection)

`.fafm` files are written from voice and AI interactions and **MUST** be treated as untrusted user input by consumers. Implementations **MUST NOT** elevate `.fafm` content to "System Instructions" or "Developer Directives" within prompt hierarchies. Memory entries are a documented vector for context poisoning and prompt injection.

### YAML safe-load

Parsers **MUST** disable custom type construction (no `!!python/object`, `!ruby/object`, etc.), enforce alias expansion limits to mitigate entity-expansion attacks, and enforce nesting depth limits to prevent stack overflow.

### Cross-context isolation

Implementations **SHOULD** scope memory consumption to the originating namepoint to prevent unintended cross-context influence between projects, agents, or users.

---

## 10. Versioning

- Uses semantic versioning in the `version` field
- Breaking changes only on major version bumps
- Implementations must remain forward-compatible with older minor versions

---

## 11. File Conventions

- **File extension:** `.fafm`
- **Encoding:** UTF-8
- **Standard filename:** `project.fafm` (alongside `project.faf` when used at the project layer)
- **Identity-anchored deployment:** served from a soul-storage backend keyed by namepoint
- **Multiple `.fafm` documents per scope are permitted**, distinguished by namepoint

---

## 12. IANA Registration Notes (Planned)

**Type name:** application  
**Subtype name:** vnd.fafm+yaml  
**Required parameters:** None  
**Optional parameters:** version  
**Encoding considerations:** 8bit (UTF-8; binary content base64-encoded inside YAML)  
**Security considerations:** See §9  
**Interoperability considerations:** See §8  
**Published specification:** This document; reference implementation at https://github.com/Wolfe-Jam/grok-faf-voice  
**Applications that use this media type:** Voice agents, LiveKit deployments, Grok Voice SDKs, future ElevenLabs/Hume integrations  
**Fragment identifier considerations:** None  
**Additional information — File extension:** `.fafm`  
**Person & email address for further information:** James Wolfe, team@faf.one  
**Intended usage:** COMMON  
**Restrictions on usage:** None  
**Author:** James Wolfe  
**Change controller:** FAF Foundation  

---

## 13. Reference Implementation

`grok-faf-voice` (PyPI, GitHub) is the reference implementation of the Voice Memory Layer. It exposes the `FAFMemory` class, which reads and writes `.fafm` files in this format.

**Local default storage path:** `~/.grok-faf-voice/identity.json` (mode 0600)  
**Remote storage:** MCPaaS soul backend keyed by namepoint

| Surface | Path / Identifier |
|---------|-------------------|
| PyPI    | `grok-faf-voice` |
| GitHub  | https://github.com/Wolfe-Jam/grok-faf-voice |
| Class   | `FAFMemory` |
| Operations | `etch`, `recall` (see §6) |

---

## 14. Future Versions

Future versions may add:

- Cryptographic signing of memory entries (provenance, audit trail)
- Selective sync / partial-load protocols for large memory files
- Time-to-live and expiration semantics on individual entries
- Schema extensions for paralinguistic, session-ledger, and scratchpad entry types

---

## 15. Change History

| Version | Date       | Changes |
|---------|------------|---------|
| 0.1     | 2026-04-30 | Initial draft |
| 0.2     | 2026-05-01 | Added `retention`, tagged facts support, minimal example, IANA notes |
| 0.3     | 2026-05-01 | IANA-readiness pass: full RFC 6838 §4.6 template fields, expanded §9 (untrusted-input / YAML safe-load / cross-context isolation), retention default annotated with user-control note, change controller = FAF Foundation, contact = team@faf.one |
| 0.4     | 2026-05-01 | Quality pass: §8 layer-position diagram + schema-extension note, new §11 File Conventions, new §13 Reference Implementation (grok-faf-voice). Numbering shifted: IANA Notes §11→§12, Change History §12→§14. |
| 0.5     | 2026-05-01 | Added §1 cognitive frame line (facts vs memories) and §14 Future Versions (4 roadmap items: signing, selective sync, TTL, schema extensions). Change History renumbered §14→§15. |
| 0.6     | 2026-05-01 | Polish pass: added Design Goals subsection, strengthened forget-mechanism requirement in §5, clarified schema-extension language in §8, minor formatting and consistency improvements. |
| 1.0     | 2026-05-01 | Released as initial stable specification for IANA media-type registration. |

---

**End of Specification.**
