# properly-workflows

A repository for template Github Action workflows for common tasks across Properly codebase.

## Reusable Workflows
Our BE services all have fundamentally the same github actions workflows copy & pasted between repos. This makes it difficult to update the workflows because we have to make sure changes are replicated across dozens of repos.

This repository introduces DRY Github Action workflows that can be used across repositories.

## Tags
From time to time there will be some breaking changes introduced to `properly-workflows`. To make the task of upgrading/ downgrading easier this repo makes use of git tags. This repo uses [annotated tags](https://git-scm.com/book/en/v2/Git-Basics-Tagging#_annotated_tags), which typically include a message indicating why the tag change occurred.

It is recommended to create a minor tag when adding any new functionality, but is required to add a major tag when a breaking change is added, as per [semantic versioning](https://semver.org/).
A tag can be attached to the branch being worked on, and once merged will apply to the merge in `main` branch.
To add a tag on branch (e.g. after changes have been merged to `main`):
```
git tag -a v1.0 -m "added a breaking change that does X and Y"
```
If the tag has already been associated with a different commit and you would like to move it to a new commit (force), use the following:
```
git tag -fa v1.0 -m "added a breaking change that does X and Y"
```
> The above command will  force the tag `v1.0` to be moved to a new commit. You will need to manually delete the tag from github (must have admin permissions to do so)
> You cannot force push over an existing tag in github

Tags are not pushed to `origin` by default so when pushing ensure to include `--tags`:
```
git push origin HEAD --tags
```


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
