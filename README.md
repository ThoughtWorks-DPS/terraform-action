<div align="center">
	<p>
	<img alt="Thoughtworks Logo" src="https://raw.githubusercontent.com/ThoughtWorks-DPS/static/master/thoughtworks_flamingo_wave.png?sanitize=true" width=200 /><br />
	<img alt="DPS Title" src="https://raw.githubusercontent.com/ThoughtWorks-DPS/static/master/EMPCPlatformStarterKitsImage.png?sanitize=true" width=350/><br />
	<h2>terraform-action</h2>
	<img alt="GitHub Actions Workflow Status" src="https://img.shields.io/github/actions/workflow/status/ThoughtWorks-DPS/terraform-action/.github%2Fworkflows%2Fdevelopment-build.yaml"> <img alt="GitHub Release" src="https://img.shields.io/github/v/release/ThoughtWorks-DPS/terraform-action"> <a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/license-MIT-blue.svg"></a>
	</p>
</div>

Action for Terraform pipelines.  _in progress_  

## Usage

### example development-build workflow, on git push
```yaml
# yamllint disable rule:line-length
# yamllint disable rule:truthy
---
name: development build

on:
  push:
    branches:
      - "*"
    tags:
      - "!*"

jobs:

  static-code-analysis:
    name: static code analysis
    uses: ThoughtWorks-DPS/terraform-action/.github/workflows/static-code-analysis.yaml@main
    with:
      tflint-scan: true
      tflint-provider: aws
      tflint-provider-version: 0.31.0
      trivy-scan: true

  plan:
    name: terraform plan
    uses: ThoughtWorks-DPS/terraform-action/.github/workflows/plan.yaml@main
    secrets:
      OP_SERVICE_ACCOUNT_TOKEN: ${{ secrets.OP_SERVICE_ACCOUNT_TOKEN }}
    with:
      workspace: sbx-i01-aws-us-east-1
      before-init: nonprod
```

### example apply workflow, on git tag
```yaml
# yamllint disable rule:line-length
# yamllint disable rule:truthy
---
name: development build

on:
  push:
    branches:
      - "!*"
    tags:
      - "*"

jobs:

  plan:
    name: terraform apply
    uses: ThoughtWorks-DPS/terraform-action/.github/workflows/apply.yaml@main
    secrets:
      OP_SERVICE_ACCOUNT_TOKEN: ${{ secrets.OP_SERVICE_ACCOUNT_TOKEN }}
    with:
      workspace: sbx-i01-aws-us-east-1
      before-init: nonprod
```

### Development

**TODO**  

1. local action development-build needs integration test workflow
2. versioned release with internal pin to release commit
