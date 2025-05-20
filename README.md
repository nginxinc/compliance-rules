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
    uses: nginxinc/compliance-rules/.github/workflows/mend.yml@main
    secrets: inherit
    with:
      product_name: ptd-demo2_${{ github.head_ref || github.ref_name }}
      project_name: ptd-demo2
```

- In the `mend` job reference the main mend workflow (in this repository)

```yaml
uses: nginxinc/compliance-rules/.github/workflows/mend.yml@main
```

- Configure `product_name` and `project_name` variables. They represent github repository name.

```yaml
product_name: <your_repo_name>_${{ github.head_ref || github.ref_name }}
project_name: <your_repo_name>
```

### Mend Workflow

1. GitHub triggers the mend workflow defined in a project repository (for example `ProjectABC`)
1. Mend job references mend rules (main `mend.yml`) defined in the workflow in this repository.
1. Mend scans the `ProjectABC` code and generates vulenerability report.
1. Depends on the scan (vulnerability) rules defined in the main `mand.yml` the pipeline fails or passes the scan.
1. The GitHub repository (`ProjectA`) must be configured to reject PRs (prevent from merging with the `main` branch) if the mend pipeline fails.  
