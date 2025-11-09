# .faf - The JPEG for AI

> **🏆 IANA-Registered Internet Standard** - Official media type: `application/vnd.faf+yaml`

Foundational AI-context Format
Universal, shareable AI-Context for any AI, human or team, regardless of size, location, languages, stack, setup or documentation.

[![IANA Registered](https://img.shields.io/badge/IANA-application%2Fvnd.faf%2Byaml-blue)](https://faf.one/blog/iana-registration)
[![GitHub stars](https://img.shields.io/github/stars/Wolfe-Jam/faf?style=social)](https://github.com/Wolfe-Jam/faf)
[![NPM Downloads](https://img.shields.io/npm/dt/claude-faf-mcp?label=total%20downloads&color=00CCFF)](https://www.npmjs.com/package/claude-faf-mcp)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## 🏆 Historic Milestone: IANA Registration (October 31, 2025)

**.faf is now an officially recognized Internet standard format.**

**Official Media Type:** `application/vnd.faf+yaml`
**IANA Status:** Registered alongside PDF, JSON, XML, and other Internet standards
**Registration Date:** October 31, 2025

### Quadruple Validation
- ✅ **IANA** - Internet Assigned Numbers Authority (Official Internet Standard)
- ✅ **Anthropic** - Official MCP Registry (PR #2759 merged)
- ✅ **Google** - Chrome Web Store (Approved 2x: Sep 11 & Oct 29, 2025)
- ✅ **Community** - 11,000+ downloads, production-tested

### What IANA Registration Means
- **Internet-Scale Legitimacy** - Same recognition as `application/pdf`, `application/json`, `application/xml`
- **HTTP Standard Headers** - `Content-Type: application/vnd.faf+yaml` officially recognized
- **Universal Compatibility** - Browsers, email clients, APIs handle `.faf` files properly
- **Future-Proof** - Format backed by Internet standards body

---

## The Visibility Revolution: `project.faf`

**New Standard:** `project.faf` - visible, universal, like `package.json`

<div align="center">
<img src="assets/WHITEPAPER-FIGURE-1-project-faf-file-structure.png" alt="project.faf sits between package.json and README.md" width="600" />

**`project.faf` sits right between `package.json` and `README.md`** - exactly where it belongs.
</div>

| File | Purpose | Who Reads It |
|------|---------|--------------|
| `package.json` | Dependencies, scripts, metadata | npm, Node.js, developers |
| `project.faf` | **Context, architecture, purpose** | **AI, Claude, Cursor, any AI tool** |
| `README.md` | Human documentation | Developers, users |

**The shift:**
```bash
# Before (hidden like secrets)
ls -la
.env          # Hidden (secrets - should be hidden) ✅
.faf          # Hidden (AI context - should be visible!) ❌

# After (visible like package.json)
ls
package.json  # Visible (dependencies) ✅
project.faf   # Visible (AI context) ✅
README.md     # Visible (documentation) ✅
.env          # Still hidden (secrets stay secret) ✅
```

`.env` hides secrets. `project.faf` shares context.

**Migration:** Legacy `.faf` files still work. Use `faf migrate` to rename to `project.faf`.

---

## What is .faf?

**.faf** (Foundational AI-context Format) is an IANA-registered open format for AI context. Transform individual files or entire projects into structured data that AI instantly understands. Your tech stack, dependencies, architecture, and goals - all in one concise, shareable format.

Instead of explaining your project every time, AI reads your .faf once and knows exactly how to help.

**11,000+ downloads** across [faf-cli](https://npmjs.com/package/faf-cli) and [claude-faf-mcp](https://npmjs.com/package/claude-faf-mcp) combined.

## The Problem

AI tools need context. Files have context. But there's no standard way to measure or structure that context for optimal AI understanding.

## The Solution

```yaml
# example.faf
version: 1.0
score: 85
type: package
confidence: 0.92
content:
  name: "my-project"
  dependencies: ["react", "typescript", "vite"]
  scripts: ["dev", "build", "test"]
metadata:
  source: "package.json"
  processed: "2024-01-26T12:00:00Z"
  engine: "faf-v1"
```

## Official Tools

<table>
<tr>
<td width="50%">

### 🩵 [faf-cli](https://npmjs.com/package/faf-cli)
**Command-Line Tool** - v3.0.1

- 41 championship commands
- TURBO-CAT format discovery
- Universal AI support
- 4,600+ downloads

```bash
npm install -g faf-cli
```

</td>
<td width="50%">

### 🧡 [claude-faf-mcp](https://npmjs.com/package/claude-faf-mcp)
**Claude Desktop MCP** - v2.5.5

- 33+ tools for Claude
- Championship scoring
- <11ms performance
- 800+ weekly downloads

```bash
npm install -g claude-faf-mcp
```

</td>
</tr>
</table>

**Total: 11,000+ downloads combined** 🎉

## Core Principles

1. **Universal** - 153+ file types supported
2. **Scored** - 0-100% context completeness score
3. **AI-Optimized** - Structured for maximum AI comprehension
4. **Human-Readable** - Simple, structured format
5. **Shareable** - One file contains complete context

## Quick Example

**Input:** `package.json`
```json
{
  "name": "my-app",
  "version": "1.0.0",
  "dependencies": {
    "react": "^18.0.0"
  }
}
```

**Output:** `package.faf` (85% context score)
```yaml
version: 1.0
score: 85
type: package
content:
  name: "my-app"
  version: "1.0.0"
  dependencies: ["react@^18.0.0"]
metadata:
  processed: "2024-01-26"
```

## Why .faf?

- **🎯 Measure** - Know exactly how complete your AI context is
- **📊 Optimize** - See what's missing, improve your score
- **🚀 Standardize** - One format for all file types
- **🤖 AI-Ready** - Optimized for LLM consumption

## File Scoring

| Score | Meaning | AI Readiness |
|-------|---------|--------------|
| 0-60% | Incomplete | Poor context |
| 61-80% | Basic | Acceptable |
| 81-90% | Good | Recommended |
| 91-99% | Excellent | Optimal |
| 100% | Perfect | Maximum context |

## Use Cases

- **File Conversion:** Transform any file type into .faf format
- **Project Context:** Create a single .faf file for your entire project
- **Build Integration:** Companies can create custom implementations
- **AI Optimization:** Perfect context for any AI tool

## Get Started

### 🚀 Quick Install (Recommended)

**Command Line Tool:**
```bash
npm install -g faf-cli
cd your-project
faf init
```
→ [faf-cli on npm](https://npmjs.com/package/faf-cli) - 4,600+ downloads

**Claude Desktop Integration:**
```bash
npm install -g claude-faf-mcp
# Add to Claude Desktop config
```
→ [claude-faf-mcp on npm](https://npmjs.com/package/claude-faf-mcp) - 800+ weekly downloads

### 📚 Learn the Format

1. **[Specification](SPECIFICATION.md)** - Complete format documentation
2. **[Examples](examples/)** - Real-world .faf files (5 examples)
3. **[example.faf](example.faf)** - Basic example file
4. **[WHERE-TO-FIND-FAF.md](WHERE-TO-FIND-FAF.md)** - Tools and resources

## Status

🏆 **IANA-Registered Internet Standard** - Specification v1.1.0 (October 2025)
- ✅ **IANA Registration** - `application/vnd.faf+yaml` (October 31, 2025)
- ✅ **Anthropic Official** - MCP Registry PR #2759 merged (October 17, 2025)
- ✅ **Google Chrome Verified** - Web Store approved 2x (Sep 11 & Oct 29, 2025)
- ✅ Format specification complete
- ✅ Enhanced with project-level context and AI workflows
- ✅ Production tools available (faf-cli, claude-faf-mcp)
- ✅ 11,000+ combined downloads (4,600+ CLI, 800+/week MCP)
- ✅ Battle-tested with 12,500+ test iterations (WJTTC GOLD Certified)
- ✅ Open source (MIT License)

## Major Milestones

- **Oct 31, 2025** - 🏆 **IANA Registration** (`application/vnd.faf+yaml`)
- **Oct 29, 2025** - Google Chrome Web Store approval (2nd)
- **Oct 17, 2025** - Official Anthropic MCP Registry merger (PR #2759)
- **Oct 12, 2025** - Specification v1.1.0 published
- **Sep 24, 2025** - CLI v2.1.0 published to npm
- **Sep 16, 2025** - MCP Server v2.0.0 published to npm
- **Sep 11, 2025** - Google Chrome Web Store approval (1st)
- **Sep 1, 2025** - Developer platform launch (fafdev.tools)
- **Aug 8, 2025** - Format created, first official .faf file generated

**Quadruple Validation: IANA, Anthropic, Google (2x)**

## Contributing

This is an open standard. Ideas, feedback, and contributions welcome.

## About This Repository

This repository contains the **format specification only**. For working implementations:
- Use [faf-cli](https://npmjs.com/package/faf-cli) for command-line tools
- Use [claude-faf-mcp](https://npmjs.com/package/claude-faf-mcp) for Claude Desktop

The package.json in this repo provides spec metadata and versioning but contains no executable code.

## License

MIT - Use freely, build anything.

---

*Created by [wolfejam.dev](https://wolfejam.dev)*

**STOP .fafFing About! 
Let's RACE 🏎️⚡️🏁
