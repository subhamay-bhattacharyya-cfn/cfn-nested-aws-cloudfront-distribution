# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a **CloudFormation template repository** that provides a reusable nested stack template for serving an existing S3 bucket through a CloudFront distribution using Origin Access Control (OAC). The template is designed to be referenced by parent CloudFormation stacks.

**Key characteristics:**

- Nested CloudFormation template (referenced via `TemplateURL`)
- CloudFront distribution in front of an existing S3 origin bucket (the bucket is not created by this template)
- Origin Access Control (sigv4) so the bucket stays private; the bucket policy grants `s3:GetObject` only to this distribution
- Distribution name and `Name` tag built from project, base name, environment, and region
- Website is served only through CloudFront; no direct S3 website endpoint is exposed
- Automated semantic versioning and releases
- AWS OIDC authentication for CI/CD deployments

## Project Structure

```text
cloudformation/
├── template.yaml                    # Nested template: CloudFront distribution + OAC + origin bucket policy
├── parameters.json                  # Parameter values for CI deployments
└── stack-config.json                # Stack name, template file, and parameter file used by CI

.env/
└── environments.yaml                # Environment-to-GitHub-environment mapping and regions

.github/workflows/
├── ci.yaml                          # Loads stack config and calls the reusable CI build workflow
├── release.yaml                     # Semantic release on push to main
├── create-branch.yaml               # Auto-create feature branches from issues
├── claude.yaml                      # Claude Code workflow
├── claude-code-review.yaml          # Claude Code review workflow
├── notify.yaml                      # Notification workflow
└── setup-environments.yaml          # GitHub environment setup

scripts/plugins/
├── release.config.js                # Semantic-release configuration
└── (other release plugins)          # Commit analysis, notes generation, publish, verification

.claude/
└── .skills/                         # Project skills

.devcontainer/
└── devcontainer.json                # Dev container setup (Node.js 20)

package.json                         # Dependencies: semantic-release, commitizen
README.md                            # Template documentation and usage examples
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

This repo provides a **nested stack template** — referenced from a parent/root CloudFormation stack via `TemplateURL`. The template is self-contained and exposes outputs for the parent stack.

- **Parent stack** calls: `AWS::CloudFormation::Stack` with `TemplateURL` pointing to S3
- **Nested template** exposes values via the `Outputs` section
- Parent retrieves outputs via `!GetAtt NestedStack.Outputs.OutputKey`

### Naming Convention

The distribution `Name` tag and the OAC name are derived from parameters:

```bash
{ProjectName}-{DistributionBaseName}-{Environment}-{AWS::Region}
```

Example: `myproject-cdn-devl-us-east-1`

The OAC name appends `-oac` to this pattern.

### Distribution Comment

CloudFront has no name or description field on the distribution itself. The `Description` parameter is passed as the distribution `Comment`, which the console shows as the description. The default is an empty string.

## Key Files to Understand

### `cloudformation/template.yaml`

**Purpose:** Creates a CloudFront distribution that fronts an existing S3 bucket, with OAC and a bucket policy that lets only that distribution read objects.

**Key inputs:**

- `ProjectName` (required): Project prefix, lowercase letters, numbers, and hyphens, max 20 characters
- `DistributionBaseName` (default: `cdn`): Base name component
- `Environment` (default: `devl`): Environment label
- `CiSuffix` (default: empty): Optional suffix for unique CI/CD deployments
- `Description` (default: empty): Distribution comment shown in the console
- `OriginBucketName` (required): Name of the existing S3 origin bucket
- `OriginBucketRegion` (required): Region of the existing S3 origin bucket
- `DefaultRootObject` (default: `index.html`): Object served at the root URL
- `PriceClass` (default: `PriceClass_100`): `PriceClass_100`, `PriceClass_200`, or `PriceClass_All`

**Key outputs:**

- `DistributionId`: CloudFront distribution ID
- `DistributionDomainName`: Domain name of the distribution; this is the address users should use

**Features:**

- Origin uses the S3 REST endpoint (`{bucket}.s3.{region}.amazonaws.com`) with OAC; do not switch it to the S3 website endpoint, because OAC does not work with website endpoints
- HTTP/2 and HTTP/3, IPv6, redirect-to-HTTPS, compression enabled
- Managed `CachingOptimized` cache policy, GET/HEAD only
- Default CloudFront certificate (no custom domain)
- Bucket policy statement `AllowCloudFrontServicePrincipalReadOnly` grants `s3:GetObject` to the distribution via `AWS:SourceArn`

**Gotchas:**

- If the origin bucket uses a customer managed KMS key (SSE-KMS), CloudFront also needs `kms:Decrypt` in that key's key policy, scoped to the distribution ARN. The bucket policy cannot grant this. Without it, requests return `AccessDenied`.
- The bucket policy grants only `s3:GetObject`, so a missing object also returns `AccessDenied` rather than `NoSuchKey`.
- The origin bucket is not managed by this template, but `OriginBucketPolicy` is an `AWS::S3::BucketPolicy`, which replaces the bucket's entire existing policy. Any other statements on the bucket will be removed on deploy.

### `cloudformation/stack-config.json` and `cloudformation/parameters.json`

`stack-config.json` names the stack, template file, and parameter file that CI deploys. `parameters.json` holds the parameter values for that deployment. Parameter keys are case-sensitive and must match the template exactly.

### `.github/workflows/ci.yaml`

**Triggered on:**

- Manual `workflow_dispatch`
- Push, pull request, and path-filter triggers are currently commented out

**Process:**

1. `load-config` job reads `cloudformation/stack-config.json` and writes the stack name, template file, and parameter file to the job outputs and step summary
2. `ci-build` job calls the reusable workflow `subhamay-bhattacharyya-gha/cfn-ci-build-reusable-wf` with the `ci` GitHub environment and the stack name

**Environment setup:**

- The reusable workflow expects GitHub environment `ci` with AWS OIDC trust configured
- `.env/environments.yaml` maps `ci` and `devl` to the `AWS-CFN-TEMPLATES` GitHub environment and lists `us-east-1` as the region

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
aws cloudformation validate-template --template-body file://cloudformation/template.yaml
```

**Manual stack deployment:**

```bash
aws cloudformation deploy \
  --template-file cloudformation/template.yaml \
  --stack-name cfn-nested-aws-cloudfront-distribution-stack \
  --parameter-overrides \
    ProjectName=myproject \
    OriginBucketName=my-origin-bucket \
    OriginBucketRegion=us-east-1 \
    Environment=devl \
  --region us-east-1
```

Then confirm the origin bucket has the default root object (for example `index.html`) and, if the bucket uses SSE-KMS, that the key policy allows `kms:Decrypt` for the distribution before testing the `DistributionDomainName` URL.

## AWS Credentials & Environment Variables

**GitHub environment `ci` requirements:**

- AWS OIDC trust for the role used by the reusable CI workflow
- AWS region `us-east-1` (per `.env/environments.yaml`)

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

## When Modifying the Template

1. **Edit `cloudformation/template.yaml`**
2. **Update `cloudformation/parameters.json`** if parameters are added, renamed, or removed
3. **Test locally** with `aws cloudformation validate-template`
4. **Create a PR** with a conventional commit message (e.g., `feat: add Description parameter`)
5. **Merge to main** → release workflow creates version tag and GitHub release

## Dev Container

Pre-configured with:

- Node.js 20
- GitHub Copilot extension

Use via VS Code: `code --remote-container-url <repo-url>`

## Current Branch

Main branch is the release branch. Feature work branches from here and merges back via PR. Branch naming follows: `{type}/CFN-{issue-number}-{slug}` (e.g., `feature/CFN-42-add-encryption`).
