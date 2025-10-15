# .faf - Project DNA ✨ for any AI
Foundational AI-context Format
Universal, shareable AI-Context for any AI, human or team, regardless of size, location, languages, stack, setup or documentation.

> **Transform any file into perfect AI context**

## ⭐ GitHub Stars are appreciated - thank you

[![GitHub stars](https://img.shields.io/github/stars/Wolfe-Jam/faf?style=social)](https://github.com/Wolfe-Jam/faf)

** 8000+ Downloads of FREE tools, and counting ⚡️

**[⭐ Star Now](https://github.com/Wolfe-Jam/faf) | [💬 Join Community](https://github.com/Wolfe-Jam/faf/discussions) | Enjoy!**

## What is .faf?

**.faf** is an open format for AI context. Transform individual files or entire projects into structured data that AI instantly understands. Your tech stack, dependencies, architecture, and goals - all in one concise, shareable format.

Instead of explaining your project every time, AI reads your .faf once and knows exactly how to help.

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

## Core Principles

1. **Universal** - 150+ file types supported
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

  ## 🚀 Get Started Now

  ### For Developers - Install the CLI

  ```bash
  # Install via npm (works everywhere)
  npm install -g faf-cli

  # Or via Homebrew (macOS/Linux)
  brew install faf-cli

  # Then initialize your project
  cd your-project
  faf init

  One command. Zero config. Championship context. 🏁

  For Claude Desktop Users - Install MCP Server

  npm install -g claude-faf-mcp

  Then add to your Claude Desktop config. https://github.com/Wolfe-Jam/claude-faf-mcp

  Try Online - No Install Required

  🌐 https://fafdev.tools/ - Web implementation (Beta)

  Drop your files, get instant .faf context!

  ---
  📚 Learn More

  - ./SPECIFICATION.md - Complete .faf format specification
  - ./example.faf - See a real .faf file
  - ./WHERE-TO-FIND-FAF.md - All FAF ecosystem tools
  - https://github.com/Wolfe-Jam/faf-cli - 41 championship commands
  - https://github.com/Wolfe-Jam/claude-faf-mcp - Claude Desktop integration

  ---
  🎯 Quick Start Paths

  Path 1: Fast Setup (5 minutes)
  npm install -g faf-cli && cd your-project && faf init

  Path 2: Try Before Installing
  Visit https://fafdev.tools/ and drop a file to see .faf in action

  Path 3: Claude Desktop Integration
  Install https://github.com/Wolfe-Jam/claude-faf-mcp for seamless AI context

  ---
  Need help? Check our https://github.com/Wolfe-Jam/faf-cli/discussions or join the community!
  
## Status

🏁 **Early Stage** - Specification v1.0 released January 2025
🏁 **Stable & Growing** - Specification v1.1.0

  **Latest Release:** v1.1.0 (October 2025)
  - Professional-grade documentation
  - 5 real-world examples (n8n, OpenAI, CLI, TypeScript, Chrome Extension)
  - Enhanced tooling integration
  - Semantic versioning established

  **Format Stability:**
  - ✅ Core format is stable and production-ready
  - ✅ 10,000+ .faf files generated across ecosystem
  - ✅ Universal AI support (Claude Code, OpenAI Codex CLI, Gemini CLI, Cursor, Warp)
  - ✅ Backwards compatible (v1.0 files remain valid)

  **Ecosystem Tools:**
  - [faf-cli](https://github.com/Wolfe-Jam/faf-cli) - 41 championship commands, 153+ format support
  - [claude-faf-mcp](https://github.com/Wolfe-Jam/claude-faf-mcp) - Claude Desktop integration
  - [fafdev.tools](https://fafdev.tools) - Web-based .faf generator (Beta)

  **Specification History:**
  - **v1.1.0** (October 2025) - Professional upgrade, real-world examples
  - **v1.0** (January 2025) - Initial public release

## Contributing

This is an open standard. Ideas, feedback, and contributions welcome.

## License

MIT - Use freely, build anything.

---

*Created by [wolfejam.dev](https://wolfejam.dev)*

**STOP .fafFing About! 
Let's RACE 🏎️⚡️🏁
