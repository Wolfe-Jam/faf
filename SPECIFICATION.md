# .faf Specification v1.0

## Overview

**.faf** (File as Format) is a universal format for representing any file's context in an AI-optimized structure.

## Format

.faf files use YAML syntax for human readability and machine parsing.

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
```

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
1. Be valid YAML syntax
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

*Specification v1.0 - January 2025*