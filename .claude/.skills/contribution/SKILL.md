---
name: contribution
description: Guidelines and workflow for contributing to the GitHub Action template
category: project-specific
command: Reference
tags: [contributing, workflow, collaboration]
---

# Contribution Skill

Guidelines and workflow for contributing to this GitHub Action template repository.

## Overview

This skill outlines the complete contribution process, from setup through merging.

## Development Setup

### Prerequisites

- Node.js 20 or higher
- npm 10 or higher
- Git

### Initial Setup

```bash
# Fork and clone the repository
git clone https://github.com/your-username/repo.git
cd repo

# Add upstream remote
git remote add upstream https://github.com/original-owner/repo.git

# Install dependencies
npm ci
```

## Branch Naming Convention

Use the pattern: `{type}/GHA-{issue-number}-{description}`

Types:
- `feature/` — New functionality
- `bug/` — Bug fixes
- `docs/` — Documentation
- `chore/` — Maintenance
- `refactor/` — Code refactoring

Examples:
- `feature/GHA-58-add-logging`
- `bug/GHA-42-fix-auth`
- `docs/GHA-100-update-readme`

## Development Workflow

### 1. Create or Assign Issue

Create an issue with a clear title and description. When assigned, a feature branch is auto-created.

### 2. Code Changes

```bash
# Update your branch
git fetch upstream
git rebase upstream/main

# Write code and run tests
npm test
npm run lint
```

### 3. Commit Changes

Use conventional commits for automatic versioning:

```bash
npm run commit
```

Commit types:
- `feat:` → MINOR version bump
- `fix:` → PATCH version bump
- `docs:` → No version bump
- `BREAKING CHANGE:` → MAJOR version bump

### 4. Push & Create PR

```bash
git push origin feature/GHA-123-description
```

Then create a pull request with:
- Description of changes
- Related issue number
- Type of change (feature/fix/docs/etc)

### 5. Review & Merge

- All checks must pass
- Maintainers review and approve
- Merge to `main` triggers automatic release

## Testing

All changes must include appropriate tests:

```bash
npm test
```

## Documentation

- Update README.md if adding new features
- Update inline comments for complex logic
- Keep documentation synchronized with code

## Code Quality

Before submitting PR:
```bash
/code-review high --fix
/simplify
```

## Bug Reporting

Include in bug reports:
- Clear description
- Steps to reproduce
- Expected vs actual behavior
- Screenshots/logs
- Environment details

## Recognition

Contributors recognized in:
- CHANGELOG.md (significant changes)
- Project acknowledgments (major contributors)

## Questions?

- Open GitHub Discussions
- Contact maintainers via issues
- Check existing documentation

## Related Files

- [CONTRIBUTING.md](../../../CONTRIBUTING.md) — Full contribution guide
- [CLAUDE.md](../../../CLAUDE.md) — Project structure and rules
