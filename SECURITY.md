# Security Policy

## Format Security

As a specification, .faf format security focuses on design principles that enable secure implementations rather than implementation vulnerabilities.

## Supported Versions

The .faf format specification maintains backward compatibility:

| Version | Status              |
| ------- | ------------------- |
| 1.x     | :white_check_mark: Current specification |

## Reporting Specification Security Issues

If you discover a security concern in the .faf format specification itself:

### Please Do Not

- Open a public GitHub issue for security concerns
- Disclose publicly before specification updates
- Exploit issues in production implementations

### Please Do

**Report specification security concerns via email to: team@faf.one**

Include:

- **Specification concern** - Which part of the format spec
- **Security implication** - How it could be exploited
- **Affected implementations** - Known tools impacted
- **Proposed fix** - Suggested specification changes
- **Backward compatibility** - Impact on existing deployments

### What to Expect

1. **Acknowledgment within 24 hours**
2. **Assessment within 72 hours** - Analysis of specification impact
3. **Coordination with implementations** - Working with tool maintainers
4. **Specification update** - If warranted
5. **Coordinated disclosure** - Synchronized with implementation fixes

## Security Design Principles

The .faf format is designed with security considerations:

### Data Validation

The specification requires:
- Structured YAML format with schema validation
- Explicit field definitions
- Type constraints
- Size limitations (recommended, not enforced)

**Implementation guidance**: Parsers should validate against schema before processing.

### No Executable Content

The .faf format is declarative data only:
- No code execution
- No script embedding
- No command injection vectors
- Pure data representation

### Privacy Considerations

The specification recommends:
- Avoid embedding credentials
- Use references to sensitive data, not inline values
- Support for redacted contexts
- Clear data boundaries

**Example - Secure**:
```yaml
database_config:
  type: "postgresql"
  credentials_ref: "vault://prod/db-creds"  # Reference, not inline
```

**Example - Insecure**:
```yaml
database_config:
  password: "actual-password-here"  # Never do this
```

### File System Safety

Specification does not mandate file operations, but recommends:
- Parsers should validate file paths
- Implementations should sandbox operations
- No arbitrary file access
- User consent for sensitive operations

## Implementation Security

While the specification is secure by design, implementations should:

### Parser Security

- **Input validation** - Reject malformed YAML
- **Size limits** - Prevent resource exhaustion
- **Schema enforcement** - Validate against .faf schema
- **Error handling** - Safe failure modes

### Tool Integration

- **Least privilege** - Minimal required permissions
- **Sandboxing** - Isolate .faf processing
- **Audit logging** - Track sensitive operations
- **User consent** - Explicit permission for file access

### YAML Security

The format uses YAML, which has known security considerations:

**Specification requirements**:
- Parsers MUST disable dangerous YAML features
- No arbitrary object instantiation
- No code evaluation
- No external entity loading

**Safe YAML subset**:
- Scalars (strings, numbers, booleans)
- Sequences (arrays)
- Mappings (objects)
- No custom tags
- No anchors/aliases (recommended)

## Known Implementation Considerations

### faf-cli Security

See [faf-cli SECURITY.md](https://github.com/Wolfe-Jam/faf-cli/blob/main/SECURITY.md)

### claude-faf-mcp Security

See [claude-faf-mcp SECURITY.md](https://github.com/Wolfe-Jam/claude-faf-mcp/blob/main/SECURITY.md)

## Specification Updates

Security-related specification changes:

1. **Immediate** - Critical security design flaws
2. **Next release** - Security enhancements
3. **Coordinated** - Synced with implementation updates

## IANA Registration Security

The .faf format is registered with IANA as application/vnd.faf+yaml:

- **Media type safety** - YAML-based, not executable
- **Content sniffing protection** - Explicit MIME type
- **Standard compliance** - Follows YAML 1.2 specification

## Best Practices for .faf Files

### For File Authors

- Never embed credentials or secrets
- Use references to sensitive configuration
- Validate before committing
- Review for sensitive information
- Use .gitignore for local .faf files with secrets

### For Tool Developers

- Validate all inputs
- Use safe YAML parsers
- Implement size limits
- Require user consent for sensitive operations
- Sandbox file operations
- Follow principle of least privilege

### For System Integrators

- Treat .faf files as configuration data
- Apply standard configuration security practices
- Use access controls
- Audit .faf file changes
- Encrypt at rest if containing sensitive references

## Vulnerability Disclosure Timeline

For specification-level security issues:

1. **Day 0**: Report received
2. **Day 1**: Acknowledgment
3. **Day 3**: Impact assessment
4. **Day 7-30**: Specification fix developed
5. **Day 30**: Implementation coordination
6. **Day 60**: Coordinated disclosure
7. **Day 90**: Public disclosure if delayed

## Security Contact

- **Specification security**: team@faf.one
- **Format steward**: Wolfe James ([ORCID: 0009-0007-0801-3841](https://orcid.org/0009-0007-0801-3841))

## Additional Resources

- [YAML Security Issues](https://yaml.org/spec/1.2/spec.html#id2761803)
- [IANA Media Type Security Considerations](https://www.iana.org/assignments/media-types/)
- [OWASP Data Validation](https://owasp.org/www-project-proactive-controls/v3/en/c5-validate-inputs)

## Responsible Disclosure

We appreciate security researchers who:
- Report issues privately first
- Allow time for fixes before disclosure
- Coordinate with implementation maintainers
- Help improve the format's security

---

**Last updated**: November 2025

**Format security by design. Implementation security by practice.**
