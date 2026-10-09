## [1.2.1](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/compare/v1.2.0...v1.2.1) (2026-09-15)


### Bug Fixes

* update setup-environments workflow to use version 1.3.0 ([472381b](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/472381b98cbf78af034e827d7000bf8aa06f39c8))

# [1.2.0](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/compare/v1.1.0...v1.2.0) (2026-09-14)


### Features

* add environments configuration and update .gitignore ([ab25757](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/ab25757cbfad78875001d27e06b35fbefbffcffe))

# [1.1.0](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/compare/v1.0.0...v1.1.0) (2026-09-14)


### Features

* add setup AWS environments workflow ([7152f9d](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/7152f9d0fb16baa2c961db2f1f9723eaee0b1c25))

# 1.0.0 (2026-09-10)


### Bug Fixes

* add bug branch to CI workflow triggers ([0a54d34](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/0a54d3426a478444ad768e4dc48567aa59b74080))
* add CFN_TEMPLATES_S3_BUCKET environment variable and restore deploy job in CI workflow ([aa71951](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/aa719517396f4f800ab518b4d97fc5c8499b2d96))
* add description for CloudFormation S3 Bucket Stack deployment ([41b4094](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/41b4094937690e5f5dfd7ab4bd3b16df0904b477))
* add ENVIRONMENT variable to CI workflow ([8aa2862](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/8aa2862ee07a78f1b331f2d39af87a4eeb902dc2))
* add permissions for actions and id-token in deploy job ([3e46edb](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/3e46edb678652413b8c202d7311a37ba51c4ea4d))
* add read permission for contents in deploy job ([e0e60e6](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/e0e60e6ee12c05a9ebb0fa872c72cf4565c00101))
* allow dependabot bot in claude-code-review workflow ([fcff5dc](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/fcff5dc6c5395ba9583f5253eb9d2d50800178b1))
* allow dependabot in claude code review workflow ([7aad9a4](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/7aad9a492ef0f7e009041e81d3f6bf362c86ec26))
* clean up environment variable references and remove unused outputs in CI workflow ([afed754](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/afed754271a54ae5e0c14cab44c6ea3d58976d9e))
* comment out deploy job in CI workflow ([7559efa](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/7559efa0396a1ea7ecac9dfb4ffbc90b877a829f))
* comment out job name for S3 bucket deployment ([a04d7c3](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/a04d7c3c95dbff19918ef8f9c942a25ba3ad1069))
* consolidate permissions in CI workflow ([f833b70](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/f833b7023b563e25716a2597cdc4d3832b765de5))
* correct cfn-templates-s3-bucket output reference in CI workflow ([de57d0f](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/de57d0f30880888d00be319fafefb13c2361332a))
* correct GitHub organization in package.json repository URLs ([00d73c8](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/00d73c8a67a122cde06a1d6374a6664fcdeaa4a4))
* correct syntax for CFN_TEMPLATES_S3_BUCKET environment variable in CI workflow ([3d0a376](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/3d0a376c15cf9eb5140afb9938f1d2d1eca2cc9b))
* correct variable references in CI workflow for AWS_REGION and AWS_ACCOUNT_ID ([a6a4407](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/a6a44071938fb7973d446e6c93108ac63e7b4680))
* remove environment input from workflow dispatch and set default environment to 'ci' ([a67b449](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/a67b4494a8984548c51252da84efd970c61f7825))
* rename job from config to get-config and update dependencies in CI workflow ([536f413](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/536f413119ac2901a502e9eedab72ab14d754ed4))
* rename job from get-config to config and update dependencies in CI workflow ([ccc7f2d](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/ccc7f2d402074d68f68ef6020ec46ece3f7af641))
* rename job to Get Env Config in CI workflow ([79d1f8e](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/79d1f8e21c36c0e550f647515e72d39177961e3d))
* skip Claude review when auth secrets are unavailable ([19b524f](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/19b524f69f3a2af27a988ca5e06b1f985f554704))
* update cfn-deploy workflow reference in CI configuration ([4203480](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/4203480c9573079d0a59e224b9f219b2d88f745f))
* update cfn-deploy workflow reference in deploy job ([84749dd](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/84749ddef53bf1aa11918d3daade5091197562df))
* update cfn-deploy workflow reference to version 1.1.0 ([cb13548](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/cb13548c143c000f8e30f58cb8ef41ef35ed9830))
* update CI workflow to include environment outputs and deploy step ([671c946](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/671c946851bc80c33eb5bf6356728fce8b50fcb6))
* update environment from ci to devl in CI workflow ([3f6ed7a](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/3f6ed7a2b38ae89fdf0863ae378683728e6d3094))
* update job dependencies in CI workflow to use correct outputs from config ([b6ed4ab](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/b6ed4ab3671b9195473059f03e2b47b26733431f))
* update job name for S3 bucket deployment in CI workflow ([da3f5c3](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/da3f5c39cf28339e5dfae78bde384b66414ba7cb))
* update parameters-file path in CI workflow ([2ccbaaa](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/2ccbaaac0c0647e53386c9ade4fd94f6c0591388))
* update push paths in CI workflow and add debug output for DEPLOY_ENV ([16cc189](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/16cc189780653060cea7cb97c65fad93c0b56cba))


### Features

* add debug output for environment configuration variables in CI workflow ([1e033e7](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/1e033e741c673debc3e10b667028c0e8415462eb))
* add workflow_dispatch trigger to CloudFormation deployment workflow and remove unused reusable workflow ([78b7c3c](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/78b7c3cb488a62ec19681465232a99a6ef9be352))
* comment out outputs and deploy job in CI workflow for environment configuration ([e6230a3](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/e6230a374d47f4a5d08b33e6ec0a1221f84e010d))
* initialize CloudFormation template repository with semantic release setup ([1cad712](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/1cad712fac569f49b77fcd7f2d3f6545e4b0a9fe))
* refactor CI workflow to use environment variables for AWS configurations and stack names ([e7d8eab](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/e7d8eab94396849428b43070d65ce66ead2b8718))
* remove environment declaration from get-config job in CI workflow ([66deae4](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/66deae4b51d8c8e6b2a82fc7081ac81378eb522d))
* remove unused environment variables from CI workflow ([779aab0](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/779aab00880b8be21479bcc1dc8f87303e6690ed))
* simplify CI workflow by removing unused steps and consolidating environment variable handling ([80995bd](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/80995bde744fa6e2753311f71d174b021c549a09))
* update AWS region configuration to use default value if not set ([2393de7](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/2393de74805bfc1cbe809b01ac31fd7af509025d))
* update AWS_ACCOUNT_ID to use vars instead of secrets in CI workflow ([396aeec](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/396aeec084bce1bd3d3b82d437cd8d7bd7f2d2aa))
* update CI workflow to deploy CloudFormation stack and streamline configuration steps ([5a978b8](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/5a978b83ae378dee99b27e254736467ed04c5eee))
* update CI workflow to use dynamic deployment environment input ([151678e](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/151678e2fb635ef9fe6e78c2e79a8917ece26fb1))
* update get-config job to use environment variables for configuration outputs ([f4832c7](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/f4832c7e46fed3e1d25be364e472f18b926cf0d8))
* update GitHub Actions to use latest checkout action and configure AWS credentials via OIDC ([f8df8c9](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/f8df8c9775f1249b0598258790296d576ea18d8e))
* update GitHub token requirement in workflow to be optional ([45c3705](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/45c370592568d89281ac3923e65bfc5d164a3ed5))
* update OIDC role name handling in CI workflow to use outputs from get-config job ([daf49c0](https://github.com/subhamay-bhattacharyya-cfn/cloudformation-template/commit/daf49c0701f19af9ec83c914fcff16104f570f39))

## [1.1.2](https://github.com/subhamay-bhattacharyya-gha/github-action-template/compare/v1.1.1...v1.1.2) (2025-05-21)


### Bug Fixes

* update devcontainer configuration to include Node.js feature ([a6d9d47](https://github.com/subhamay-bhattacharyya-gha/github-action-template/commit/a6d9d478096bf3f7a94f5fd44e26c3deb6e2611c))

## [1.1.1](https://github.com/subhamay-bhattacharyya-gha/github-action-template/compare/v1.1.0...v1.1.1) (2025-05-19)


### Bug Fixes

* add missing permissions for issue assignment workflow ([6c1c99c](https://github.com/subhamay-bhattacharyya-gha/github-action-template/commit/6c1c99cb15f3df2cda6f7e8ea385447d43011bd7))

# [1.1.0](https://github.com/subhamay-bhattacharyya-gha/github-action-template/compare/v1.0.0...v1.1.0) (2025-05-19)


### Features

* add workflow to auto create branches on issue assignment ([c598e00](https://github.com/subhamay-bhattacharyya-gha/github-action-template/commit/c598e002938006d48354017d1131d1f43e378393))

# 1.0.0 (2025-05-18)


### Bug Fixes

* update release configuration path in package.json ([d94b611](https://github.com/subhamay-bhattacharyya-gha/github-action-template/commit/d94b61152ef216a98f5303e2ed2d78dbe309dd0e))


### Features

* add plugins for analyzing commits, generating release notes, preparing release, publishing, and verifying conditions ([60bfe70](https://github.com/subhamay-bhattacharyya-gha/github-action-template/commit/60bfe70c3415559965a970971676f25a960d884f))
* add release configuration for semantic release ([bc81687](https://github.com/subhamay-bhattacharyya-gha/github-action-template/commit/bc81687811d3ba20fb35a813afd29d222b37dbe0))
* initialize semantic release setup with custom plugins ([3a795e3](https://github.com/subhamay-bhattacharyya-gha/github-action-template/commit/3a795e3f38397cceb825de4380c5d88907a3b744))
* migrate release configuration from release.config.js to .releaserc.json ([1351e55](https://github.com/subhamay-bhattacharyya-gha/github-action-template/commit/1351e55fedf58fa9fb9bba217eecf1dba18c5a5c))
* reorder badges in README for better visibility ([f594ac6](https://github.com/subhamay-bhattacharyya-gha/github-action-template/commit/f594ac67198dddcb460c3e8f14b06ecc05dc7c36))
* update actions/checkout and actions/setup-node to v4 ([ea8ec0d](https://github.com/subhamay-bhattacharyya-gha/github-action-template/commit/ea8ec0d94afac30900f7d7229330ad4e6cc00a3d))

# Changelog

All notable changes to this project will be documented in this file.
