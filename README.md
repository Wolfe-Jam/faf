# .faf - The JPEG for AI
Foundational AI-context Format
Universal, shareable AI-Context for any AI, human or team, regardless of size, location, languages, stack, setup or documentation.

> **Transform any file into perfect AI context**

## 🏁 Join the Community!
**3,000+ dot.faffers racing together!** Join discussions, share wins, get help: [**github.com/Wolfe-Jam/faf/discussions**](https://github.com/Wolfe-Jam/faf/discussions)

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

## Get Started

1. **View the [Specification](SPECIFICATION.md)**
2. **See [example.faf](example.faf)**
3. **Try it:** [fafdev.tools](https://fafdev.tools/) - Web implementation (Beta)
4. **Find .faf:** [WHERE-TO-FIND-FAF.md](WHERE-TO-FIND-FAF.md) - Tools, implementations, and community resources

## Status

🏁 **Early Stage** - Specification v1.0 released January 2025

## Contributing

This is an open standard. Ideas, feedback, and contributions welcome.

## License

MIT - Use freely, build anything.

---

*Created by [wolfejam.dev](https://wolfejam.dev)*

**STOP .fafFing About! 
Let's RACE 🏎️⚡️🏁
