---
name: readme-md
description: Manage and validate README badges for the CloudFormation template repository
---

# README Badge Management Skill

This skill helps manage, validate, and update badges in the README.md file for the CloudFormation S3 Template Repository.

## Badges in Use

The README includes the following badges at the top:

1. **Built with Claude Code** - Indicates the project was built using Claude Code
2. **Commit Activity** - Shows total commit activity
3. **Last Commit** - Displays the most recent commit date
4. **Release Date** - Shows the date of the latest release
5. **Repo Size** - Displays the repository size
6. **File Count** - Shows the total file count
7. **Issues** - Displays open issues count
8. **Top Language** - Shows the primary programming language
9. **Status** - Custom endpoint showing project status

## Badge URLs

All badges use shields.io endpoints with the repository: `subhamay-bhattacharyya-cfn/cloudformation-template`

### Gist Endpoint

The custom status badge uses a gist endpoint:
```
https://gist.githubusercontent.com/bsubhamay/0d518b3ce02fae859c9a3c4d3bb6b94d/raw/cloudformation-template.json
```

**Gist Link:** https://gist.github.com/bsubhamay/0d518b3ce02fae859c9a3c4d3bb6b94d

The JSON format for the status endpoint is:

```json
{
  "schemaVersion": 1,
  "label": "status",
  "message": "in progress",
  "color": "yellow",
  "style": "flat"
}
```

## Common Tasks

### Update Status Badge Color

To change the status badge appearance, update the gist at:
- **Gist ID**: `0d518b3ce02fae859c9a3c4d3bb6b94d`
- **File**: `cloudformation-template.json`
- **URL**: https://gist.github.com/bsubhamay/0d518b3ce02fae859c9a3c4d3bb6b94d

Supported colors: `green` (28a745), `yellow` (dfb317), `red` (e05d44), `blue` (007ec6)

### Verify Badges

All badges should display correctly once pushed to main. To verify:
1. Check the README.md in the GitHub web interface
2. Ensure all badge URLs use the correct repository name (`subhamay-bhattacharyya-cfn/cloudformation-template`)
3. Verify the gist endpoint is accessible and returns valid JSON

### Add New Badge

To add a new badge:
1. Identify the shields.io endpoint for the badge
2. Update the repository reference to `subhamay-bhattacharyya-cfn/cloudformation-template`
3. Add to the README badges line at the top
4. Ensure proper spacing with `&nbsp;` between badges

## Repository Reference

- **Repository**: `subhamay-bhattacharyya-cfn/cloudformation-template`
- **Organization**: subhamay-bhattacharyya-cfn
- **Type**: CloudFormation template repository

## See Also

- [shields.io](https://shields.io) - Badge generation service
- [GitHub API badges](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-repository-badges) - Available metrics
