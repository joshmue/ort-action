# Execute ORT with a Github Action

This action allows you to run [ORT](https://oss-review-toolkit.org/). The OSS
Review Toolkit (ORT) aims to assist with the tasks that commonly need to be
performed in the context of license compliance checks, especially for (but not
limited to) Free and Open Source Software dependencies.

<!-- action-docs-description -->
## Description
Run oss-review-toolkit to review Open Source software dependencies.
<!-- action-docs-description -->

<!-- action-docs-inputs -->
## Inputs
| parameter | description | required | default |
| - | - | - | - |
| script | script to run | `true` | `None` |
<!-- action-docs-inputs -->

<!-- action-docs-outputs -->
## Outputs
| parameter | description |
| - | - |
<!-- action-docs-outputs -->

<!-- action-docs-runs -->
## Runs
This action is an `docker` action.
<!-- action-docs-runs -->
## GitHub workflow

```yml
name: ORT

on:
  workflow_dispatch:

jobs:
  ort-scan:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - uses: actions/cache@v3
      with:
        path: data_dir
        key: data
    - name: 'Licensing: Analyse, Evaluate, Report'
      id: ort-action
      uses: joshmue/ort-action@container_action
      with:
        script: |
          set -ex
          ort analyze -i . -o $PWD/result
          ort evaluate -i $PWD/result/analyzer-result.yml -o $PWD/result --rules-file test.rules.kts || echo "Detected issues"
          ort report -i $PWD/result/evaluation-result.yml -o $PWD/result -f StaticHtml
    - name: Upload Results
      uses: actions/upload-artifact@v3
      if: always()
      with:
        name: results
        path: result
```
