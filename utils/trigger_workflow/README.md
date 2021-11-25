# Trigger Workflow Action

This action simplify the usage of the GitHub API to allow you to trigger a workflow inside another repository with ease.

## Example 

Here how you can use it:

```yaml
# file -> .github/workflows/on_release.yml
name: Populate Release

on:
  release:
    types: [published]

jobs:
  populate_release:
    name: Populate Release
    # You must use a linux distro because of GitHub's composite action limitation.
    runs-on: ubuntu-latest
    timeout-minutes: 10
    steps:
      - name: create_pr
        uses: lenra-io/create-or-update-pr-action/utils/trigger_workflow@v1
        with:
          target_ref: beta
          target_repository: "${{ github.repository_owner }}/another-repo,${{ github.repository_owner }}/one-more-repo"
          target_workflow: create_or_update_pr.yml
          params: |
            {
              "origin": "original_repo"
            }
          token: ${{ secrets.WORKFLOW_GITHUB_TOKEN }}
```
