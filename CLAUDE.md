<!-- faf:start -->
<!-- faf: @faf/specification | JavaScript | documentation | .faf (Foundational AI-context Format) - Official format specification -->
<!-- faf: claim=project.faf | family=FAF -->

# CLAUDE.md — @faf/specification

## What This Is

.faf (Foundational AI-context Format) - Official format specification

## Stack

- **Language:** JavaScript

## Context

- **Who:** AI agents, developers, and tool implementers who read or write project.faf — anyone building an implementation that conforms to the format
- **What:** .faf (Foundational AI-context Format) - Official format specification
- **Why:** A portable AI-context standard needs one canonical, IANA-registered specification — the authoritative source for application/vnd.faf+yaml that every implementation conforms to
- **Where:** https://github.com/Wolfe-Jam/faf
- **When:** IANA-registered (application/vnd.faf+yaml, October 31 2025) — Specification v1.1.0
- **How:** Read SPECIFICATION.md and the JSON schemas; reference example.faf and examples/; implementations generate and consume project.faf files conforming to this spec

---

*STATUS: BI-SYNC ACTIVE — 2026-06-20T01:17:45.287Z*
<!-- faf:end -->

# 🏎️ CLAUDE.md - @faf/specification
**Official FAF Format Specification**

## PROJECT STATE: IANA-REGISTERED 🏆
**Current Position:** Official Internet standard format specification
**Tyre Compound:** ULTRASOFT C5 (Maximum Performance)
**License:** MIT (Open Source)
**Status:** IANA-registered (application/vnd.faf+yaml)

---

## 🎨 CORE CONTEXT

### Project Identity
- **Name:** @faf/specification
- **Version:** 1.1.0
- **Type:** Format specification (documentation)
- **License:** MIT (open source)
- **Quality:** IANA-REGISTERED INTERNET STANDARD

### Our Mission - The Format Itself

**THIS IS THE SPEC:**

```
@faf/specification: THE SOURCE OF TRUTH
├── SPECIFICATION.md - Official format specification
├── example.faf - Reference implementation
├── examples/ - 5 real-world examples
├── assets/ - Logos and graphics
└── GitHub repo: Wolfe-Jam/faf
```

**CRITICAL: This is the FORMAT DEFINITION, not implementation code**
- ✅ IANA-registered (October 31, 2025)
- ✅ Official media type: `application/vnd.faf+yaml`
- ✅ Same status as PDF, JSON, XML
- ✅ Internet standard format
- ✅ Open specification (MIT license)
- ✅ GitHub: https://github.com/Wolfe-Jam/faf

### Historic Achievement

**Quadruple Validation:**
1. **IANA** - Internet Assigned Numbers Authority (Oct 31, 2025)
2. **Anthropic** - Official MCP Registry PR #2759 merged (Oct 17, 2025)
3. **Google** - Chrome Web Store approved 2x (Sep 11 & Oct 29, 2025)
4. **Community** - 11,000+ downloads, production-tested

**What IANA Registration Means:**
- Same recognition as `application/pdf`, `application/json`, `application/xml`
- Official HTTP headers: `Content-Type: application/vnd.faf+yaml`
- Universal browser/client compatibility
- Future-proof Internet standard

### Technical Architecture

**This Package Contains:**

```
faf/
├── SPECIFICATION.md           # Official format spec
├── example.faf                # Basic example
├── examples/                  # Real-world examples
│   ├── package.faf           # npm package example
│   ├── project.faf           # Project context example
│   ├── tsconfig.faf          # TypeScript config example
│   └── README.md             # Examples documentation
├── assets/                    # Logos and graphics
│   ├── logos/                # FAF branding
│   └── *.png                 # Documentation images
├── README.md                  # Main documentation
├── package.json              # Spec metadata (no code)
└── LICENSE                   # MIT

NO EXECUTABLE CODE - This is specification only.
```

**Implementation Packages (Separate):**
- `faf-cli` - CLI toolchain (6.5k downloads)
- `claude-faf-mcp` - MCP server (6.7k downloads)
- `faf-mcp` - Universal MCP server
- Chrome extension - Browser integration

### 📊 Context Quality Status
- **Overall Assessment:** IANA-registered Internet standard
- **Spec Version:** 1.1.0
- **License:** MIT
- **GitHub Repo:** https://github.com/Wolfe-Jam/faf
- **Last Updated:** November 2025

---

## 🚨 NPM PUBLISH PROTOCOL - MODIFIED 🚨

**This package is SPECIFICATION ONLY - different rules apply.**

### What This Package Is

**NOT a code package:**
- ❌ No executable code
- ❌ No build step
- ❌ No tests to run
- ✅ Documentation files only
- ✅ Spec versioning metadata
- ✅ Reference examples

### Publishing Rules

**Before publishing @faf/specification:**

1. **Update specification files**
   - [ ] Update SPECIFICATION.md if format changes
   - [ ] Update example.faf if needed
   - [ ] Add new examples/ if applicable
   - [ ] Update README.md with latest stats

2. **Version bump**
   - [ ] Update package.json version
   - [ ] Follow semantic versioning:
     - MAJOR: Breaking format changes
     - MINOR: New format features
     - PATCH: Documentation fixes

3. **Verify files**
   - [ ] Check package.json "files" array
   - [ ] Ensure all examples are valid YAML
   - [ ] Test example.faf parses correctly
   - [ ] Verify assets/logos are included

4. **Update stats** (in README.md)
   - [ ] Total downloads (CLI + MCP combined)
   - [ ] Current CLI version
   - [ ] Current MCP version
   - [ ] GitHub stars

5. **Request approval**
   ```
   Ready to publish @faf/specification v1.1.1

   Changes:
   - Updated download stats (11k → 13k)
   - Added new project.faf example
   - Updated SPECIFICATION.md with clarifications

   Files included: SPECIFICATION.md, README.md, example.faf, examples/, assets/

   Ready for: GO! or GREEN LIGHT
   ```

6. **Wait for OFFICIAL GO!**
   - ✅ **"GO!"** - Approved to publish
   - ✅ **"GREEN LIGHT"** - Approved to publish
   - ❌ Anything else - DO NOT PUBLISH

7. **Publish**
   ```bash
   npm publish --access public
   ```

**Why this matters:**
- This is the SOURCE OF TRUTH for the format
- Every implementer references this spec
- IANA registration depends on spec stability
- Versioning must be precise and meaningful

**No exceptions. Specification precision is critical.**

---

## 📋 The Specification

### SPECIFICATION.md

**Current version:** 1.1.0

**Key sections:**
1. **Format Structure** - YAML-based, scored context
2. **File Types** - 153+ supported formats
3. **Scoring System** - 0-100% AI-readiness
4. **Version Evolution** - project.faf visibility revolution
5. **Media Type** - `application/vnd.faf+yaml`

**Core format:**
```yaml
version: 1.0
score: 85
type: package
confidence: 0.92
content:
  name: "my-project"
  dependencies: ["react", "typescript", "vite"]
metadata:
  source: "package.json"
  processed: "2024-01-26T12:00:00Z"
  engine: "faf-v1"
```

### The Visibility Revolution: project.faf

**Evolution from hidden to visible:**

```bash
# Before (hidden like secrets)
ls -la
.faf          # Hidden (AI context - should be visible!) ❌

# After (visible like package.json)
ls
package.json  # Visible (dependencies) ✅
project.faf   # Visible (AI context) ✅
README.md     # Visible (documentation) ✅
```

**Why visible:**
- AI context is NOT a secret
- Should sit next to package.json
- Universal discovery
- Standard file location

**Legacy support:**
- `.faf` files still work
- Use `faf migrate` to rename to `project.faf`

---

## 📦 Examples

### Included Examples

**examples/ directory contains:**

1. **package.faf** - npm package transformation
2. **project.faf** - Full project context
3. **tsconfig.faf** - TypeScript configuration
4. **component.faf** - React component
5. **README.md** - Examples documentation

**All examples:**
- ✅ Valid YAML syntax
- ✅ Follow spec v1.1.0
- ✅ Include scores
- ✅ Production-tested

### example.faf

**Basic reference implementation:**
```yaml
version: 1.0
score: 85
type: package
confidence: 0.92
content:
  name: "example-project"
  dependencies: ["react", "typescript"]
metadata:
  source: "package.json"
  processed: "2024-01-26T12:00:00Z"
```

---

## 🎯 Strategic Position

**Role in FAF Ecosystem:**
- **Source of Truth:** All implementations follow this spec
- **IANA Standard:** Official Internet media type
- **Community Hub:** GitHub repository for issues/discussions
- **Versioning Authority:** Spec version drives ecosystem

**Relationship to Implementation Packages:**

```
@faf/specification (this package)
        ↓
   Source of Truth
        ↓
    ┌───┴───┬───────┬──────────┐
    ↓       ↓       ↓          ↓
faf-cli  MCP  Chrome Ext  Future Tools
```

**All tools implement THIS specification.**

---

## 🔗 Related Packages

**FAF Ecosystem:**
- `faf-cli` - CLI implementation (6.5k downloads)
- `claude-faf-mcp` - MCP server (6.7k downloads)
- `faf-mcp` - Universal MCP server
- `@faf/core` - Core library (crown jewels)
- `@faf/engine` - Fast engine (crown jewels)

**This Package:**
- Specification ONLY
- No code dependencies
- No implementation
- Documentation and examples

---

## 🛠️ Maintenance

### Updating the Spec

**When to bump versions:**

**MAJOR (e.g., 1.x.x → 2.0.0):**
- Breaking changes to format structure
- Removal of required fields
- Incompatible YAML changes

**MINOR (e.g., 1.1.x → 1.2.0):**
- New optional fields
- New file type support
- New scoring algorithms
- Enhanced features (backwards compatible)

**PATCH (e.g., 1.1.0 → 1.1.1):**
- Documentation fixes
- Example updates
- README improvements
- Typo corrections

### Keeping Stats Current

**Update in README.md:**
- Total downloads (check npm: faf-cli + claude-faf-mcp)
- Tool versions (check package.json of each)
- GitHub stars (check repo)
- Community size (Discord, etc.)

**Sources:**
- npm: https://npmjs.com/package/faf-cli
- npm: https://npmjs.com/package/claude-faf-mcp
- GitHub: https://github.com/Wolfe-Jam/faf

---

## 📚 Documentation Files

### Main Files

**SPECIFICATION.md:**
- Official format specification
- Complete technical documentation
- IANA registration reference

**README.md:**
- User-facing documentation
- Quick start guide
- Examples and use cases
- Latest stats and milestones

**WHERE-TO-FIND-FAF.md:**
- Tools directory
- Implementation packages
- Community resources

### Community Files

**CONTRIBUTING.md:**
- How to contribute to spec
- Issue reporting
- Discussion guidelines

**CODE_OF_CONDUCT.md:**
- Community standards
- Professional conduct

**SECURITY.md:**
- Security policy
- Vulnerability reporting

**SUPPORT.md:**
- Getting help
- FAQ
- Community channels

---

## 📖 Notes for AI Assistants

### When Working in @faf/specification:

1. **This is the SPEC** - Not implementation code
2. **IANA-registered** - Internet standard status
3. **Documentation-only** - No executable code
4. **Versioning critical** - Semantic versioning strictly enforced
5. **Examples must be valid** - All .faf files must parse correctly
6. **Stats must be current** - Update before each publish

### Architecture Decisions:

- ✅ Specification in SPECIFICATION.md
- ✅ Examples in examples/ directory
- ✅ No build step (documentation only)
- ✅ No dependencies (specification only)
- ✅ MIT license (open standard)
- ❌ No executable code
- ❌ No test suite (examples are the tests)

### Publishing Guidelines:

- **Different from code packages** - No build, no tests
- **Focus on spec integrity** - Ensure documentation is accurate
- **Version carefully** - Semantic versioning strictly applied
- **Update stats** - Keep download numbers current
- **Examples must work** - All .faf files must be valid YAML

---

## 🏁 Milestones

**Historic Achievements:**

- **Oct 31, 2025** - 🏆 IANA Registration (application/vnd.faf+yaml)
- **Oct 29, 2025** - Google Chrome Web Store approval (2nd)
- **Oct 17, 2025** - Anthropic MCP Registry merger (PR #2759)
- **Oct 12, 2025** - Specification v1.1.0 published
- **Sep 11, 2025** - Google Chrome Web Store approval (1st)
- **Aug 8, 2025** - Format created, first .faf file generated

**Quadruple Validation: IANA + Anthropic + Google (2x)**

---

**STATUS: IANA-REGISTERED INTERNET STANDARD**

*Last Updated: 2025*
*License: MIT*
*Registry: application/vnd.faf+yaml*
*🏎️⚡️_internet_standard*
