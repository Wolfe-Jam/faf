# Implementations

**Tools, SDKs, and servers that implement the FAF specification.**

YAML `.faf` files follow [SPECIFICATION.md](SPECIFICATION.md) and the live human spec at [faf.one/spec](https://faf.one/spec).  
FAFb **wire v2** is specified in [faf-rust `BINARY-FORMAT.md`](https://github.com/Wolfe-Jam/faf-rust/blob/main/crates/faf-fafb/BINARY-FORMAT.md). The `BINARY-FORMAT.md` in this repo is a retired v1 pointer.

---

## CLI

| Package | Version | Registry | Install |
|---------|---------|----------|---------|
| [faf-cli](https://npmjs.com/package/faf-cli) | v8.10.1 | npm · Homebrew | `npm i -g faf-cli` · `brew install wolfe-jam/faf/faf-cli` |

`npx faf-cli auto` writes or refreshes `project.faf`. `faf compile` emits a `.fafb` via the WASM kernel. Live totals: [faf.one/downloads](https://faf.one/downloads).

## MCP Servers

| Package | Platform | Registry | Install |
|---------|----------|----------|---------|
| [claude-faf-mcp](https://npmjs.com/package/claude-faf-mcp) | Anthropic | npm | `npm i -g claude-faf-mcp` |
| [faf-mcp](https://npmjs.com/package/faf-mcp) | Universal | npm | `npm i -g faf-mcp` |
| [grok-faf-mcp](https://npmjs.com/package/grok-faf-mcp) | xAI | npm | `npm i -g grok-faf-mcp` |
| [gemini-faf-mcp](https://pypi.org/project/gemini-faf-mcp/) | Google | PyPI | `pip install gemini-faf-mcp` |
| [rust-faf-mcp](https://crates.io/crates/rust-faf-mcp) | Rust, any MCP client | crates.io · npm | `cargo install rust-faf-mcp` |
| [MCPaaS](https://mcpaas.live) | Infrastructure | SaaS | mcpaas.live |

## Memory and agent cards

| Package | Version | Formats | Registry | Install |
|---------|---------|---------|----------|---------|
| [faf-cli](https://npmjs.com/package/faf-cli) | v8.10.1 | `.fafm` (`faf memory`), `.fafa` (`faf cards` reads `agent.fafa`) | npm | `npm i -g faf-cli` |
| [claude-fafm-sdk](https://pypi.org/project/claude-fafm-sdk/) | 2.2.0 | `.fafm` | PyPI | `pip install claude-fafm-sdk` |
| [grok-faf-voice](https://pypi.org/project/grok-faf-voice/) | 0.6.0 | `.fafm` | PyPI | `pip install grok-faf-voice` |

## Compilers & Runtimes

The brick is FAFb **v2**. These emit or read that layout:

| Component | Language | Output | Link |
|-----------|----------|--------|------|
| faf-fafb | Rust | FAFb v2 brick | [crates.io](https://crates.io/crates/faf-fafb) · [faf-rust](https://github.com/Wolfe-Jam/faf-rust) |
| faf-cli | TypeScript + WASM | `faf compile` via faf-scoring-kernel | [npm](https://www.npmjs.com/package/faf-cli) |
| faf-wasm-sdk | Rust→WASM | same v2 engine | [crates.io](https://crates.io/crates/faf-wasm-sdk) |
| faf-rust-sdk | Rust | facade over kernel + faf-fafb | [crates.io](https://crates.io/crates/faf-rust-sdk) |

v1 ROMs (numeric section types, `version_major = 1`) are not this format. A v2 reader rejects them. Recompile from `.faf`.

## Browser

| Tool | Platform | Link |
|------|----------|------|
| FAF Chrome Extension | Chrome Web Store | [Install](https://chromewebstore.google.com/detail/lnecebepmpjpilldfmndnaofbfjkjlkm) |
| faf.one | Web | [faf.one](https://faf.one) |

## AI Interop

faf-cli authors the instruction files from one `project.faf`:

| Format | Platform | Command |
|--------|----------|---------|
| `AGENTS.md` | Codex, Cursor, Goose, OpenCode, Warp and others | `faf export --agents` |
| `GEMINI.md` | Google Gemini | `faf export --gemini` |
| `.cursorrules` | Cursor | `faf export --cursor` |
| `CLAUDE.md` | Anthropic Claude | `faf sync` |
| `.github/copilot-instructions.md` | GitHub Copilot | `faf export --copilot` |
| All of the above | — | `faf export --all` |

## Try it

- In the browser: [devtools.faf.one](https://devtools.faf.one)
- Any public GitHub repo, no install: `npx faf-cli git https://github.com/facebook/react` (writes `./project.faf` with the tech slots filled)
- Your own project: `npx faf-cli init`, then `npx faf-cli go` for the goal and six W's

## Find more

- [`.faf` files on GitHub](https://github.com/search?q=extension%3Afaf&type=code)
- [npm packages named faf](https://www.npmjs.com/search?q=faf) · [PyPI](https://pypi.org/search/?q=faf)

---

## Add Your Implementation

Built something that reads or writes `.faf` files? [Open a PR](https://github.com/Wolfe-Jam/faf/pulls) to add it here.
