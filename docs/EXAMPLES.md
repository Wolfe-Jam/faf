# .faf by example

One task per section: a sentence, then a file or a command. Every file below is in this repository, and every output was captured with faf-cli 8.10.0 or the JSON Schemas in [`schemas/`](../schemas). The rules themselves are in [SPECIFICATION.md](../SPECIFICATION.md).

- [The smallest valid project.faf](#the-smallest-valid-projectfaf)
- [A complete project.faf that scores 100%](#a-complete-projectfaf-that-scores-100)
- [Mark a slot that doesn't apply: slotignored](#mark-a-slot-that-doesnt-apply-slotignored)
- [Work out a score by hand](#work-out-a-score-by-hand)
- [What makes a .faf invalid](#what-makes-a-faf-invalid)
- [Validate against the JSON Schema](#validate-against-the-json-schema)
- [A .fafm memory file](#a-fafm-memory-file)
- [A .fafa agent card](#a-fafa-agent-card)

---

## The smallest valid project.faf

A valid `.faf` needs `faf_version` and `project.name`. Everything else is optional. This file is [`conformance/faf/valid/minimal.faf`](../conformance/faf/valid/minimal.faf).

```yaml
faf_version: 2.5.0
project:
  name: minimal-demo
  goal: Minimal valid faf fixture for conformance
  main_language: TypeScript
```

Valid, and scored on what it holds: 3 of 33 slots.

```bash
$ faf check conformance/faf/valid/minimal.faf
valid conformance/faf/valid/minimal.faf
○ RED 9% 3/33 slots — conformance/faf/valid/minimal.faf
  30 slots from Trophy: human_context.who, human_context.what, human_context.why, +27 more
```

## A complete project.faf that scores 100%

A CLI tool: the project block, the six W's, the three delivery slots it needs, and every other slot marked `slotignored`. This file is [`examples/cli-project.faf`](../examples/cli-project.faf) (comments trimmed).

```yaml
faf_version: 2.5.0

app_type: cli

project:
  name: faf-cli
  goal: Read, write and score project.faf from the command line
  main_language: TypeScript

human_context:
  who: Developers who want their project context portable across AI tools
  what: A CLI that detects a project's stack, writes project.faf, and scores it
  why: Every AI session re-discovers the same project facts; writing them down once ends that
  where: Installed from npm, run in any repository
  when: Active development; released continuously
  how: Detects the stack from manifests, fills what the tree can prove, leaves the rest for a human

stack:
  hosting: npm registry
  build: tsup
  cicd: GitHub Actions
  frontend: slotignored
  css_framework: slotignored
  ui_library: slotignored
  state_management: slotignored
  backend: slotignored
  api_type: slotignored
  runtime: slotignored
  database: slotignored
  connection: slotignored
  monorepo_tool: slotignored
  package_manager: slotignored
  workspaces: slotignored
  admin: slotignored
  cache: slotignored
  search: slotignored
  storage: slotignored

monorepo:
  packages_count: slotignored
  build_orchestrator: slotignored
  versioning_strategy: slotignored
  shared_configs: slotignored
  remote_cache: slotignored
```

```bash
$ faf score examples/cli-project.faf
✪ TROPHY 100% 12/12 slots — examples/cli-project.faf
  Trophy. AI never has to guess.
```

More complete examples, all scoring 100%: [`examples/`](../examples) (Chrome extension, documentation, monorepo root, n8n workflow).

## Mark a slot that doesn't apply: slotignored

A slot the app type doesn't need is written as `slotignored`. It leaves the calculation. A slot that is simply left out counts as **empty**, so write the label.

```yaml
stack:
  database: slotignored    # a CLI has no database: not counted
  hosting: npm registry    # populated: counts and scores
  cicd: ""                 # empty: counts, scores 0
```

A slot holds one fact: text or a number. A list, a mapping, `true`/`false`, or a placeholder such as `TBD` or `None` counts as empty.

## Work out a score by hand

```
score = populated / active × 100        active = 33 − slotignored
```

For `examples/cli-project.faf`: 21 slots are `slotignored`, so 12 are active. All 12 are populated.

```
active    = 33 − 21 = 12
populated = 3 (project) + 6 (human_context) + 3 (hosting, build, cicd) = 12
score     = 12 / 12 × 100 = 100   → ✪ Trophy
```

Tiers: ✪ Trophy 100 · ★ Gold 99 · ◆ Silver 95 · ◇ Bronze 85 · ● Green 70 · ● Yellow 55 · ○ Red 1 · ♡ White 0.

## What makes a .faf invalid

A `.faf` is invalid if it isn't YAML, or lacks `faf_version` or `project.name`. This file is [`conformance/faf/invalid/malformed.faf`](../conformance/faf/invalid/malformed.faf): an unquoted colon breaks the YAML.

```yaml
faf_version: 2.5.0
project:
  name: broken-demo
  goal: this value has an unquoted: colon that breaks YAML parsing
```

```bash
$ faf check conformance/faf/invalid/malformed.faf
faf: conformance/faf/invalid/malformed.faf is not valid YAML (Nested mappings are not allowed in compact mappings, line 4) — faf left it unchanged
```

Quote the value to fix it: `goal: "this value has an unquoted: colon …"`.

## Validate against the JSON Schema

Each format has a JSON Schema (draft 2020-12): [`faf.schema.json`](https://faf.one/schemas/faf.schema.json), [`fafm.schema.json`](https://faf.one/schemas/fafm.schema.json), [`fafa.schema.json`](https://faf.one/schemas/fafa.schema.json). Any JSON Schema validator works once the YAML is parsed. In Python:

```python
import json, yaml, jsonschema

schema = json.load(open("schemas/faf.schema.json"))
doc = yaml.safe_load(open("examples/cli-project.faf"))
jsonschema.validate(doc, schema)   # raises ValidationError if invalid
```

Results on this repository's files:

```
conformance/faf/valid/minimal.faf          valid against schemas/faf.schema.json
examples/cli-project.faf                   valid against schemas/faf.schema.json
conformance/fafm/valid/knowledge.fafm      valid against schemas/fafm.schema.json
conformance/fafm/invalid/missing-required.fafm
  jsonschema.exceptions.ValidationError: 'memory' is a required property
```

The shared conformance suite, and the outcome each file must produce, is in [`conformance/`](../conformance).

## A .fafm memory file

`.fafm` holds what an agent remembers: facts with an id, a type and a priority. The full format is in [MEMORY-FORMAT.md](../MEMORY-FORMAT.md). This file is [`conformance/fafm/valid/knowledge.fafm`](../conformance/fafm/valid/knowledge.fafm).

```yaml
version: "1.1"
profile: knowledge
namepoint: "@claude-code:demo"
created: "2026-05-21T00:00:00Z"
last_etched: "2026-05-21T00:00:00Z"
index:
  - "stack: TypeScript + Bun"
memory:
  facts:
    - id: f1
      text: "The project uses Bun"
      type: project
      priority: high
      confidence_score: 0.9
      verification_status: verified
```

```bash
$ faf memory ls -f conformance/fafm/valid/knowledge.fafm
memory ls — @claude-code:demo (1 facts)

  f1  The project uses Bun
         project · high
```

`memory` is required. Without it the file is rejected (see the validation results above).

## A .fafa agent card

`.fafa` declares an agent: who it is, what it can do, how to reach it. The full format is in [AGENT-FORMAT.md](../AGENT-FORMAT.md). The minimal valid card, from AGENT-FORMAT.md §10:

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

Required: `version`; `agent` with `name` and `id`; `capabilities` (may be empty); `endpoints` (at least one). It validates against [`fafa.schema.json`](https://faf.one/schemas/fafa.schema.json).

---

Media types: `.faf` `application/vnd.faf+yaml` · `.fafm` `application/vnd.fafm+yaml` · `.fafa` `application/vnd.fafa+yaml`, all IANA-registered.
