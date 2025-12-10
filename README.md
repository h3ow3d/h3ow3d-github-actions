# h3ow3d GitHub Actions

Reusable GitHub Actions workflows for h3ow3d infrastructure repositories.

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

## Workflow Structure

```
.github/
└── workflows/
    ├── terraform-module-ci.yml      # CI pipeline for modules
    └── terraform-module-release.yml # Release automation
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
