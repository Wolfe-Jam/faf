# .faf Format Specification

**Version:** 1.2.0
**Status:** Stable
**Released:** October 2025
**License:** MIT

## Overview

**.faf** (Foundational AI-context Format) is a universal format for representing project context in an AI-optimized structure.

**Standard Filename:** `project.faf` (visible standard, like package.json)
**Legacy Filename:** `.faf` (hidden, still supported for backward compatibility)

## Filename Standard

As of v1.2.0, the standard filename is **`project.faf`** (visible, next to package.json).

**Why the change?**
- Improved discoverability (visible in standard tools)
- Easier evangelism ("show, don't tell")
- Universal like package.json, README.md
- Path-based identity (no UUID needed)

**Legacy support:**
- `.faf` (hidden filename) still works
- Backward compatible with all tools
- Optional migration via `faf migrate` command

**File lookup priority:**
1. `project.faf` (standard, preferred)
2. `.faf` (legacy, shows deprecation warning)

## Format

.faf files use a structured key-value format for human readability and machine parsing.

## Required Fields

```yaml
version: 1.0           # Specification version
score: 85              # Context completeness (0-100)
type: "package"        # File type category
content: {}            # Extracted content (type-specific)
```

## Optional Fields

```yaml
confidence: 0.92       # Extraction confidence (0-1)
metadata:              # Processing metadata
  source: "file.ext"   # Original filename
  processed: "ISO8601" # Processing timestamp
  engine: "engine-id"  # Processing engine identifier
  size: 1024          # Original file size (bytes)
tags: []              # Descriptive tags
skills: []            # Claude Code skills (v1.1.0+)
```

### skills (v1.1.0+)

List of Claude Code skill names to use with this project. Skills are loaded from `~/.claude/skills/` and provide specialized expertise for specific tasks.

```yaml
skills:
  - faf                # .faf format expertise
  - wjttc-tester       # Championship testing
  - nextjs-builder     # Next.js development patterns
```

**When to use:**
- Document which skills are relevant to the project
- Help team members know which skills to enable
- Track recommended skills for specific tech stacks

**Notes:**
- Skills must exist in `~/.claude/skills/` directory
- Each skill name matches its directory name
- Skills are loaded dynamically by Claude Code
- See: https://github.com/anthropics/skills for skill format

## Type Categories

Common types and their expected content structure:

### `package` - Package managers
```yaml
type: "package"
content:
  name: "project-name"
  version: "1.0.0"
  dependencies: ["dep1", "dep2"]
  devDependencies: ["dev1", "dev2"]
  scripts: ["build", "test", "dev"]
```

### `config` - Configuration files
```yaml
type: "config"
content:
  framework: "vite"
  settings: 
    key: value
```

### `documentation` - Docs/README
```yaml
type: "documentation"
content:
  title: "Project Title"
  sections: ["intro", "usage", "api"]
  keywords: ["keyword1", "keyword2"]
```

### `code` - Source code files
```yaml
type: "code"
content:
  language: "typescript"
  imports: ["react", "vite"]
  exports: ["Component", "function"]
  classes: ["MyClass"]
  functions: ["myFunction"]
```

### `data` - Data files
```yaml
type: "data"
content:
  format: "json"
  schema: {}
  records: 100
```

## Scoring Algorithm

Context score (0-100) based on:

1. **Type recognition** (20 points)
   - File type identified correctly

2. **Content extraction** (50 points)
   - Key information extracted
   - Completeness of extraction

3. **Metadata quality** (20 points)
   - Dependencies identified
   - Relationships mapped
   - Configuration detected

4. **AI optimization** (10 points)
   - Structure clarity
   - Relevance to common queries

## Example Files

### High Score (95%)
```yaml
version: 1.0
score: 95
type: "package"
confidence: 0.98
content:
  name: "enterprise-app"
  version: "2.0.0"
  description: "Production application"
  dependencies: ["react", "typescript", "vite"]
  devDependencies: ["vitest", "eslint"]
  scripts: ["dev", "build", "test", "lint"]
  engines:
    node: ">=20.0.0"
metadata:
  source: "package.json"
  processed: "2024-01-26T12:00:00Z"
  size: 2048
tags: ["frontend", "typescript", "production"]
```

### Low Score (45%)
```yaml
version: 1.0
score: 45
type: "unknown"
confidence: 0.4
content:
  raw: "partial content extracted"
metadata:
  source: "mystery.dat"
  processed: "2024-01-26T12:00:00Z"
```

## Validation

A valid .faf file must:
1. Be valid structured syntax
2. Include all required fields
3. Have score between 0-100
4. Include version number

## Future Versions

Future versions may add:
- Binary encoding options
- Compression support
- Signature/verification
- Extended metadata

---

## Version History

**v1.2.0** (October 2025)
- Standard filename: `project.faf` (visible, like package.json)
- Legacy filename: `.faf` (hidden, still supported)
- Improved discoverability and evangelism
- Backward compatible with v1.1.0 and v1.0.0
- Path-based identity (no UUID needed)
- Shareable, fork-friendly format

**v1.1.0** (October 2025)
- Project-level context support (not just file-level)
- Architecture and pattern fields
- Human context (6 W's: who/what/why/where/when/how)
- Multiple scoring types (faf_score, ai_compatibility_score, completeness_score)
- Agent context for AI workflows
- Source data preservation
- Universal Intelligence Pattern support
- Enhanced metadata fields

**v1.0.0** (January 2025)
- Initial stable release
- Universal format specification
- Core type categories defined
- Basic scoring algorithm documented

---

*Maintained by [wolfejam.dev](https://wolfejam.dev) | [faf.one](https://faf.one)*