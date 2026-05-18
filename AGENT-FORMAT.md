# `.fafa` — FAF Agent Format Specification

**Version:** 1.0
**Status:** Stable
**MIME Type (proposed):** `application/vnd.fafa+yaml`
**File Extension:** `.fafa`
**IANA Registration:** Planned (queued behind `application/vnd.fafm+yaml`, registered 2026-05-13)
**Last Updated:** 2026-05-18

---

## 1. Purpose

`.fafa` is the **Agent Card** format of the FAF family. It is a structured, vendor-neutral declaration of what an agent **is**: its identity, the capabilities it exposes, and the endpoints through which it is reached.

It is the agent-specific sibling of the IANA-registered `.faf` format (`application/vnd.faf+yaml`) and the IANA-registered `.fafm` format (`application/vnd.fafm+yaml`).

**Design Goals**

- Declarative, never executable — a `.fafa` document describes an agent; it never instructs one
- Vendor-neutral — no dependency on any single agent-interaction protocol
- Composable — sits at the format layer beneath protocols, not in competition with them
- Human-readable, YAML-native, forward-compatible by default
- Clear separation of concerns within the family

*In the FAF family: `.faf` carries what the project **IS** (Foundational Context Layer). `.fafm` carries what the agent **REMEMBERS** (Voice Memory Layer). `.fafa` carries what the agent **IS** (Agent Card).*

---

## 2. Why This Format Exists

Agent-interaction work consistently defines an object model for agent identity, capability, and endpoint — and then defers the serialization format to "some JSON" or an unspecified schema. The object model is specified repeatedly; the on-the-wire, in-the-registry, in-the-repo *format* is left unowned.

`.fafa` fills that gap with a vendor-registered, YAML-native, FAF-family-coherent answer. It does not define discovery, transport, authorization, or orchestration. It defines the **document** those mechanisms carry, reference, and resolve.

The format is complementary by default. It composes with existing and future agent-interaction protocols; it competes with none of them.

---

## 3. Core Concepts

| Term | Definition |
|------|------------|
| **Agent** | An autonomous or semi-autonomous software entity that exposes capabilities and accepts invocation |
| **Capability** | A named, addressable function the agent can perform |
| **Endpoint** | A reachable address plus protocol binding for invoking the agent |
| **Attachment** | (Optional) The infrastructure context within which the agent operates — gateway, domain, policy scope |
| **Provenance** | (Optional) The context substrate the agent reads from — most commonly a `.faf` reference |

---

## 4. File Structure

A `.fafa` file is a single YAML document:

```yaml
version: "1.0"

agent:                       # REQUIRED — identity
  name: faf-agent
  id: "did:web:faf.one:agent"
  vendor: WolfeJAM
  version: "1.0.0"
  description: "Cites the spec or refuses out of scope, without drama"

capabilities:                # REQUIRED — what the agent can do (may be empty)
  - name: validate
    type: tool
    description: "Validate .faf documents against the spec"
    tags: [validation, faf, spec]

endpoints:                   # REQUIRED — how to reach the agent (non-empty)
  - protocol: mcp
    transport: stdio
    location: faf-agent-mcp

attachment:                  # OPTIONAL — infrastructure context
  gateway: ...
  domain: ...
  policy: ...

provenance:                  # OPTIONAL — context substrate
  faf: "./project.faf"
  spec: "application/vnd.faf+yaml"

signature:                   # OPTIONAL — provenance / integrity
  method: ...
  value: ...
```

**Required top-level fields:** `version`, `agent` (with at least `name` and `id`), `capabilities` (array; MAY be empty), `endpoints` (array; MUST be non-empty).

**Optional top-level fields:** `attachment`, `provenance`, `signature`, `metadata` (free-form vendor extensions).

---

## 5. Agent Section

Declares identity.

**Required:** `name` (Unicode string, unique within the issuing vendor namespace), `id` (globally unique identifier — DID, URI, or vendor-scoped UUID).

**Optional:** `vendor`, `version` (semantic version of the agent, NOT of this spec), `description`, `homepage`, `license` (SPDX identifier).

---

## 6. Capabilities Section

Each capability is a structured object.

**Required:** `name` (unique within the document), `type` (`tool` | `resource` | `prompt` | `event` | other).

**Optional:** `description`, `input_schema`, `output_schema`, `tags`, `apophatic` (boolean — if true, the capability refuses out-of-scope invocation rather than attempting best-effort), `cites_spec` (the format/spec the capability operates against, e.g. `application/vnd.faf+yaml`).

`apophatic` and `cites_spec` are FAF-family conventions: capabilities that cite their spec or refuse cleanly.

---

## 7. Endpoints Section

Each endpoint declares a protocol binding.

**Required:** `protocol` (`mcp`, `a2a`, `grpc`, `http`, or other), `transport` (`stdio`, `http`, `websocket`, `quic`), `location` (reachable address — URI, package name, or transport-specific locator).

**Optional:** `version`, `auth`, `region`, `health`.

Multiple endpoints MAY declare the same protocol with different transports.

---

## 8. Attachment Section (Optional)

Describes the infrastructure context within which the agent operates. It does not require any particular gateway protocol.

**Optional:** `gateway`, `domain`, `policy`, `attachment_point`.

---

## 9. Provenance Section (Optional)

Cites the context substrate the agent operates from. This is the FAF-family integration point: a `.fafa` agent SHOULD declare which `.faf` it reads.

**Optional:** `faf` (reference to a `.faf` document — path, URL, or namepoint), `fafm` (reference to a `.fafm` document), `spec` (media type of the substrate), `version`.

---

## 10. Minimal Valid Example

```yaml
version: "1.0"
agent:
  name: example-agent
  id: "did:web:example.com:agent"
capabilities: []
endpoints:
  - protocol: mcp
    transport: stdio
    location: example-agent
```

---

## 11. Relationship to the FAF Family

```
.faf    →  Foundational Context Layer    (project IS — static, read once)
.fafm   →  Voice Memory Layer (VML)      (agent REMEMBERS — mutating, persisted)
.fafa   →  Agent Card                    (agent IS — declarative identity + capability)
```

Three formats. Three roles. One ecosystem.

`.fafa` is **format-level**, not protocol-level. It composes with — does not replace — agent-interaction protocols: a protocol carries, references, or resolves a `.fafa` document; the document declares the agent the protocol acts upon.

The `.fafa` schema **extends** `application/vnd.faf+yaml` — a `.fafa` document remains a valid YAML file. Consumers that only understand `.faf` will safely ignore the agent-specific blocks and still extract identity from top-level fields. Consumers that understand `.fafa` gain the full agent declaration. All three formats share the same family branding, YAML 1.2 / RFC 9512 baseline, and MIT license.

---

## 12. Security & Privacy Considerations

The following addresses the IANA media-type security-considerations checklist.

### Active or executable content

None. A `.fafa` document is YAML data. It contains no scripts, code, or active markup. Implementations MUST treat `.fafa` content as data, never as instructions, commands, or code. Declaration is not authorization: an implementation MUST NOT auto-execute a capability purely because it is declared.

### Privacy and integrity needs

`.fafa` carries identity claims and capability declarations. It is not intended to carry personal data. Integrity of `endpoints` and `signature` is required where present; confidentiality is generally not required for the declaration itself, but transport SHOULD be authenticated and encrypted.

### Privacy and integrity services

The format does not itself provide cryptographic services. Where present, `signature` carries provenance/integrity material; verification is the consumer's responsibility. Implementations MUST treat `endpoints` as untrusted until verified against `signature` if present, and MUST apply least privilege when consuming declared capabilities.

### YAML format considerations

`.fafa` is built on YAML 1.2; all YAML 1.2 / RFC 9512 §6 security considerations apply. Specifically, parsers MUST disable custom type construction (no `!!python/object`, `!ruby/object`, or equivalent), MUST enforce alias-expansion limits to mitigate entity-expansion ("billion-laughs") attacks, and MUST enforce nesting-depth limits to prevent stack overflow.

### Untrusted input / context poisoning / prompt injection

`.fafa` documents originate from agents and third parties and MUST be treated as untrusted input by consumers. Implementations MUST NOT elevate `.fafa` content to "system instructions" or "developer directives" within prompt hierarchies. Capability declarations are a documented vector for over-trust and prompt injection.

### Cross-context isolation

Implementations SHOULD scope capability consumption to the declaring agent's verified identity to prevent unintended cross-agent or cross-domain influence.

### Prohibited content

`.fafa` documents MUST NOT contain secrets, API keys, or credentials; MUST NOT contain user-personal data (use `.fafm` for memory of that kind); and MUST NOT contain executable code. The format is declarative, not executable.

---

## 13. Interoperability Considerations

`.fafa` is YAML 1.2 / RFC 9512 compliant. Consumers:

- MUST use YAML safe-load (no custom type construction)
- MUST treat unknown top-level fields, and unknown subfields within `agent`, `capabilities`, and `endpoints`, as forward-compatible extensions
- SHOULD use UTF-8 encoding
- SHOULD keep documents within common registry storage budgets (recommended < 64 KB)

`.fafa` composes with agent-interaction protocols at the format layer. A protocol that needs an agent-identity-and-capability document MAY use `.fafa` as that document without adopting any other part of the FAF family. A consumer that understands only `application/vnd.faf+yaml` can still parse a `.fafa` file as valid YAML and extract top-level identity.

---

## 14. Versioning

- Semantic versioning in the `version` field
- `0.x` — draft, format may change, not for production
- `1.0` — stable; backwards-compatible changes only within `1.x`
- `2.0` — breaking changes only with strong cause

The top-level `version` field declares which version of this specification the document conforms to.

---

## 15. File Conventions

- **File extension:** `.fafa`
- **Encoding:** UTF-8
- **Standard filename:** `agent.fafa` (alongside `project.faf` when used at the project layer)
- **Multiple `.fafa` documents per scope are permitted**, distinguished by `agent.id`

---

## 16. IANA Registration Notes (Planned)

**Type name:** application
**Subtype name:** vnd.fafa+yaml
**Required parameters:** N/A
**Optional parameters:** version
**Encoding considerations:** 8bit (UTF-8; binary content, if any, base64-encoded inside YAML)
**Security considerations:** See §12
**Interoperability considerations:** See §13
**Published specification:** This document, canonical at `https://github.com/Wolfe-Jam/faf/blob/main/AGENT-FORMAT.md`; reference implementation `faf-agent-mcp` (PyPI)
**Applications that use this media type:** faf-agent-mcp (reference implementation), xAI/Grok, claude-faf-mcp, gemini-faf-mcp and future FAF MCP-family integrations
**Fragment identifier considerations:** N/A
**Additional information — File extension:** `.fafa`
**Person & email address for further information:** James Wolfe, team@faf.one
**Intended usage:** COMMON
**Restrictions on usage:** N/A
**Author:** James Wolfe
**Change controller:** FAF Foundation

---

## 17. Reference Implementation

`faf-agent-mcp` (PyPI) is the reference consumer of `.fafa`. The agent reads its own `.fafa` declaration to advertise capabilities to MCP clients.

| Surface | Path / Identifier |
|---------|-------------------|
| PyPI | `faf-agent-mcp` |
| Canonical spec | `https://github.com/Wolfe-Jam/faf/blob/main/AGENT-FORMAT.md` |
| Family siblings | `application/vnd.faf+yaml`, `application/vnd.fafm+yaml` |

---

## 18. Future Versions

Deferred to successive versions, resolved before any field becomes load-bearing in `1.x`:

- Multiple simultaneous identifier schemes for `agent.id`
- `signature` as an array (multiple signers)
- Hierarchical / grouped capability declaration
- A `lifecycle` field for create / start / stop / destroy semantics
- Content-addressed provenance (binary-form hash) for tamper-evidence
- Expression of degraded / partial capability (e.g., available but rate-limited)

---

## 19. Change History

| Version | Date | Changes |
|---------|------|---------|
| 0.1 | 2026-05-11 | Initial draft — agent / capabilities / endpoints / attachment / provenance structure |
| 1.0 | 2026-05-18 | IANA-readiness pass: full RFC 6838 §4.6 template fields inlined (§16); Security restructured to the media-type security-considerations checklist (§12); Interoperability separated and RFC 9512-anchored (§13); family-relationship section made schema-extension-explicit (§11); competitor-draft enumeration removed (gap stated structurally, not by rivalry); internal-path references removed; change controller = FAF Foundation, contact = team@faf.one; released as initial stable specification for IANA media-type registration |

---

**End of Specification.**
