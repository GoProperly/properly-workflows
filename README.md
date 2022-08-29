# properly-workflows

A repository for template Github Action workflows for common tasks across Properly codebase.

## Usage
These workflows can be easily imported by any repository by pointing the `uses` parameter of the `job` to a workflow here. Example for `properties` service repository:
```
name: Test & Deploy

on: push

jobs:
  test_and_deploy:
    uses: GoProperly/properly-workflows/.github/workflows/test-and-deploy.yml@main
    with:
      python_version_file: properties/.python-version
      working_directory: properties
      run_datadog_ci: true
    secrets: inherit
```
