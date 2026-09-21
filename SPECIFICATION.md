<!-- faf: faf-specification | YAML | specification | .faf Format Specification — the authoritative source for application/vnd.faf+yaml. Defines the file, the 33 slots, and how a score is worked out. -->

# .faf Format Specification

**Specification:** 3.3.0 — "the 33"
**Media type:** `application/vnd.faf+yaml` (IANA-registered, 30 October 2025)
**Status:** Stable
**License:** MIT

## What a .faf file is

`.faf` is to context what `package.json` is to dependencies: one portable file that
says what a project is, how it is built, and why.

It is plain YAML. A human can read it, edit it, and review it in a pull request. It
sits next to `package.json`, not hidden away.

## Filename

**`project.faf`** — visible, beside `package.json`.

`.faf` (hidden) still works and still parses. Tools look for `project.faf` first.

## The file

Required: `faf_version`, and a `project` block with a `name`. Everything else is
optional — you write what is true and leave the rest.

```yaml
faf_version: 2.5.0

project:
  name: awesome-ai-project
  goal: Next-generation AI context optimization engine
  main_language: TypeScript

app_type: fullstack

human_context:
  who: Developers building AI-assisted tooling
  what: An engine that scores and optimizes AI context
  why: AI assistants re-discover project context every session
  where: Web app on Vercel with a Node API
  when: Started 2026; active development
  how: Reads project.faf, scores it, exposes results over REST

stack:
  frontend: React
  css_framework: Tailwind CSS
  ui_library: slotignored
  state_management: Zustand
  backend: Node.js
  api_type: REST
  runtime: Node
  database: PostgreSQL
  connection: Prisma
  hosting: Vercel
  build: Vite
  cicd: GitHub Actions
```

The blocks that describe a project:

| Key | Holds |
|-----|-------|
| `faf_version` | format version of this file |
| `project` | `name` · `goal` · `main_language` |
| `app_type` | what kind of app this is — see below |
| `stack` | the scored technology slots |
| `human_context` | the six Ws |
| `tech_stack` | flat list of technologies |
| `key_files` | important paths |
| `commands` | build / test / lint / dev |
| `monorepo` | packages, orchestration, shared config |
| `architecture` | free-form structural description |
| `scores` | a carried claim, see below |
| `context` | free-form extra signal |

Anything else you write is kept, not discarded. The complete canonical list, and how
each key is sealed into the compiled `.fafb`, is in the
[FAFb Binary Format](https://github.com/Wolfe-Jam/faf-rust/blob/main/crates/faf-fafb/BINARY-FORMAT.md)
specification.

## The 33 slots

A `.faf` is scored by counting slots. There are 33 and the list never changes — that
is what makes two projects comparable.

| Group | Slots | Keys |
|-------|------:|------|
| Project | 3 | `name` · `goal` · `main_language` |
| Human context | 6 | `who` · `what` · `why` · `where` · `when` · `how` |
| Frontend | 4 | `framework` · `css` · `ui_library` · `state` |
| Backend | 5 | `backend` · `api` · `runtime` · `db` · `connection` |
| Delivery | 3 | `hosting` · `build` · `cicd` |
| Monorepo | 5 | `monorepo_tool` · `pkg_manager` · `workspaces` · `packages_count` · `build_orchestrator` |
| Team application | 4 | `admin` · `cache` · `search` · `storage` |
| Team operations | 3 | `versioning_strategy` · `shared_configs` · `remote_cache` |
| **Total** | **33** | |

Longer spellings are accepted and mean the same thing: `frontend` = `framework`,
`css_framework` = `css`, `state_management` = `state`, `api_type` = `api`,
`database` = `db`, `package_manager` = `pkg_manager`.

## How a score is worked out

Every slot starts **empty**. Three states, never a fourth:

- **empty** — the default. Nothing established yet.
- **slotignored** — not required for this app type. Labelled, and does not score.
- **populated** — holds a verified fact.

**`app_type` decides which slots are required.** A CLI tool is not asked about its CSS
framework; a documentation repo is not asked about a database. Slots the type does not
require are labelled `slotignored` and leave the calculation entirely. Nothing is held
against a project for lacking something it was never meant to have.

The label is written into the file, as the slot's value:

```yaml
stack:
  ui_library: slotignored
```

Everything still required stays `empty` until a verified fact fills it. Empty is not a
failure; it is an honest statement that the answer is not established yet.

```
score = populated / active × 100        where active = 33 − slotignored
```

That is the whole calculation. No weighting, no judgement, no model. The same file
scores the same everywhere, and anyone can check the arithmetic by hand.

Tiers label the number: 100 is ✪ Trophy, with Gold, Silver, Bronze, Green, Yellow,
Red and White below it.

## 21 or 33

**[faf-cli](https://www.npmjs.com/package/faf-cli) (MIT, free) scores 21 slots, for
every app type.** That is the whole picture for a single application, and it costs
nothing.

**The full 33** adds 12 slots that only matter once a project becomes a monorepo or a
team: how packages are organised, what orchestrates the build, how versioning and
shared configuration work. Tools built for monorepos and teams score all 33.

Both are honest, and they agree. Scoring this repository's `example.faf`:

```
33-slot   33 total · 12 slotignored · 21 active · 21 populated → 100
faf-cli   21 total ·  1 slotignored · 20 active · 20 populated → 100
```

Same file, same answer, different universe.

## scores is carried, not computed

If a `.faf` contains a `scores` block, it is a **claim recorded at the time the file
was written** — not a live result. A compiler copies it through unchanged.

A score you can rely on comes from running a scorer over the file you have. Never
read `scores` as verification.

## Validation

A valid `.faf` file:

1. Parses as YAML
2. Has `faf_version`
3. Has `project.name`
4. Uses only `populated`, `empty` or `slotignored` as slot state

Schema: <https://faf.one/schemas/faf.schema.json>

Siblings, each IANA-registered in its own right: `.fafm` (memory), `.fafa` (agents).
The compiled binary form is `.fafb` — `application/vnd.fafb`.

## Version history

**3.3.0** — "the 33". States the format as implemented: the 33 slots, `app_type`
selecting which are required, and `populated / active` scoring. Replaces the
points-based description (type recognition, content extraction, metadata quality, AI
optimization) that no implementation used, and the earlier `type` / `content` /
`confidence` model, which described extracting a single file rather than writing
project context.

**1.2.0** (October 2025) — `project.faf` became the standard filename; `.faf` kept working.

**1.1.0** (October 2025) — project-level context; human context (the six Ws).

**1.0.0** (January 2025) — first stable release.

---

*[faf.one](https://faf.one) · MIT*
