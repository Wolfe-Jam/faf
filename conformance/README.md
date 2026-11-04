# FAF Conformance Corpus

A **shared conformance suite** for the FAF format family. The spec owns this
corpus (pure **data** — no code, no deps, no runner, consistent with
`@faf/specification` being specification-only). Every implementation
(`faf-cli`, `faf-rust-sdk`, `faf-wasm-sdk`, `@faf/enterprise`, third parties)
runs the *same* fixtures with its own test runner — so the standard is proven
to be read identically everywhere. This is the family's anti-drift anchor.

## Layout

```
conformance/
  expected.json          # fixture → expected outcome (the contract)
  faf/valid/             # .faf that MUST validate
  faf/invalid/           # .faf that MUST be rejected
  fafm/valid/            # .fafm that MUST conform to schemas/fafm.schema.json
  fafm/invalid/          # .fafm that MUST be rejected
```

## How to run (reference: faf-cli)

`faf-cli/tests/wjttc/spec-conformance.test.ts` loads this corpus, validates each
`.faf` through the scoring kernel and each `.fafm` against
`schemas/fafm.schema.json`, and asserts the result matches `expected.json`.

## Notes

- These are **conformance fixtures** (test data), not real project files — the
  `invalid/` cases are deliberately broken and no tool would author them.
- `fafm/valid/unknown-fields.fafm` encodes the §10 rule: unknown fields are
  permitted and MUST be ignored, not rejected.
- `fafm/valid/unquoted-dates.fafm` encodes the §10 date rule: a parser that
  turns YAML timestamps into native dates (PyYAML, js-yaml 4) must serialize
  them back to RFC 3339 strings before validating. `yaml` (eemeli) returns
  strings already.
- `fafm/txt_hash-vectors.json` holds `txt_hash` test vectors (§8.1): input,
  normalized text and expected hash, including the code points that are and
  aren't stripped. Every implementation that writes tombstones should match all
  of them.
- To add a fixture: drop the file in the right `valid/` or `invalid/` dir and
  add its expected outcome to `expected.json`.
