# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a **CloudFormation template repository** that provides reusable nested stack templates for deploying S3 buckets with security best practices. Templates follow the nested stack pattern and are designed to be referenced by parent CloudFormation stacks.

**Key characteristics:**

- Nested CloudFormation templates (referenced via `TemplateURL`)
- Parameterized bucket naming with account ID, environment, and region
- S3 security defaults: versioning enabled, public access blocked
- Optional S3 bucket policy enforcement (encryption, secure transport)
- Automated semantic versioning and releases
- AWS OIDC authentication for CI/CD deployments

## Project Structure

```text
templates/
├── s3-bucket.yaml                # Nested template: S3 bucket creation
└── s3-bucket-policy.yaml         # Nested template: S3 bucket policy

parameters/
├── dev.json                       # Parameters for development environment
├── staging.json                   # Parameters for staging environment
├── prod.json                      # Parameters for production environment
├── policy-dev.json                # Bucket policy parameters (development)
├── policy-staging.json            # Bucket policy parameters (staging)
└── policy-prod.json               # Bucket policy parameters (production)

.github/workflows/
├── ci.yaml                        # Validates, deploys, and cleans up templates
├── release.yaml                   # Semantic release on push to main
└── create-branch.yaml             # Auto-create feature branches from issues

scripts/plugins/
├── release.config.js              # Semantic-release configuration
└── (other release plugins)        # Custom commit analysis, notes generation

.claude/
├── settings.json                  # Claude Code workspace settings
└── settings.local.json            # Local overrides

.devcontainer/
└── devcontainer.json              # Dev container setup (Node.js 20)

package.json                       # Dependencies: semantic-release, commitizen
README.md                          # Template documentation and usage examples
```

## Development Commands

### Install dependencies

```bash
npm ci
```

### Trigger semantic release (usually automatic on main)

```bash
npm run release
```

### Commit with conventional commit format

```bash
npx cz commit
```

Select `feat`, `fix`, or `chore` type. Only `feat` and `fix` trigger releases.

## Key Architecture Concepts

### Nested Stack Pattern

This repo provides **nested stack templates** — templates that are referenced from parent/root CloudFormation stacks via `TemplateURL`. The templates are self-contained and export outputs for cross-stack references.

- **Parent stack** calls: `AWS::CloudFormation::Stack` with `TemplateURL` pointing to S3
- **Nested templates** output values via `Outputs` section with `Export`
- Parent retrieves outputs via `!GetAtt NestedStack.Outputs.OutputKey`

### Bucket Naming Convention

Bucket names follow a deterministic pattern driven by parameters:

```bash
{ProjectName}-{BucketBaseName}-{AccountId}-{Environment}-{Region}[-{CiSuffix}]
```

Example: `myproject-cfn-bucket-123456789012-devl-us-east-1`

This ensures:

- Uniqueness across AWS accounts and regions
- Environment isolation
- Consistent naming for infrastructure automation

### Parameter-Driven Configuration

Both templates accept parameters to support:

- **Standalone mode**: Direct bucket name provided
- **Integrated mode**: Bucket name constructed from project/environment parameters

The `s3-bucket-policy.yaml` template checks if `BucketName` is provided; if not, it constructs the name using the same parameters as the bucket template.

## Key Files to Understand

### `templates/s3-bucket.yaml`

**Purpose:** Creates an S3 bucket with security defaults

**Key inputs:**

- `ProjectName` (required): Project prefix
- `BucketBaseName` (default: `cfn-bucket`): Base name component
- `environment`: Environment label (devl, stag, prod)
- `CiSuffix`: Optional suffix for CI/CD unique deployments

**Key outputs:**

- `S3BucketName`: Bucket name (exported for parent stack)
- `S3BucketArn`: Bucket ARN

**Features:**

- Versioning enabled by default
- Public access blocking enabled (all 4 options)
- Conditional naming: different bucket name with/without CI suffix

### `templates/s3-bucket-policy.yaml`

**Purpose:** Applies an optional S3 bucket policy for encryption and transport security

**Key inputs:** Same as bucket template, plus `BucketName` (standalone mode)

**Behavior:**

- If `BucketName` provided (non-empty), use it directly
- Otherwise, construct name from ProjectName/BucketBaseName/environment/CiSuffix
- Enforces S3 encryption on PutObject
- Enforces HTTPS-only transport

### `.github/workflows/ci.yaml`

**Triggered on:**

- Manual workflow_dispatch (anytime)
- Pull requests (any branch)
- Pushes to `feature/**` and `bug/**` branches

**Path filter:** Only runs if changes to `templates/`, `parameters/`, or `.github/workflows/ci.yaml`

**Phases:**

1. **Validation:** `aws cloudformation validate-template` on both templates
2. **Deployment:** Creates CloudFormation stacks in CI environment
3. **Cleanup:** Destroys stacks (policy stack first, then bucket) for ephemeral testing

**Environment setup:**

- Reads config from GitHub environment variables: `AWS_REGION`, `AWS_ACCOUNT_ID`, `OIDC_ROLE_NAME`, `CFN_TEMPLATES_S3_BUCKET`
- Uses AWS OIDC for keyless authentication via `aws-actions/configure-aws-credentials`
- Requires GitHub environment `ci` with OIDC trust configured

### `.github/workflows/release.yaml`

**Triggered:** On push to main

**Process:**

1. Analyze commits (conventional format: `feat:`, `fix:`, `BREAKING CHANGE:`)
2. Generate release notes
3. Update CHANGELOG.md
4. Create GitHub release and tag
5. Commit version bump

**Release rules:**

- `feat:` → MINOR bump (0.1.0 → 0.2.0)
- `fix:` → PATCH bump (0.1.0 → 0.1.1)
- `BREAKING CHANGE:` → MAJOR bump (0.1.0 → 1.0.0)
- Other commits → no release

## Testing & Validation

**Manual template validation:**

```bash
aws cloudformation validate-template --template-body file://templates/s3-bucket.yaml
aws cloudformation validate-template --template-body file://templates/s3-bucket-policy.yaml
```

**Manual stack deployment:**

```bash
# Deploy bucket to dev environment
aws cloudformation deploy \
  --template-file templates/s3-bucket.yaml \
  --stack-name my-stack-dev \
  --parameter-overrides file://parameters/dev.json \
  --region us-east-1

# Deploy policy after bucket is created
aws cloudformation deploy \
  --template-file templates/s3-bucket-policy.yaml \
  --stack-name my-policy-dev \
  --parameter-overrides file://parameters/policy-dev.json \
  --region us-east-1
```

The CI workflow (ci.yaml) runs this full cycle automatically on PR, then cleans up.

## AWS Credentials & Environment Variables

**GitHub environment variables required in `ci` environment:**

- `AWS_REGION`: CloudFormation deployment region
- `AWS_ACCOUNT_ID`: AWS account to deploy into
- `OIDC_ROLE_NAME`: IAM role name for OIDC trust (uses `arn:aws:iam::{ACCOUNT_ID}:role/{ROLE_NAME}`)
- `CFN_TEMPLATES_S3_BUCKET`: S3 bucket where templates are stored

**OIDC setup:** The CI workflow uses AWS OIDC for keyless auth. The GitHub OIDC provider must trust the specified role.

## Conventional Commits & Release Flow

This repo enforces conventional commits to drive semantic versioning:

```bash
npx cz commit
```

Commit types:

- `feat: add support for X` → triggers MINOR release
- `fix: correct behavior of Y` → triggers PATCH release
- `chore: update deps` → no release
- `docs: clarify README` → no release

Only commits to `main` trigger releases. Feature branches use this format but releases happen on merge to main.

## When Modifying Templates

1. **Edit the template YAML** in `templates/`
2. **Update parameter files** in `parameters/` if new parameters added
3. **Test locally** with `aws cloudformation validate-template`
4. **Create a PR** with conventional commit message (e.g., `feat: add encryption key parameter`)
5. **CI validates and deploys** to dev environment automatically
6. **Merge to main** → release workflow creates version tag and GitHub release

## Dev Container

Pre-configured with:

- Node.js 20
- GitHub Copilot extension

Use via VS Code: `code --remote-container-url <repo-url>`

## Current Branch

Main branch is the release branch. Feature work branches from here and merges back via PR. Branch naming follows: `{type}/CFN-{issue-number}-{slug}` (e.g., `feature/CFN-42-add-encryption`).

