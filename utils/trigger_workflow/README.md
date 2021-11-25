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
        # Import the Action in your workflow
        uses: lenra-io/create-or-update-pr-action/utils/trigger_workflow@v1
        with:
          # Choose the branch where you want to run your remote workflow
          target_ref: beta
          # Specify the remote repositories the action will be triggered
          target_repository: "${{ github.repository_owner }}/another-repo,${{ github.repository_owner }}/one-more-repo"
          # Give the filename of the workflow to execute inside of the remote repositories
          target_workflow: create_or_update_pr.yml
          # You can specify params passed to the remote repositories at runtime
          params: |
            {
              "origin": "original_repo"
            }
          # Specify the GitHub token that will trigger the workflow. 
          # That use the user of that token, so If you use `secret.GITHUB_ACTION` the remote workflow can't trigger any other events (like on: push, on: release, etc)
          token: ${{ secrets.WORKFLOW_GITHUB_TOKEN }}
```
