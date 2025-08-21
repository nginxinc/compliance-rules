# Compliance-rules

Compliance repo with main rules for SCA and SAST scan

This repository provides common GitHub workflows for Mend and CodeQL scans.

## Configure Mand workflow in a GitHub repository

- Create `mend.yml` workflow in your project repository

```yaml
name: Mend

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main
  workflow_dispatch:
    inputs:
      branch:
        type: string
        required: false
        default: main
  workflow_call:
    inputs:
      branch:
        type: string
        required: true

permissions:
  contents: read

jobs:
  mend:
    uses: nginxinc/compliance-rules/.github/workflows/mend.yml@<git_tag>
    secrets: inherit
    with:
      product_name: <caller_product_name>_${{ github.head_ref || github.ref_name }}
      project_name: <caller_project_name>
```

- In the `mend` job reference the main mend workflow (in this repository)

```yaml
uses: nginxinc/compliance-rules/.github/workflows/mend.yml@<git_tag>
```

- Configure `product_name` and `project_name` variables. They represent caller github repository `product` and `project` name.

```yaml
product_name: <caller_product_name>_${{ github.head_ref || github.ref_name }}
project_name: <caller_project_name>
```

### Mend workflow

1. GitHub triggers the mend workflow defined in a project repository (for example `ProjectABC`)
1. Mend job references mend rules (main `mend.yml`) defined in the workflow in this repository.
1. Mend scans the `ProjectABC` code and generates vulenerability report.
1. Depends on the scan (vulnerability) rules defined in the main `mand.yml` the pipeline fails or passes the scan.
1. The GitHub repository (`ProjectA`) must be configured to reject PRs (prevent from merging with the `main` branch) if the mend pipeline fails.  

## Configure CodeQL workflow in a GitHub repository

- Create `codeql.yml` workflow in your project repository

```yaml
name: "CodeQL"

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main
  workflow_dispatch:
    inputs:
      branch:
        type: string
        required: false
        default: main
  workflow_call:
    inputs:
      branch:
        type: string
        required: true

concurrency:
  group: ${{ github.ref_name }}-codeql
  cancel-in-progress: true

permissions:
  contents: read

jobs:
  codeql:
    uses: nginxinc/compliance-rules/.github/workflows/codeql.yml@<git_tag>
    with:
      requested_languages: go
```

- In the `codeql` job reference the main `codeql` workflow (in this repository)

```yaml
uses: nginxinc/compliance-rules/.github/workflows/codeql.yml@<git_tag>
```

### CodeQL workflow

1. GitHub triggers the CodeQL workflow defined in a project repository (for example `ProjectABC`)
1. CodeQL job references `codeql` rules (main `codeql.yml`) defined in the workflow in this repository.
1. CodeQL analyses the `ProjectABC` code.
1. Depends on the results the pipeline fails or passes.
1. The GitHub repository (`ProjectA`) must be configured to reject PRs (prevent from merging with the `main` branch) if the codeql detects issues and the pipeline fails.  

## Configure Assertion Document Workflow

In your project release workflow add a step (workflow) for generating the Assertion Document.

Example:

```yaml
```

In the release job in the assertion step reference the main `assertion` workflow (in this repository)

```yaml
uses: nginxinc/complience-rules/.github/workflows/assertion-reusable.yml@<git_tag>
```
