<div align="center">

# GitHub configuration

[![CGL](https://img.shields.io/github/actions/workflow/status/eliashaeussler/.github/cgl.yaml?label=cgl&logo=github)](https://github.com/eliashaeussler/.github/actions/workflows/cgl.yaml)

</div>

This repository contains my personal GitHub configuration for use in my
personal projects. It is not meant to be used anywhere else. I won't provide
support and don't accept pull requests for this repo.

## 🔈 GitHub Actions

### `eliashaeussler/.github/actions/github/check-pr`

This action checks if any open pull request exists with the current branch as
[head ref][1].

#### Reference

[`actions/github/check-pr/action.yml`](actions/github/check-pr/action.yml)

#### Example

```yaml
name: Renovate
on:
  push:
    branches:
      - 'renovate/**'

jobs:
  check-pr:
    runs-on: ubuntu-latest
    outputs:
      pr-exists: ${{ steps.check.outputs.pr-exists }}
    steps:
      - name: Check if PR exists
        id: check
        uses: eliashaeussler/.github/actions/github/check-pr@1.0.0

  trigger-cgl:
    needs: check-pr
    if: '!needs.check-pr.outputs.pr-exists'
    uses: ./.github/workflows/cgl.yaml

  trigger-tests:
    needs: check-pr
    if: '!needs.check-pr.outputs.pr-exists'
    uses: ./.github/workflows/tests.yaml
```

#### Inputs

| Name         | Required | Description                               | Default value              |
|--------------|----------|-------------------------------------------|----------------------------|
| `repository` | ✅        | GitHub repository to check for an open PR | `${{ github.repository }}` |
| `branch`     | ✅        | Branch name to check for an open PR       | `${{ github.ref_name }}`   |
| `token`      | ✅        | GitHub token used for authentication      | `${{ github.token }}`      |

#### Outputs

| Name        | Description                                |
|-------------|--------------------------------------------|
| `pr-exists` | Flag to indicate whether an open PR exists |
| `pr-url`    | URL to an open PR if any exists            |

## ⭐ License

This project is licensed under [GNU General Public License 3.0 (or later)](LICENSE).

[1]: https://docs.github.com/en/actions/learn-github-actions/contexts#github-context
