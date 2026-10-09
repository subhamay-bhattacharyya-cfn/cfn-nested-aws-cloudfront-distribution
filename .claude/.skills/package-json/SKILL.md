---
name: package-json
description: Package.json configuration and requirements
category: project-specific
command: Reference
tags: [package, dependencies, npm, configuration]
---

# Package.json Skill

Configuration, requirements, and best practices for package.json in this GitHub Action template.

## Overview

The `package.json` file is critical for this project. It defines dependencies, scripts, metadata, and must adhere to specific requirements for proper operation.

## Key File: package.json

Located at repository root: [package.json](../../../package.json)

## Required Fields

These fields **MUST** be present and correct:

### name

**Requirement:** Must contain the current repository name

```json
{
  "name": "github-action-template"
}
```

Why: Identifies the package uniquely in npm registry and semantic-release versioning.

### description

**Requirement:** Must contain a description of the repository

```json
{
  "description": "A template repository for creating reusable GitHub Actions"
}
```

Why: Helps users understand project purpose in registries and documentation.

### version

**Requirement:** Automatically managed by semantic-release

```json
{
  "version": "1.2.3"
}
```

Why: Semantic-release auto-increments based on conventional commits:
- `feat:` → MINOR (1.2.3 → 1.3.0)
- `fix:` → PATCH (1.2.3 → 1.2.4)
- `BREAKING CHANGE:` → MAJOR (1.2.3 → 2.0.0)

**Do NOT manually edit version** — let semantic-release manage it.

## Dependencies

### Management

```bash
# Install dependencies
npm ci

# Add new dependency
npm install package-name

# Update dependencies
npm update

# Check for vulnerabilities
npm audit
```

### Lock File

**package-lock.json** maintains exact versions:
- Always commit to version control
- Ensures reproducible installations
- Managed by npm automatically

## Scripts

Define common tasks:

```json
{
  "scripts": {
    "test": "jest",
    "lint": "eslint .",
    "commit": "cz commit",
    "release": "semantic-release",
    "build": "npm run prepare"
  }
}
```

### Available Scripts

| Script | Purpose | Usage |
|--------|---------|-------|
| `npm ci` | Clean install dependencies | `npm ci` |
| `npm test` | Run test suite | `npm test` |
| `npm run lint` | Check code quality | `npm run lint` |
| `npm run commit` | Conventional commit | `npm run commit` |
| `npm run release` | Trigger semantic release | Usually automatic |
| `npm run build` | Build/prepare files | `npm run build` |

## Semantic-Release Configuration

Link semantic-release config:

```json
{
  "release": {
    "extends": "semantic-release-config"
  }
}
```

Or reference external config file:

```json
{
  "semantic-release": "./scripts/plugins/release.config.js"
}
```

## Conventional Commits

Use commitizen for formatted commits:

```bash
npm run commit
```

Triggers automatic version bumping based on commit type:
- `feat:` → MINOR bump
- `fix:` → PATCH bump
- `docs:`, `chore:`, `test:`, `refactor:` → No bump
- `BREAKING CHANGE:` → MAJOR bump

## Engines

Specify Node.js and npm version requirements:

```json
{
  "engines": {
    "node": ">=20.0.0",
    "npm": ">=10.0.0"
  }
}
```

Ensures developers use compatible versions.

## Repository Metadata

```json
{
  "repository": {
    "type": "git",
    "url": "https://github.com/user/repo.git"
  },
  "bugs": {
    "url": "https://github.com/user/repo/issues"
  },
  "homepage": "https://github.com/user/repo#readme"
}
```

## Author and License

```json
{
  "author": "Your Name <email@example.com>",
  "license": "MIT"
}
```

## Keywords

Help users discover package:

```json
{
  "keywords": [
    "github-action",
    "composite-action",
    "template",
    "automation"
  ]
}
```

## DevDependencies vs Dependencies

- **dependencies** — Required at runtime/installation
- **devDependencies** — Only needed for development

```json
{
  "dependencies": {
    "express": "^4.18.0"
  },
  "devDependencies": {
    "jest": "^29.0.0",
    "eslint": "^8.0.0"
  }
}
```

## Versioning Strategy

Semantic Versioning (SEMVER): `MAJOR.MINOR.PATCH`

- **MAJOR** (X.0.0) — Breaking changes
- **MINOR** (0.X.0) — New features (backward compatible)
- **PATCH** (0.0.X) — Bug fixes

## Best Practices

1. **Commit lock file** — Always check in package-lock.json
2. **Use npm ci** — For consistent installations (CI/CD)
3. **Regular updates** — Keep dependencies current
4. **Security audits** — Run `npm audit` regularly
5. **Document scripts** — Add comments for non-obvious scripts
6. **Pin versions** — Use exact versions for critical dependencies
7. **Check metadata** — Keep name/description accurate
8. **Test after updates** — Run tests after dependency changes

## Workflow

1. **Make changes** → Update code
2. **Commit** → Use `npm run commit`
3. **Push to main** → Triggers automatic versioning
4. **Version bumped** → Semantic-release updates package.json
5. **Release published** → GitHub release created

## Security

```bash
# Check for vulnerabilities
npm audit

# Fix vulnerabilities
npm audit fix

# Check specific package
npm list package-name
```

## Related Files

- [package-lock.json](../../../package-lock.json) — Locked versions
- [.releaserc.json](../../../.releaserc.json) — Semantic-release config
- [CLAUDE.md](../../../CLAUDE.md) — Project rules and structure

## Troubleshooting

### Version Not Updating?
- Verify conventional commit format: `feat:`, `fix:`, `BREAKING CHANGE:`
- Check `.releaserc.json` configuration
- Review semantic-release workflow logs

### Dependency Issues?
- Run `npm ci` for clean install
- Check Node.js version: `node --version`
- Verify npm version: `npm --version`
- Clear cache: `npm cache clean --force`

### Permission Issues?
- Verify npm registry access
- Check authentication tokens
- Review GitHub Actions permissions
