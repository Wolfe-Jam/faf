# Getting Support

Thank you for your interest in the .faf (Foundational AI-context Format) specification. This document explains how to get help with the format.

## Documentation

Start with our specification resources:

- **SPECIFICATION.md** - Complete format definition
- **SCHEMA.yaml** - Validation schema
- **EXAMPLES/** - Reference implementations
- **faf.one** - Official website and guides
- **CHANGELOG.md** - Version history

## Common Questions

### Understanding .faf

**Q: What is .faf?**

A: .faf is an IANA-registered format (application/vnd.faf+yaml) for universal AI context. It provides structured project information that works across all AI tools.

**Q: Why does .faf exist?**

A: AI tools need project context. Without a standard format, every tool requires custom setup. .faf provides universal, shareable context that works everywhere.

**Q: Is .faf free to use?**

A: Yes. The format specification is MIT licensed and free for everyone - individuals, companies, and any use case.

### Format Specification

**Q: What version should I use?**

A: Use the latest stable version (1.x). The specification maintains backward compatibility.

**Q: Can I extend .faf with custom fields?**

A: The specification allows additional fields, but they should not conflict with defined fields. Consider proposing commonly needed fields to the specification.

**Q: Is .faf valid YAML?**

A: Yes. All .faf files are valid YAML 1.2. You can use any YAML parser.

**Q: How do I validate a .faf file?**

A: Use the schema file (SCHEMA.yaml) with a YAML validator, or use implementation tools like faf-cli or claude-faf-mcp which include validation.

### Implementation

**Q: What tools support .faf?**

A: Reference implementations include:
- faf-cli (command line)
- claude-faf-mcp (Claude Desktop)
- Chrome Extension (browser)
- See faf.one for full ecosystem

**Q: How do I implement a .faf parser?**

A: Use any YAML parser with the specification schema. See EXAMPLES/ for reference implementations.

**Q: Can I build commercial tools with .faf?**

A: Yes. The format specification is MIT licensed. Commercial, open source, or any use is permitted.

## Getting Help

### GitHub Discussions

For questions and community support:

[github.com/Wolfe-Jam/faf/discussions](https://github.com/Wolfe-Jam/faf/discussions)

**Discussion categories**:
- **Q&A** - Ask questions about the format
- **Specification** - Discuss format improvements
- **Implementations** - Share tools and integrations
- **Use Cases** - Show how you're using .faf
- **General** - Everything else

### GitHub Issues

For specification issues:

[github.com/Wolfe-Jam/faf/issues](https://github.com/Wolfe-Jam/faf/issues)

**Use issues for**:
- Specification ambiguities or errors
- Schema validation problems
- Documentation gaps
- Feature proposals (after discussion)

**Provide**:
- Specific specification section reference
- Expected vs actual behavior
- Example demonstrating the issue
- Proposed fix (if applicable)

### Tool-Specific Support

For implementation tools:

- **faf-cli**: [github.com/Wolfe-Jam/faf-cli/issues](https://github.com/Wolfe-Jam/faf-cli/issues)
- **claude-faf-mcp**: [github.com/Wolfe-Jam/claude-faf-mcp/issues](https://github.com/Wolfe-Jam/claude-faf-mcp/issues)

### Email Support

For private inquiries:

**team@faf.one**

**Use email for**:
- IANA registration questions
- Format stewardship matters
- Partnership inquiries
- Security concerns (see [SECURITY.md](SECURITY.md))
- Media requests

**Do not use email for**:
- General questions (use discussions)
- Specification issues (use issues)
- Tool support (use tool-specific channels)

## Contributing

Want to improve the specification?

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on:
- Proposing format enhancements
- Fixing specification issues
- Adding examples
- Improving documentation

## Format Stewardship

The .faf format is:

- **IANA registered** as application/vnd.faf+yaml
- **MIT licensed** for universal adoption
- **Openly governed** through community discussion
- **Backward compatible** by design

**Format steward**: Wolfe James ([ORCID: 0009-0007-0801-3841](https://orcid.org/0009-0007-0801-3841))

## Specification Version Support

- **Current (1.x)**: Full support and active development
- **Previous**: All previous versions remain valid (backward compatible)

## Implementation Ecosystem

### Official Tools

- **faf-cli** (6,000+ npm downloads) - Command-line tool
- **claude-faf-mcp** (5,000+ npm downloads) - Claude Desktop integration
- **Chrome Extension** - Browser integration (Google-approved)

### Community Tools

See [faf.one/ecosystem](https://faf.one) for community-built integrations.

Want to add your tool? Open a discussion or PR.

## Best Practices

### File Naming

- Use `.faf` extension
- Common names: `project.faf`, `context.faf`
- Place in project root for discoverability

### Version Control

- Commit .faf files to version control
- Update with project changes
- Use in CI/CD for consistent context

### AI Tool Integration

- Most AI tools auto-detect .faf files
- Improves context quality immediately
- Works across Claude, ChatGPT, Gemini, etc.

## Learning Resources

### Examples

See EXAMPLES/ directory for:
- Minimal .faf file
- Complete .faf file
- Various project types
- Integration examples

### Tutorials

Visit faf.one for:
- Getting started guide
- Format specification walkthrough
- Integration tutorials
- Best practices

## Response Times

Expected response times:

- **Critical spec issues**: 24-48 hours
- **Specification questions**: 2-5 business days (community-driven)
- **Enhancement proposals**: Reviewed in planning cycles
- **Email inquiries**: 1-3 business days

## Related Standards

.faf builds on established standards:

- **YAML 1.2** - Base format
- **IANA Media Types** - Official registration
- **Semantic Versioning** - Version scheme
- **RFC 2119** - Specification keywords

## Community

Join the .faf community:

- **GitHub Discussions** - Q&A and proposals
- **npm** - Download implementations
- **faf.one** - Official website
- **ORCID** - Format creator: [0009-0007-0801-3841](https://orcid.org/0009-0007-0801-3841)

## License

The .faf format specification is MIT licensed. See [LICENSE](LICENSE) for details.

## Format History

.faf was created in August 2025 by Wolfe James, drawing on 30 years of format design experience:

- **1985-1990s**: Commodore Amiga .iff format
- **2000s**: 3D simulation formats
- **2025**: .faf format (IANA registered)

Built to last. Built for everyone.

---

**Universal AI context. Open specification. Free forever.**

Format designed for decades of interoperability.
