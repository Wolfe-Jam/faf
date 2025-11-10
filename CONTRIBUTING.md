# Contributing to .faf Format Specification

Thank you for your interest in contributing to the .faf (Foundational AI-context Format) specification. This document provides guidelines for contributing to the format standard.

## About .faf

.faf is an IANA-registered format (application/vnd.faf+yaml) designed as universal AI context infrastructure. The specification defines how AI context should be structured, validated, and exchanged across tools and platforms.

## Development Philosophy

This specification follows format design principles:

- **Stability** - Changes must maintain backward compatibility
- **Simplicity** - Complexity is the enemy of adoption
- **Universality** - Works across all AI tools, stacks, and platforms
- **Interoperability** - Standards-compliant YAML structure
- **Longevity** - Built to last decades, like .iff format (1985-present)

## Before You Start

- Read the [Code of Conduct](CODE_OF_CONDUCT.md)
- Review the current specification
- Check [existing issues](https://github.com/Wolfe-Jam/faf/issues)
- Understand IANA registration implications
- For major changes, open a discussion first

## Types of Contributions

### Specification Improvements

**Format Enhancement Proposals (FEPs)**
- New optional fields
- Additional metadata structures
- Enhanced validation rules
- Compatibility extensions

**Clarifications**
- Ambiguous language fixes
- Example additions
- Implementation guidance
- Edge case documentation

**Reference Implementations**
- Parser examples
- Validator code
- Integration samples
- Tool demonstrations

### Documentation

- Usage examples
- Integration guides
- Best practices
- Migration guides
- Tutorials

## Contribution Process

### 1. Proposal Phase

For specification changes:

1. **Open a Discussion** at [github.com/Wolfe-Jam/faf/discussions](https://github.com/Wolfe-Jam/faf/discussions)

2. **Describe the problem**:
   - What limitation exists
   - Real-world use case
   - Impact on ecosystem

3. **Propose solution**:
   - Specification changes
   - Backward compatibility analysis
   - Implementation examples
   - Migration path (if breaking)

4. **Gather feedback** from community and maintainers

### 2. Specification Change Process

**Small changes** (clarifications, examples):
- Create PR directly
- Explain rationale in description

**Medium changes** (new optional fields):
- Discussion first
- Proof of concept
- Impact assessment
- PR with full documentation

**Major changes** (breaking changes, structural):
- Extended discussion period
- Multiple implementations tested
- IANA coordination if needed
- Formal review process
- Versioning strategy

### 3. Submitting Changes

```bash
# Fork and clone
git clone https://github.com/YOUR_USERNAME/faf.git
cd faf

# Create branch
git checkout -b spec/your-change-name

# Make changes to specification files
# Edit SPECIFICATION.md, examples, etc.

# Commit with clear message
git commit -m "spec: add optional context_metadata field

- Enables extensible metadata without breaking changes
- Maintains backward compatibility with v1.0
- Includes validation schema updates
- Adds usage examples"

# Push and create PR
git push origin spec/your-change-name
```

## Specification Structure

Key files in the repository:

- **SPECIFICATION.md** - Core format definition
- **SCHEMA.yaml** - Validation schema
- **EXAMPLES/** - Reference implementations
- **CHANGELOG.md** - Version history
- **MIGRATION.md** - Upgrade guides

## Writing Standards

### Specification Language

- Use RFC 2119 keywords (MUST, SHOULD, MAY)
- Be precise and unambiguous
- Provide examples for clarity
- Define all terms explicitly
- Consider edge cases

**Example**:
```markdown
The `context_version` field MUST be a string following semantic versioning.
The `project_type` field SHOULD be one of the predefined types but MAY contain custom values.
```

### Examples

All examples must:
- Be valid YAML
- Include comments explaining purpose
- Show both minimal and complete usage
- Demonstrate edge cases
- Work with existing tools

## Backward Compatibility

Breaking changes require:

1. **Strong justification** - Why is breaking change necessary?
2. **Migration path** - How do users upgrade?
3. **Deprecation period** - Timeline for transition
4. **Tooling support** - Auto-migration tools if possible
5. **Version bump** - Major version increment

## IANA Considerations

The .faf format is registered with IANA as:
- **Media Type**: application/vnd.faf+yaml
- **Registration**: Official internet standard

Changes affecting IANA registration require:
- Coordination with format steward
- IANA update procedures
- Extended review period

## Testing Validation

Specification changes should be validated by:

1. **Parser compatibility** - Test with existing parsers
2. **Tool integration** - Verify faf-cli, claude-faf-mcp work
3. **Example validation** - All examples must parse correctly
4. **Schema validation** - Update and test schemas
5. **Edge cases** - Test boundary conditions

## Reference Implementations

When proposing changes, include:

```yaml
# Example: New optional field proposal
---
faf_version: "1.1"
context_version: "1.0.0"
project_name: "example-project"

# Proposed new field
context_metadata:
  author: "developer@example.com"
  created_at: "2025-11-09T12:00:00Z"
  
# ... rest of standard fields
```

## Community Discussion

Active discussion channels:

- **GitHub Discussions** - [github.com/Wolfe-Jam/faf/discussions](https://github.com/Wolfe-Jam/faf/discussions)
- **Issues** - For specific proposals and bugs
- **Email** - team@faf.one for private inquiries

## What We're Looking For

### High Priority

- Backward-compatible enhancements
- Clarifications of ambiguous language
- Additional usage examples
- Integration guides
- Validation improvements

### Welcome Contributions

- Format documentation
- Parser implementations
- Tool integrations
- Use case examples
- Best practices guides

### Not Accepting

- Breaking changes without strong justification
- Vendor-specific extensions
- Unnecessary complexity
- Proprietary requirements
- Changes conflicting with IANA registration

## Recognition

Contributors are recognized:

- Listed in CHANGELOG.md for their contributions
- Credited in specification acknowledgments
- Mentioned in release announcements
- Added to contributors list

## Getting Help

- **Discussions**: For questions and proposals
- **Issues**: For specific specification problems
- **Email**: team@faf.one for IANA or format stewardship questions

## License

The .faf format specification is MIT licensed. Contributions will be under the same license. See [LICENSE](LICENSE) file for details.

## Related Projects

- **faf-cli** - Reference CLI implementation ([npm](https://npmjs.com/package/faf-cli))
- **claude-faf-mcp** - MCP server implementation ([npm](https://npmjs.com/package/claude-faf-mcp))
- **Chrome Extension** - Browser integration ([Chrome Web Store](https://chromewebstore.google.com/detail/lnecebepmpjpilldfmndnaofbfjkjlkm))

## Specification Stewardship

Format steward: Wolfe James ([ORCID: 0009-0007-0801-3841](https://orcid.org/0009-0007-0801-3841))

IANA registration steward: Responsible for maintaining official format registration and coordinating updates with IANA.

---

**Built to last. Built for everyone.**

*Format specification designed for universal AI context interoperability*
