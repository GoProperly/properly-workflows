# properly-workflows

A repository for template Github Action workflows for common tasks across Properly codebase.

## Reusable Workflows
Our BE services all have fundamentally the same github actions workflows copy & pasted between repos. This makes it difficult to update the workflows because we have to make sure changes are replicated across dozens of repos.

This repository introduces DRY Github Action workflows that can be used across repositories.

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
      default_branch: master
      run_datadog_ci: true
    secrets: inherit
```

## Testing
New/existing workflows on different branches can be tested by pointing caller workflows to specific versions. This can be done by updating the `uses` parameter mentioned above. For example, to test the `test-and-deploy.yml` workflow on the `test-101` branch of this repository:
```
...

jobs:
  my_job_name:
    uses: GoProperly/properly-workflows/.github/workflows/test-and-deploy.yml@test-101

...
```
We can reference reusable workflow files using one of the following syntaxes:

1. For reusable workflows in public repositories:

```
{owner}/{repo}/.github/workflows/{filename}@{ref}
```
2. For reusable workflows in the same repository:
```
./.github/workflows/{filename}
```

`{ref}` can be any of:
- SHA
- a release tag, or
- a branch name

Using the commit SHA is the safest for stability and security. For more information, see ["Security hardening for GitHub Actions."](https://docs.github.com/en/actions/learn-github-actions/security-hardening-for-github-actions#reusing-third-party-workflows) If you use the second syntax option (without `{owner}/{repo} and @{ref}`) the called workflow is from the same commit as the caller workflow.