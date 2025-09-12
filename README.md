# .faf - Foundational Universal AI-Context File Format

> **Transform any file into perfect AI context**

## What is .faf?

**.faf** gives any AI complete project context in one concise, shareable format. Your tech stack, dependencies, architecture, and goals - all structured for instant AI understanding.

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

## Get Started

1. **View the [Specification](SPECIFICATION.md)**
2. **See [example.faf](example.faf)**
3. **Try it:** [fafdev.tools](https://fafdev.tools/) - Web implementation (Beta)

## Status

🏁 **Early Stage** - Specification v1.0 released January 2025

## Contributing

This is an open standard. Ideas, feedback, and contributions welcome.

## License

MIT - Use freely, build anything.

---

*Created by [wolfejam.dev](https://wolfejam.dev)*

**The dial IS .faf** Let's RACE 🏎️⚡️🏁
