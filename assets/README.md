# FAF Assets

**Approved** graphics, logos, and screenshots for the FAF ecosystem.

This directory serves as the canonical source for FAF branding assets, referenced across all FAF repositories via GitHub CDN.

## Structure

```
assets/
├── screenshots/     # Product screenshots, demos, placement examples
├── logos/          # FAF logos (SVG, PNG formats)
└── branding/       # Social cards, banners, promotional assets
```

## Usage in Documentation

Reference assets via GitHub raw URLs in any FAF repository README:

```markdown
<!-- CLI README, MCP README, faf.one, etc. -->
<img src="https://raw.githubusercontent.com/Wolfe-Jam/faf/main/assets/screenshots/package-json-project-faf.png" alt="project.faf placement" width="500" />
```

## CDN Behavior

- **Cache**: GitHub CDN caches for ~5 minutes
- **Updates**: Push to main branch → CDN updates within minutes
- **Always latest**: Use `/main/` in URL path
- **Pinned version**: Use git tag like `/v2.0.0/` for stability

## Current Assets

### Screenshots
- `package-json-project-faf.png` (88KB) - Shows project.faf file placement between package.json and README.md in VS Code

### Logos
- Coming soon

### Branding
- Coming soon

## Benefits

- ✅ Single source of truth for FAF branding
- ✅ No asset duplication across repos (cli, mcp, etc.)
- ✅ Smaller npm packages (images not bundled)
- ✅ Update once, reflects everywhere
- ✅ Free GitHub CDN hosting

## Used By

- [faf-cli](https://github.com/Wolfe-Jam/faf-cli) - README documentation
- [claude-faf-mcp](https://github.com/Wolfe-Jam/claude-faf-mcp) - README documentation
- [faf.one](https://faf.one) - Website assets
- All FAF ecosystem repositories

---

**Asset Approval**: All images in this directory are approved for use in FAF documentation and marketing.

**Repository**: [github.com/Wolfe-Jam/faf](https://github.com/Wolfe-Jam/faf)
**License**: MIT
