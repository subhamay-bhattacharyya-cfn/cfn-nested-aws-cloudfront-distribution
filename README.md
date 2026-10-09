# CloudFormation CloudFront Distribution Nested Stack Template

<!-- Row 1: Status - Most Important -->
[![Release](https://github.com/subhamay-bhattacharyya-cfn/cfn-nested-aws-cloudfront-distribution/actions/workflows/release.yaml/badge.svg)](https://github.com/subhamay-bhattacharyya-cfn/cfn-nested-aws-cloudfront-distribution)&nbsp;[![GitHub Repo](https://img.shields.io/badge/GitHub-Repository-blue?logo=github)](https://github.com/subhamay-bhattacharyya-cfn/cfn-nested-aws-cloudfront-distribution)&nbsp;[![Issues](https://img.shields.io/github/issues/subhamay-bhattacharyya-cfn/cfn-nested-aws-cloudfront-distribution)](https://github.com/subhamay-bhattacharyya-cfn/cfn-nested-aws-cloudfront-distribution/issues)&nbsp;[![Last Commit](https://img.shields.io/github/last-commit/subhamay-bhattacharyya-cfn/cfn-nested-aws-cloudfront-distribution)](https://github.com/subhamay-bhattacharyya-cfn/cfn-nested-aws-cloudfront-distribution/commits)

<!-- Row 2: Code Quality -->
[![Top Language](https://img.shields.io/github/languages/top/subhamay-bhattacharyya-cfn/cfn-nested-aws-cloudfront-distribution)](https://github.com/subhamay-bhattacharyya-cfn/cfn-nested-aws-cloudfront-distribution)&nbsp;[![Commits](https://img.shields.io/github/commit-activity/t/subhamay-bhattacharyya-cfn/cfn-nested-aws-cloudfront-distribution)](https://github.com/subhamay-bhattacharyya-cfn/cfn-nested-aws-cloudfront-distribution/commits)

<!-- Row 3: Tech Stack -->
[![CloudFormation](https://img.shields.io/badge/CloudFormation-IaC-orange?logo=amazon&logoColor=white)](https://aws.amazon.com/cloudformation/)&nbsp;[![Built with Claude Code](https://img.shields.io/badge/Built_with-Claude_Code-D97757?logo=anthropic&logoColor=white)](https://claude.ai/)

<!-- Row 4: Repository Info -->
[![Files](https://img.shields.io/github/directory-file-count/subhamay-bhattacharyya-cfn/cfn-nested-aws-cloudfront-distribution)](https://github.com/subhamay-bhattacharyya-cfn/cfn-nested-aws-cloudfront-distribution)&nbsp;[![Repo Size](https://img.shields.io/github/repo-size/subhamay-bhattacharyya-cfn/cfn-nested-aws-cloudfront-distribution)](https://github.com/subhamay-bhattacharyya-cfn/cfn-nested-aws-cloudfront-distribution)&nbsp;[![Release Date](https://img.shields.io/github/release-date/subhamay-bhattacharyya-cfn/cfn-nested-aws-cloudfront-distribution)](https://github.com/subhamay-bhattacharyya-cfn/cfn-nested-aws-cloudfront-distribution/releases)

<!-- Row 5: Custom Metrics -->
[![Custom Endpoint](https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/bsubhamay/afbe99191e9ad64b76530399a6a8fe20/raw/cfn-nested-aws-cloudfront-distribution.json)](https://gist.github.com/subhamay-bhattacharyya/afbe99191e9ad64b76530399a6a8fe20)

This repository contains a nested CloudFormation template that creates a CloudFront distribution in front of an existing S3 origin bucket. The bucket stays private: CloudFront reads it through Origin Access Control (OAC), and the bucket policy allows only that distribution.

## Overview

This is a **nested stack template** designed to be invoked from a parent/root CloudFormation stack. The template is stored in this repository and should be uploaded to an S3 bucket for reference by parent stacks.

## Template File

- **`cloudformation/template.yaml`** — Nested template for the CloudFront distribution, OAC, and origin bucket policy
- **`cloudformation/parameters.json`** — Parameter values used by CI
- **`cloudformation/stack-config.json`** — Stack name, template file, and parameter file used by CI

## Template Features

- ✅ CloudFront distribution in front of an existing S3 origin bucket
- ✅ Origin Access Control (sigv4) — the bucket stays private
- ✅ Bucket policy grants `s3:GetObject` only to this distribution
- ✅ Redirect HTTP to HTTPS
- ✅ HTTP/2 and HTTP/3, IPv6 enabled
- ✅ Managed `CachingOptimized` cache policy with compression
- ✅ Distribution `Name` tag and comment for easy identification

## Parameters

| Parameter | Type | Default | Description |
| ----------- | ------ | --------- | ------------- |
| `ProjectName` | String | — | Project name used as the prefix for the distribution name (required; lowercase letters, numbers, hyphens; max 20 characters) |
| `DistributionBaseName` | String | `cdn` | Base name for the distribution |
| `environment` | String | `devl` | Deployment environment |
| `Description` | String | `""` | Free-text comment shown as the distribution description in the console |
| `OriginBucketName` | String | — | Name of the existing S3 bucket used as the origin (required) |
| `OriginBucketRegion` | String | — | Region of the existing S3 origin bucket (required) |
| `DefaultRootObject` | String | `index.html` | Object returned when the root URL is requested |
| `PriceClass` | String | `PriceClass_100` | `PriceClass_100`, `PriceClass_200`, or `PriceClass_All` |

## Outputs

- `DistributionId` — ID of the CloudFront distribution
- `DistributionDomainName` — Domain name of the distribution; use this URL to access the site

The site is served only through CloudFront. The S3 website endpoint is not exposed.

## Usage

### 1. Upload the Template to S3

```bash
aws s3 cp cloudformation/template.yaml s3://your-cfn-bucket/templates/cloudfront-distribution.yaml
```

### 2. Reference from Parent Stack

In your parent/root CloudFormation template:

```yaml
CloudFrontNestedStack:
  Type: AWS::CloudFormation::Stack
  Properties:
    TemplateURL: https://s3.amazonaws.com/your-cfn-bucket/templates/cloudfront-distribution.yaml
    Parameters:
      ProjectName: myproject
      DistributionBaseName: cdn
      environment: !Ref Environment
      Description: Marketing site distribution
      OriginBucketName: !Ref OriginBucketName
      OriginBucketRegion: us-east-1
    Tags:
      - Key: Environment
        Value: !Ref Environment

Outputs:
  DistributionDomainName:
    Value: !GetAtt CloudFrontNestedStack.Outputs.DistributionDomainName
```

### 3. Deploy Using AWS CLI

```bash
aws cloudformation deploy \
  --template-file cloudformation/template.yaml \
  --stack-name cfn-cloudfront-distribution-devl \
  --parameter-overrides \
    ProjectName=myproject \
    OriginBucketName=my-origin-bucket \
    OriginBucketRegion=us-east-1 \
    environment=devl \
  --region us-east-1
```

Get the site address from the stack outputs:

```bash
aws cloudformation describe-stacks \
  --stack-name cfn-cloudfront-distribution-devl \
  --query 'Stacks[0].Outputs[?OutputKey==`DistributionDomainName`].OutputValue' \
  --output text
```

## Naming Convention

The distribution `Name` tag and the OAC name are generated from the parameters:

```bash
{ProjectName}-{DistributionBaseName}-{environment}-{AWS::Region}
```

Example: `myproject-cdn-devl-us-east-1`

The OAC name appends `-oac` to this pattern.

## Requirements and Gotchas

- **Existing origin bucket:** The bucket must already exist. This template does not create it.
- **Index object:** Upload the default root object (for example `index.html`) to the bucket. Without it, requests return `AccessDenied`, because the bucket policy grants only `s3:GetObject`.
- **Customer managed KMS keys:** If the bucket encrypts objects with an SSE-KMS key, add a statement to that key's policy allowing `cloudfront.amazonaws.com` to perform `kms:Decrypt`, conditioned on the distribution ARN (`AWS:SourceArn`). Without it, requests return `AccessDenied`. The template cannot add this, because the key policy is outside the stack.
- **Bucket policy replacement:** The template creates an `AWS::S3::BucketPolicy` on the origin bucket. This replaces the bucket's entire existing policy, so any other statements are removed on deploy.
- **Origin endpoint:** The origin uses the S3 REST endpoint, which OAC requires. Do not switch it to the S3 website endpoint.

## Best Practices Implemented

- ✅ Origin kept private with Origin Access Control
- ✅ HTTPS-only viewer access
- ✅ Distribution naming and tagging for environment isolation

## License

MIT
