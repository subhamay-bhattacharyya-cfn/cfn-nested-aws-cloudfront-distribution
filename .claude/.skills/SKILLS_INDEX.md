---
name: skills-index
description: Index of available Claude skills for the CloudFormation template repository
metadata:
  type: reference
---

# Skills Index

Reference guide for the skills available in the cloudformation-template repository.

## Available Skills

### 1. [GitHub CI/CD Workflow](./github-ci-workflow/SKILL.md)

**Category:** Project-specific  
**Description:** CloudFormation Template Lifecycle CI/CD workflow automation

Covers:

- Workflow triggers (manual dispatch, pull requests)
- Workflow phases (validation, deployment, cleanup)
- Required AWS setup (OIDC configuration)
- GitHub environment variables and secrets
- All workflow jobs and their responsibilities
- Stack naming conventions
- Troubleshooting common issues
- Best practices for template development
- Useful AWS CLI commands
- FAQ for workflow management

**File:** [.github/workflows/ci.yaml](../../.github/workflows/ci.yaml)

---

## Quick Reference

| Skill | Purpose | Main File |
| ------- | --------- | ----------- |
| GitHub CI/CD Workflow | CloudFormation deployment automation | .github/workflows/ci.yaml |

---

## Usage Guide

### For Running the Workflow

1. Read [GitHub CI/CD Workflow](./github-ci-workflow/SKILL.md) skill
2. Set up GitHub environment `ci` with required variables
3. Configure AWS OIDC role
4. Create parameter files (`parameters/ci.json`, `parameters/policy-ci.json`)
5. Trigger workflow manually or via PR

### For Troubleshooting Workflow Failures

1. Check workflow logs in GitHub Actions tab
2. Review CloudFormation stack events in AWS console
3. Refer to troubleshooting section in [GitHub CI/CD Workflow](./github-ci-workflow/SKILL.md)
4. Use AWS CLI commands to inspect stack state

### For Modifying the Workflow

1. Update `.github/workflows/ci.yaml`
2. Test changes in a feature branch (workflow runs on PR)
3. Verify all jobs complete successfully
4. Merge to main once validated

---

## Workflow Overview

The CloudFormation Template Lifecycle workflow:

1. **Validates** CloudFormation syntax
2. **Deploys** S3 bucket and policy stacks to AWS
3. **Cleans up** stacks automatically after testing
4. **Reports** comprehensive status summary

**Triggers:**
- Manual via GitHub Actions UI
- Automatic on PR to `main` with template/parameter changes
- Programmatic via GitHub CLI or API

---

## Key Files

- [.github/workflows/ci.yaml](../../.github/workflows/ci.yaml) — Workflow definition
- [templates/](../../templates/) — CloudFormation templates
- [parameters/](../../parameters/) — Parameter files
- [CLAUDE.md](../../CLAUDE.md) — Project structure and guidelines

---

## Related Documentation

- [CONTRIBUTING.md](../../CONTRIBUTING.md) — Contribution guidelines
- [README.md](../../README.md) — Project overview
- [AWS CloudFormation Documentation](https://docs.aws.amazon.com/cloudformation/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)

---

## Best Practices Summary

### Workflow Execution

✅ Run validation step before manual dispatch  
✅ Check workflow logs for errors  
✅ Monitor CloudFormation stack events  
✅ Use GitHub environment variables for AWS config  

### Infrastructure as Code

✅ Version control all templates  
✅ Use parameter files for configuration  
✅ Follow stack naming conventions  
✅ Document template inputs/outputs  

### Security

✅ Use OIDC for AWS authentication  
✅ Store configuration in GitHub environment variables  
✅ Never commit AWS credentials  
✅ Restrict environment access by branch  

---

## Questions?

- **How to trigger the workflow?** → See [GitHub CI/CD Workflow](./github-ci-workflow/SKILL.md) - Workflow Triggers
- **How to set up AWS?** → See [GitHub CI/CD Workflow](./github-ci-workflow/SKILL.md) - Required Setup
- **How to troubleshoot?** → See [GitHub CI/CD Workflow](./github-ci-workflow/SKILL.md) - Troubleshooting

---
