---
name: github-ci-workflow
description: CloudFormation Template Lifecycle CI/CD workflow guide
category: project-specific
command: Reference
tags: [ci-cd, cloudformation, github-actions, aws, automation, deployment]
---

# GitHub CI/CD Workflow Skill

Complete guide to the CloudFormation Template Lifecycle CI/CD workflow for the cloudformation-template repository.

## Overview

The `ci.yaml` workflow automates CloudFormation template validation, deployment, and cleanup. It provides a complete infrastructure testing lifecycle with AWS integration via OIDC authentication.

### Workflow Phases

1. **Configuration** — Load AWS environment settings
2. **Validation** — Check CloudFormation syntax
3. **Deployment** — Deploy S3 bucket and policy stacks
4. **Cleanup** — Destroy stacks in reverse order
5. **Reporting** — Display comprehensive summary

## Workflow Triggers

### Manual Dispatch
- **GitHub UI:** Actions tab → "CloudFormation Template Lifecycle" → "Run workflow"
- **GitHub CLI:** `gh workflow run ci.yaml -r main`

### Automatic on Pull Request
- Triggered when PR created/updated against `main`
- Only if changes affect:
  - `templates/**` — CloudFormation templates
  - `parameters/**` — Parameter files
  - `.github/workflows/ci.yaml` — Workflow file

## Required Setup

### GitHub Environment Variables
Create `ci` environment with these variables:
- `AWS_REGION` — AWS region (e.g., `us-east-1`)
- `AWS_ACCOUNT_ID` — AWS account ID
- `OIDC_ROLE_NAME` — IAM role for OIDC
- `CFN_TEMPLATES_S3_BUCKET` — S3 bucket name (optional)

**Setup:** Repo Settings → Environments → Create `ci` → Add variables

### AWS OIDC Configuration
1. Trust GitHub OIDC provider in IAM role
2. Role must have CloudFormation and S3 permissions
3. No credentials stored; uses temporary tokens

### Parameter Files Required
- `parameters/ci.json` — Bucket parameters
- `parameters/policy-ci.json` — Policy parameters

## Workflow Jobs

### `get-config`
Loads and outputs AWS environment configuration for all downstream jobs.

**Outputs:**
```
environment, aws-region, aws-account-id, oidc-role-name, cfn-templates-s3-bucket
```

### `validate-templates`
Validates CloudFormation syntax for both templates without deploying.

```bash
aws cloudformation validate-template --template-body file://templates/s3-bucket.yaml
aws cloudformation validate-template --template-body file://templates/s3-bucket-policy.yaml
```

### `deploy-bucket`
Deploys S3 bucket stack using CloudFormation.

**Stack name:** `{repo-name}-bucket-{environment}`  
**Outputs:** Bucket name for policy stack dependency

### `deploy-policy`
Deploys S3 bucket policy stack with bucket reference.

**Stack name:** `{repo-name}-policy-{environment}`  
**Parameter override:** Bucket name from `deploy-bucket`

### `notify-deployment-completion`
Checkpoint that validates deployment phase success before cleanup.

### `destroy-policy-stack`
Deletes policy stack in cleanup phase (deployed second, deleted first).

### `destroy-bucket-stack`
Deletes bucket stack after policy removal.

### `display-full-cycle-summary`
Generates comprehensive GitHub Actions step summary showing:
- Phase statuses (success/failure)
- Stack names deployed
- AWS region and environment
- Triggered by actor and event type

## Key Configuration

```yaml
env:
  DEPLOY_ENV: ci
  # Allowed: ci, devl, test, stag, prod
```

Change `DEPLOY_ENV` to deploy to different environments.

## Stack Naming Convention

```
{repository-name}-{stack-type}-{environment}
```

**Examples:**
- `cloudformation-template-bucket-ci`
- `cloudformation-template-policy-ci`

## Useful AWS CLI Commands

```bash
# List all stacks
aws cloudformation list-stacks --region us-east-1

# Describe specific stack
aws cloudformation describe-stacks --stack-name cloudformation-template-bucket-ci --region us-east-1

# View stack events
aws cloudformation describe-stack-events --stack-name cloudformation-template-bucket-ci

# Get stack outputs
aws cloudformation describe-stacks --stack-name cloudformation-template-bucket-ci \
  --query 'Stacks[0].Outputs' --region us-east-1

# Validate template locally
aws cloudformation validate-template --template-body file://templates/s3-bucket.yaml
```

## Troubleshooting

### Template Validation Fails
- Check YAML syntax (indentation)
- Verify CloudFormation resource types exist
- Review parameter references

### Deployment Fails
- Check IAM role permissions in AWS console
- Verify parameter files have valid values
- S3 bucket names must be globally unique
- Review CloudFormation stack events

### Cleanup Fails
- Check for orphaned resources
- Verify stack deletion order (policy → bucket)
- Review stack protection settings

### OIDC Authentication Fails
- Verify OIDC role exists and is configured
- Check GitHub OIDC provider trust relationship
- Confirm role has required permissions

## Best Practices

✅ **Do:**
- Validate templates before pushing
- Test parameter files locally
- Review CloudFormation stack events on failure
- Use descriptive stack outputs
- Version control all templates and parameters

❌ **Don't:**
- Hardcode values in templates
- Skip validation step
- Manually modify deployed stacks
- Leave failed stacks in AWS
- Store credentials in code or workflows

## Related Files

- [.github/workflows/ci.yaml](../../../.github/workflows/ci.yaml) — Workflow definition
- [templates/](../../../templates/) — CloudFormation templates
- [parameters/](../../../parameters) — Parameter files
- [CLAUDE.md](../../../CLAUDE.md) — Project guidelines

## AWS Services Used

- **CloudFormation** — Infrastructure orchestration
- **S3** — Object storage
- **IAM** — OIDC authentication
- **CloudWatch** — Logs

## FAQ

**Q: How do I deploy to a different environment?**
A: Update `DEPLOY_ENV` in `.github/workflows/ci.yaml` (values: ci, devl, test, stag, prod)

**Q: Can I skip the cleanup phase?**
A: Yes, comment out `destroy-*-stack` jobs to keep stacks for inspection

**Q: Why stacks deleted in reverse order?**
A: Policy depends on bucket; CloudFormation requires dependent resources deleted first

**Q: How to debug locally?**
A: Run `aws cloudformation validate-template --template-body file://templates/s3-bucket.yaml`

**Q: Can I run on a schedule?**
A: Yes, add `schedule` trigger to workflow file for automated runs
