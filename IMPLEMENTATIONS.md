# Implementations

**Tools, SDKs, and servers that implement the FAF specification.**

All implementations read or write `project.faf` files conforming to [SPECIFICATION.md](SPECIFICATION.md).

---

## CLI

| Package | Version | Registry | Install |
|---------|---------|----------|---------|
| [faf-cli](https://npmjs.com/package/faf-cli) | v4.5.0 | npm | `npm i -g faf-cli` |

63 commands. Generates `project.faf` from any codebase or GitHub URL.

## MCP Servers

| Package | Platform | Registry | Install |
|---------|----------|----------|---------|
| [claude-faf-mcp](https://npmjs.com/package/claude-faf-mcp) | Anthropic | npm | `npm i -g claude-faf-mcp` |
| [faf-mcp](https://npmjs.com/package/faf-mcp) | Universal | npm | `npm i -g faf-mcp` |
| [grok-faf-mcp](https://npmjs.com/package/grok-faf-mcp) | xAI | npm | `npm i -g grok-faf-mcp` |
| [gemini-faf-mcp](https://pypi.org/project/gemini-faf-mcp/) | Google | PyPI | `pip install gemini-faf-mcp` |
| [MCPaaS](https://mcpaas.live) | Infrastructure | SaaS | mcpaas.live |

## Compilers & Runtimes

| Component | Language | Output | Link |
|-----------|----------|--------|------|
| xai-faf-rust | Rust | Native binary + `.fafb` | [GitHub](https://github.com/Wolfe-Jam/xai-faf-rust) |
| xai-faf-zig | Zig | 2.7KB WASM | [GitHub](https://github.com/Wolfe-Jam/xai-faf-zig) |
| faf-wasm-sdk | Rust→WASM | 211KB browser runtime | [GitHub](https://github.com/Wolfe-Jam/faf-wasm-sdk) |
| faf-rust-sdk | Rust | crates.io SDK | [GitHub](https://github.com/Wolfe-Jam/faf-rust-sdk) |

## Browser

| Tool | Platform | Link |
|------|----------|------|
| FAF Chrome Extension | Chrome Web Store | [Install](https://chromewebstore.google.com/detail/lnecebepmpjpilldfmndnaofbfjkjlkm) |
| fafdev.tools | Web | [Try it](https://fafdev.tools) |

## AI Interop

faf-cli v4.5.0 generates all four AI instruction formats from a single `project.faf`:

| Format | Platform | Command |
|--------|----------|---------|
| `CLAUDE.md` | Anthropic Claude | `faf bi-sync` |
| `AGENTS.md` | OpenAI Codex + 20 tools | `faf agents export` |
| `.cursorrules` | Cursor IDE | `faf cursor export` |
| `GEMINI.md` | Google Gemini | `faf gemini export` |
| All at once | Everything | `faf bi-sync --all` |

---

## Add Your Implementation

Built something that reads or writes `.faf` files? [Open a PR](https://github.com/Wolfe-Jam/faf/pulls) to add it here.
