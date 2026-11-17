<!-- faf: faf | YAML | format-spec | .faf — the IANA-registered Foundational AI-context Format (application/vnd.faf+yaml). Portable, shareable AI-context for any AI, human, or team. -->

# .faf format specification

`.faf` is a YAML format for a project's facts: its name, goal, main language, app type, stack, and the six W's (who, what, why, where, when, how). The file is named `project.faf` and sits in the repository root, next to `package.json`. AI tools read it for context about the project.

This repository holds the specifications for `.faf` and its two sibling formats, with their JSON Schemas, conformance files and examples. It contains no executable code.

## Media types

| Format | Holds | Media type | Standard filename | Registered with IANA |
|---|---|---|---|---|
| `.faf` | project facts | [`application/vnd.faf+yaml`](https://www.iana.org/assignments/media-types/application/vnd.faf+yaml) | `project.faf` | 30 October 2025 |
| `.fafm` | memory | [`application/vnd.fafm+yaml`](https://www.iana.org/assignments/media-types/application/vnd.fafm+yaml) | `project.fafm` | 13 May 2026 |
| `.fafa` | an agent card | [`application/vnd.fafa+yaml`](https://www.iana.org/assignments/media-types/application/vnd.fafa+yaml) | `agent.fafa` | 26 June 2026 |

All three are UTF-8 encoded YAML 1.2.

## Example

```yaml
faf_version: "3.0"
project:
  name: orders-api
  goal: Track customer orders through a REST API
  main_language: TypeScript
  type: backend
stack:
  backend: Express
  database: PostgreSQL
human_context:
  who: Backend developers on the orders team
  why: Orders were tracked in a spreadsheet
```

A file is valid when it parses as YAML and has `faf_version` and `project.name`. Every other field is optional.

## Scoring

A `.faf` file has 33 slots. Each slot is populated, empty or `slotignored`. The app type (`project.type`) decides which slots are `slotignored`, and those leave the count.

```
score = populated / active × 100        active = 33 − slotignored
```

| Score | Tier |
|---|---|
| 100% | ✪ Trophy |
| 99% | ★ Gold |
| 95% | ◆ Silver |
| 85% | ◇ Bronze |
| 70% | ● Green |
| 55% | ● Yellow |
| 1% | ○ Red |
| 0% | ♡ White |

The score is deterministic: the same file gets the same score from every conforming scorer. [SPECIFICATION.md](SPECIFICATION.md) has the slot list and the rule.

## Documents

| Document | Contents |
|---|---|
| [SPECIFICATION.md](SPECIFICATION.md) | `.faf`: fields, slots, scoring, validity |
| [docs/EXAMPLES.md](docs/EXAMPLES.md) | one task per section: valid and invalid files, a 100% file, schema checks, `.fafm`, `.fafa` |
| [MEMORY-FORMAT.md](MEMORY-FORMAT.md) | `.fafm` |
| [AGENT-FORMAT.md](AGENT-FORMAT.md) | `.fafa` |
| [CONTEXT-INGESTION.md](CONTEXT-INGESTION.md) | how a tool takes in `.faf` context |
| [IMPLEMENTATIONS.md](IMPLEMENTATIONS.md) | tools that read or write the formats, with versions |
| [schemas/](schemas/) | JSON Schemas for `.faf`, `.fafm` and `.fafa` |
| [conformance/](conformance/) | files that must pass or fail, with the expected results |
| [examples/](examples/) | example `.faf` and `.fafm` files |

## Tools

[faf-cli](https://github.com/Wolfe-Jam/faf-cli) creates and scores `project.faf`:

```bash
npx faf-cli init
```

MCP servers for Claude, Cursor, Codex, Goose, Grok, Gemini and other MCP clients, a Rust implementation and the rest are listed in [IMPLEMENTATIONS.md](IMPLEMENTATIONS.md).

## Citing

- Wolfe, J. (2025). *Format-Driven AI Context Architecture: The .faf Standard for Persistent Project Understanding*. Zenodo. https://doi.org/10.5281/zenodo.18251362
- Wolfe, J. (2026). *Permanent Memory and Instant Recall: The .fafm Standard for Multi-Profile AI Agent Memory*. Zenodo. https://doi.org/10.5281/zenodo.20348942
- Wolfe, J. (2026). *Why Agents Need a Passport: .fafa — Portable Identity for the Agentic Era*. Zenodo. https://doi.org/10.5281/zenodo.21951641

[CITATION.cff](CITATION.cff) has the full records.

## License

MIT. Home: [faf.one](https://faf.one).
