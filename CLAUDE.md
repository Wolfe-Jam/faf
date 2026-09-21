<!-- faf:start -->
<!-- faf: @faf/specification | JavaScript | documentation | .faf (Foundational AI-context Format) - Official format specification -->
<!-- faf: claim=project.faf | family=FAF -->

# CLAUDE.md — @faf/specification

## What This Is

.faf (Foundational AI-context Format) - Official format specification

## Stack

- **Language:** JavaScript

## Context

- **Who:** AI agents, developers, and tool implementers who read or write project.faf — anyone building an implementation that conforms to the format
- **What:** .faf (Foundational AI-context Format) - Official format specification
- **Why:** A portable AI-context standard needs one canonical, IANA-registered specification — the authoritative source for application/vnd.faf+yaml that every implementation conforms to
- **Where:** https://github.com/Wolfe-Jam/faf
- **When:** IANA-registered (application/vnd.faf+yaml, October 30 2025) — Specification 3.3.0
- **How:** Read SPECIFICATION.md and the JSON schemas; reference example.faf and examples/; implementations write and read project.faf files conforming to this spec

---

*STATUS: BI-SYNC ACTIVE — 2026-06-20T01:17:45.287Z*
<!-- faf:end -->
# CLAUDE.md — Wolfe-Jam/faf

## What this is

The **format specifications**. Three IANA-registered media types live here, plus the
schemas and conformance fixtures that let anyone check an implementation.

**There is no executable code in this repo.** It defines the formats; other
repositories implement them.

| File | Defines |
|------|---------|
| `SPECIFICATION.md` | `.faf` — context. `application/vnd.faf+yaml` |
| `MEMORY-FORMAT.md` | `.fafm` — memory. `application/vnd.fafm+yaml` |
| `AGENT-FORMAT.md` | `.fafa` — agents. `application/vnd.fafa+yaml` |
| `CONTEXT-INGESTION.md` | the ingestion contract |
| `BINARY-FORMAT.md` | **a pointer only** — the live FAFb wire spec is in `faf-rust/crates/faf-fafb` |
| `IMPLEMENTATIONS.md` | who implements what, and at which version |
| `schemas/` · `conformance/` · `examples/` | the checkable parts |

---

## Version axes — four numbers, do not conflate them

The most confusing thing about this repo, and getting it wrong has already shipped.

| Number | Versions | Lives in |
|--------|----------|----------|
| **Spec 3.3.0** | the `.faf` format — "the 33" | `SPECIFICATION.md` header |
| **`faf_version: 2.5.0`** | the file being written | stamped inside each `.faf` |
| **FAFb wire v2** | the compiled binary container | `faf-fafb/BINARY-FORMAT.md` |
| **`@faf/specification` 1.1.0** | the npm package of these docs | `package.json` |

One sentence from the FAFb spec ties two together: *"faf-fafb wire v2 implements
FAF-33 (spec 3.3.0)."*

**`package.json` is still 1.1.0 while the spec is 3.3.0.** Bumping it means publishing
to npm — a `/pubpro` job with his GO, never folded into a docs commit.

---

## Where truth lives

This repo is authoritative for **what the format means**. It is not the source for
**what fields exist**:

- **`.faf` structure** → `faf-cli/src/core/types.ts` (`FafData`). The FAFb canonical
  chunk table mirrors it and says so outright.
- **Slot list** → `faf-kernel/src/score.rs` and faf-cli's `src/core/slots.ts`.
- **Wire bytes** → `faf-fafb/BINARY-FORMAT.md`, with `tests/parity/golden.fafb` as the
  byte-exact reference.

Change a field here without checking those and the spec drifts from the thing it
describes — which is exactly the state it was in until 2026-09-20.

---

## The model, in short

Every slot starts **empty**. Three states, never a fourth: `empty`, `slotignored`
(not required for this `app_type`), `populated` (holds a verified fact).

```
score = populated / active × 100        active = 33 − slotignored
```

`app_type` selects which slots are required. faf-cli (MIT, free) scores 21 slots for
every app type; the full 33 adds monorepo and team slots.

---

## Do not reintroduce

`SPECIFICATION.md` was rewritten on 2026-09-20 (`7dcd8aa`) because it described a
format nobody builds. If any of this reappears, it is a regression:

- **Points-based scoring** — type recognition 20, content extraction 50, metadata
  quality 20, AI optimization 10. No implementation ever used it.
- **The extraction file model** — `type:` / `content:` / `confidence:` with
  `package` / `config` / `documentation` / `code` / `data` categories. That described
  extracting one file, not writing project context.
- **`score: 85` / `type: package` / `confidence: 0.92`** as "the core format". It was
  never that in any shipping tool.

---

## Landmines

- **`scores` in a `.faf` is a carried claim, not a result.** A compiler copies it
  through unchanged. Never present it as verification; run a scorer.
- **Examples must actually score.** `example.faf` and `examples/` are read as ground
  truth. Verify by running a scorer, never by writing a plausible number — the old
  spec claimed 95% and 45% against a model that did not exist.
- **`about` is a repo role, not an `app_type`** — and stays out of the public spec.
- **Deliberately absent from `SPECIFICATION.md`:** the 24-type ladder and per-type
  slot counts. The rule is public; the table is not.
- **Docs only.** No build, no tests, no dependencies. The examples are the tests.
- The block at the top of this file is **faf-managed** (`faf:start`/`faf:end`) — it
  comes from `project.faf`. Edit the source, not the mirror.

---

## Publishing

`@faf/specification` goes to npm through **`/pubpro`**, with his GO. Nothing here is
published by hand. Implementers pin against this, so a version bump is a claim about
stability — patch for wording, minor for new optional fields, major for a change in
what the format means.
