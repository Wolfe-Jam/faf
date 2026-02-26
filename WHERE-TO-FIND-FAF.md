# WHERE TO FIND .FAF

**Tools, implementations, and resources for the FAF ecosystem**

*Last Updated: February 2026*

---

## Official Resources

### Specifications & Documentation
- **Format Specification:** [github.com/Wolfe-Jam/faf](https://github.com/Wolfe-Jam/faf)
- **Planet dotFAF:** [faf.one](https://faf.one) - The home of .faf
- **Developer Tools:** [fafdev.tools](https://fafdev.tools) - Try .faf in your browser

### Try It NOW

```bash
# Zero install, zero clone — 2 seconds for ANY GitHub repo
npx faf-cli git https://github.com/facebook/react
```

- **Claude Code:** `faf init` → `faf go` → 100% AI-Readiness
- **Claude Desktop:** Full local repo support with MCP server
- **Any terminal:** `npx faf-cli init` works everywhere

---

## Official Tools

### CLI

| Package | Version | Downloads | Install |
|---------|---------|-----------|---------|
| [faf-cli](https://npmjs.com/package/faf-cli) | v4.5.0 | 14,000+ | `npm i -g faf-cli` |

**63 commands** including `faf git`, `faf go`, `faf auto`, `faf bi-sync --all`, `faf agents`, `faf cursor`, `faf gemini`

### MCP Servers

| Package | Platform | Registry | Downloads |
|---------|----------|----------|-----------|
| [claude-faf-mcp](https://npmjs.com/package/claude-faf-mcp) | Anthropic | npm | 9,800+ |
| [faf-mcp](https://npmjs.com/package/faf-mcp) | Universal | npm | 2,800+ |
| [grok-faf-mcp](https://npmjs.com/package/grok-faf-mcp) | xAI | npm | 850+ |
| [gemini-faf-mcp](https://pypi.org/project/gemini-faf-mcp/) | Google | PyPI | v1.0.2 |
| [MCPaaS](https://mcpaas.live) | Infrastructure | mcpaas.live | 153 souls |

**Total: 27,000+ downloads across the ecosystem**

### Browser & Extensions
- **Chrome Extension:** [Chrome Web Store](https://chromewebstore.google.com/detail/lnecebepmpjpilldfmndnaofbfjkjlkm) - Google-approved (2x)

---

## Compiler Toolchain

FAF compiles to native binaries and WebAssembly:

| Component | Language | Output | Link |
|-----------|----------|--------|------|
| xai-faf-rust | Rust | Native + .fafb binary | [GitHub](https://github.com/Wolfe-Jam/xai-faf-rust) |
| xai-faf-zig | Zig→WASM | 2.7KB ghost binary | [GitHub](https://github.com/Wolfe-Jam/xai-faf-zig) |
| faf-wasm-sdk | Rust→WASM | 211KB browser runtime | [GitHub](https://github.com/Wolfe-Jam/faf-wasm-sdk) |
| faf-rust-sdk | Rust | crates.io SDK | [GitHub](https://github.com/Wolfe-Jam/faf-rust-sdk) |

### FAFb Binary Format (v1.0)
- **Header:** 32 bytes (magic "FAFB", version, CRC32 checksum)
- **Sections:** 11 types with priority truncation
- **Output:** `.fafb` files for edge/embedded deployment

---

## AI Interop (v4.5.0)

One `project.faf` generates all AI instruction formats:

| Platform | Format | Command |
|----------|--------|---------|
| OpenAI Codex | `AGENTS.md` | `faf agents export` |
| Cursor IDE | `.cursorrules` | `faf cursor export` |
| Claude Code | `CLAUDE.md` | `faf bi-sync` |
| Gemini CLI | `GEMINI.md` | `faf gemini export` |
| All at once | Everything | `faf bi-sync --all` |

---

## NPM Packages

| Package | Purpose | Downloads |
|---------|---------|-----------|
| [faf-cli](https://npmjs.com/package/faf-cli) | CLI tool (v4.5.0) | 14,000+ |
| [claude-faf-mcp](https://npmjs.com/package/claude-faf-mcp) | Anthropic MCP server | 9,800+ |
| [faf-mcp](https://npmjs.com/package/faf-mcp) | Universal MCP server | 2,800+ |
| [grok-faf-mcp](https://npmjs.com/package/grok-faf-mcp) | xAI MCP server | 850+ |
| [faf-wasm](https://npmjs.com/package/faf-wasm) | WASM SDK | — |

### PyPI Packages
| Package | Purpose | Version |
|---------|---------|---------|
| [gemini-faf-mcp](https://pypi.org/project/gemini-faf-mcp/) | Google MCP server | v1.0.2 |

---

## GitHub Search
- [Search GitHub for .faf files](https://github.com/search?q=extension%3Afaf&type=code)
- [Search GitHub for faf topics](https://github.com/topics/faf)
- [NPM packages with 'faf'](https://www.npmjs.com/search?q=faf)
- [PyPI packages with 'faf'](https://pypi.org/search/?q=faf)

---

## Statistics

- **Total Downloads:** 27,000+
- **MCP Servers:** 5 (Anthropic, Universal, xAI, Google, MCPaaS)
- **Compilers:** 3 (Rust, Zig, WASM)
- **AI Formats:** 4 (AGENTS.md, .cursorrules, CLAUDE.md, GEMINI.md)
- **File Types Supported:** 153+
- **IANA Media Type:** `application/vnd.faf+yaml`

---

## Contributing

Want to add your .faf resource? Submit a PR:

1. Fork [this repository](https://github.com/Wolfe-Jam/faf)
2. Add your link in the appropriate section
3. Submit a PR with a brief description

---

## Contact

- **GitHub Issues:** [github.com/Wolfe-Jam/faf/issues](https://github.com/Wolfe-Jam/faf/issues)
- **Discussions:** [github.com/Wolfe-Jam/faf/discussions](https://github.com/Wolfe-Jam/faf/discussions)
- **Website:** [faf.one](https://faf.one)

---

**.faf** (Foundational AI-context Format) — Project DNA for any AI. Learn more at [github.com/Wolfe-Jam/faf](https://github.com/Wolfe-Jam/faf)

*Created by [wolfejam.dev](https://wolfejam.dev)*
