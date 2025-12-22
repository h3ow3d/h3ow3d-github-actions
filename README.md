# h3ow3d GitHub Actions

Reusable GitHub Actions workflows and composite actions for h3ow3d infrastructure repositories.

## Available Workflows

### Terraform Module CI
**Location:** `.github/workflows/terraform-module-ci.yml`

Complete CI pipeline for Terraform modules including formatting, linting, security scanning, documentation generation, and example testing.

**Usage:**
```yaml
name: Terraform CI
on: [push, pull_request]

jobs:
  ci:
    uses: h3ow3d/h3ow3d-github-actions/.github/workflows/terraform-module-ci.yml@main
    permissions:
      contents: write
      pull-requests: write
```

**Inputs:**
- `terraform_version` (default: `1.14.1`) - Terraform version
- `tflint_version` (default: `v0.50.0`) - TFLint version
- `working_directory` (default: `.`) - Working directory
- `enable_security_scans` (default: `true`) - Enable tfsec and Checkov
- `enable_docs` (default: `true`) - Enable auto-documentation
- `test_examples` (default: `true`) - Test example configurations

### Terraform Module Release
**Location:** `.github/workflows/terraform-module-release.yml`

Automated semantic versioning and release creation for Terraform modules.

**Usage:**
```yaml
name: Release
on:
  push:
    branches: [main]

jobs:
  release:
    uses: h3ow3d/h3ow3d-github-actions/.github/workflows/terraform-module-release.yml@main
    permissions:
      contents: write
    with:
      module_type: cognito
```

**Inputs:**
- `terraform_version` (default: `1.14.1`) - Terraform version
- `enable_breaking_change_detection` (default: `true`) - Detect breaking changes
- `module_type` (default: `module`) - Module name for release notes

**Release Triggers:**
- Commit message must contain `[release]` to trigger a release
- Breaking changes detected with `!` or `BREAKING CHANGE` in commit message
- Automatic semantic versioning (major for breaking, patch for regular)

## Available Composite Actions

### Node.js Build and Test
**Location:** `actions/node-build-test/action.yml`

Composite action for Node.js projects: setup, install, test, type-check, and build.

**Usage:**
```yaml
- name: Build and test
  uses: h3ow3d/h3ow3d-github-actions/actions/node-build-test@main
  with:
    node-version: '20'
    working-directory: './apps/api'
    type-check-command: 'npm run type-check'
    build-command: 'npm run build'
    upload-artifacts: 'true'
```

**Inputs:**
- `node-version` (default: `20`) - Node.js version
- `working-directory` (default: `.`) - Project directory
- `install-command` (default: `npm ci`) - Install command
- `test-command` (default: `''`) - Test command (empty to skip)
- `type-check-command` (default: `''`) - Type check command (empty to skip)
- `build-command` (default: `npm run build`) - Build command
- `upload-artifacts` (default: `false`) - Upload build artifacts
- `artifact-name` (default: `dist`) - Artifact name
- `artifact-path` (default: `dist/`) - Artifact path

### Terraform Deploy
**Location:** `actions/terraform-deploy/action.yml`

Composite action to initialize, plan, and apply Terraform configurations.

**Usage:**
```yaml
- name: Deploy with Terraform
  uses: h3ow3d/h3ow3d-github-actions/actions/terraform-deploy@main
  with:
    terraform-version: '1.5.0'
    working-directory: 'infra/terraform'
    auto-approve: 'true'
```

**Inputs:**
- `terraform-version` (default: `1.5.0`) - Terraform version
- `working-directory` (default: `infra/terraform`) - Terraform directory
- `terraform-wrapper` (default: `true`) - Enable Terraform wrapper
- `format-check` (default: `true`) - Run fmt check
- `auto-approve` (default: `true`) - Auto-approve apply
- `environment` (default: `dev`) - Environment name

**Outputs:**
- `plan-exitcode` - Terraform plan exit code

### AWS Configure
**Location:** `actions/aws-configure/action.yml`

Configure AWS credentials using OIDC or static keys.

**Usage (OIDC):**
```yaml
- name: Configure AWS
  uses: h3ow3d/h3ow3d-github-actions/actions/aws-configure@main
  with:
    aws-region: 'us-east-1'
    auth-method: 'oidc'
    role-to-assume: ${{ secrets.AWS_DEPLOY_ROLE_ARN }}
```

**Usage (Static Keys):**
```yaml
- name: Configure AWS
  uses: h3ow3d/h3ow3d-github-actions/actions/aws-configure@main
  with:
    aws-region: 'eu-west-2'
    auth-method: 'static'
    aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
    aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
```

**Inputs:**
- `aws-region` (required) - AWS region
- `auth-method` (default: `oidc`) - Authentication method: `oidc` or `static`
- `role-to-assume` - IAM role ARN (for OIDC)
- `role-session-name` (default: `GitHubActions`) - Role session name
- `aws-access-key-id` - AWS access key ID (for static)
- `aws-secret-access-key` - AWS secret access key (for static)

## Workflow Structure

```
.github/
├── workflows/
│   ├── terraform-module-ci.yml      # CI pipeline for modules
│   └── terraform-module-release.yml # Release automation
└── actions/
    ├── node-build-test/             # Node.js build and test
    ├── terraform-deploy/            # Terraform deployment
    └── aws-configure/               # AWS credentials setup
```

## Example: Complete Module Repository Setup

```yaml
# .github/workflows/ci.yml
name: CI
on: [push, pull_request]

jobs:
  test:
    uses: h3ow3d/h3ow3d-github-actions/.github/workflows/terraform-module-ci.yml@main
    permissions:
      contents: write
      pull-requests: write

  release:
    needs: test
    if: github.ref == 'refs/heads/main'
    uses: h3ow3d/h3ow3d-github-actions/.github/workflows/terraform-module-release.yml@main
    permissions:
      contents: write
    with:
      module_type: networking
```

## Version Pinning

### Using `@main` (Latest)
```yaml
uses: h3ow3d/h3ow3d-github-actions/.github/workflows/terraform-module-ci.yml@main
```
Always uses the latest version. Good for active development.

### Using `@v1.0.0` (Stable)
```yaml
uses: h3ow3d/h3ow3d-github-actions/.github/workflows/terraform-module-ci.yml@v1.0.0
```
Pins to a specific version. Recommended for production.

## Features

### ✅ Included in CI Pipeline
- **Format Check**: `terraform fmt -check -recursive`
- **Syntax Check**: `terraform init -backend=false`
- **Linting**: TFLint with AWS plugin
- **Security**: tfsec and Checkov scanning
- **Documentation**: Auto-generate with terraform-docs
- **Examples**: Validate basic and complete examples

### 🚀 Release Automation
- **Semantic Versioning**: Automatic version bumping
- **Breaking Changes**: Detects and creates major versions
- **Release Notes**: Auto-generated from commit messages
- **Git Tags**: Automatically created and pushed

## Contributing

When updating workflows:
1. Test changes in a separate branch
2. Version workflows with git tags
3. Update this README with any new inputs/outputs
4. Notify teams using these workflows

## Maintenance

### Updating Shared Workflows
1. Make changes in this repository
2. Test with a module repository
3. Create a release tag (e.g., `v1.1.0`)
4. Update consuming repositories to use new version

### Adding New Workflows
1. Create workflow in `.github/workflows/`
2. Use `workflow_call` trigger
3. Document inputs and usage
4. Update this README
