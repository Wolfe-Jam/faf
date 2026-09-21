# .faf Examples

Five `.faf` files, each a different `app_type`, arranged as a ladder — from the
smallest honest file to the root of a monorepo.

Every score below was produced by running a scorer over the file in this
directory. None is written by hand.

## The ladder

| Example | `app_type` | Active slots | Score |
|---------|-----------|-------------:|------:|
| [`documentation.faf`](documentation.faf) | `documentation` | 9 | 100 ✪ |
| [`cli-project.faf`](cli-project.faf) | `cli` | 12 | 100 ✪ |
| [`chrome-extension.faf`](chrome-extension.faf) | `extension` | 15 | 100 ✪ |
| [`n8n-workflow.faf`](n8n-workflow.faf) | `backend` | 16 | 100 ✪ |
| [`monorepo-root.faf`](monorepo-root.faf) | `monorepo-root` | 19 | 100 ✪ |

All five score **100**. That is the point of the ladder: a documentation repo
with nine slots filled is as complete as a monorepo root with nineteen. It is
not scored against questions it was never asked.

## What each one shows

**`documentation.faf`** — the minimum. Project and human context only. A docs
repo has no stack, so twenty-four slots are marked `slotignored` and never
enter the score.

**`cli-project.faf`** — adds delivery: hosting, build, CI. A CLI ships and
builds somewhere, so those count. No frontend, no database.

**`chrome-extension.faf`** — adds the frontend slots. Note `ui_library:
slotignored` among otherwise populated frontend slots: this extension styles
its popup directly and uses no component library. Saying so is different from
leaving it empty.

**`n8n-workflow.faf`** — adds the backend slots. It also shows what a `.faf`
describes: the support bot, not the workflow JSON that defines it. The subject
is always the project.

**`monorepo-root.faf`** — the twelve slots the others never reach: how packages
are organised, what orchestrates the build, how versioning and shared config
work. Worth comparing across tools:

```
faf-cli (21 slots)   100 — 9 active     the enterprise slots are not in scope
33-slot engine       100 — 19 active    they are
```

Same file, same answer, different universe.

## The mechanism

`app_type` says which slots a project is asked about. Everything it is not asked
about is written into the file as `slotignored`, which removes it from the
denominator:

```
score = populated / active × 100        active = 33 − slotignored
```

A slot left out entirely is **not** the same as one marked `slotignored` — an
absent slot reads as `empty` and counts against the score. Tooling writes the
labels for you; `faf auto` marks what the type implies.

## Memory files

Two `.fafm` examples sit alongside these — [`voice-memory.fafm`](voice-memory.fafm)
and [`knowledge-memory.fafm`](knowledge-memory.fafm). Memory is a separate
format with its own specification ([MEMORY-FORMAT.md](../MEMORY-FORMAT.md)) and
is not slot-scored.

## Write your own

```bash
npm install -g faf-cli
cd your-project
faf init      # writes project.faf
faf auto      # fills what the tree can prove, marks what the type does not need
```

## More

- [SPECIFICATION.md](../SPECIFICATION.md) — the format, the 33 slots, the scoring rule
- [faf.one/spec](https://faf.one/spec) — the same, in prose
- [faf-cli](https://npmjs.com/package/faf-cli) — the MIT toolchain
