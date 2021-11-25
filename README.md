# Create or update PR Action

A simple but powerful GitHub Action to link workflows and manage dependencies between projects.


## Usage: 

To learn accurately how to use it, you must see the [guide here](/examples)

## Quick Example:

```yaml
# file -> .github/workflows/create_or_update_pr.yml
name: Create or Update PR

on:
  workflow_dispatch:

jobs:
  create_or_update_pr:
    name: Create or Update PR
    runs-on: ubuntu-latest
    timeout-minutes: 10
    steps:
      # Clone the repository
      - name: Checkout
        uses: actions/checkout@v2
      # Install the yq command that allow to edit yaml files
      - name: Setup yq
        id: setup-yq
        uses: shiipou/setup-yq-action@v2.0.1
      # Call `Create or update PR` GitHub action
      - name: create_pr
        uses: lenra-io/create-or-update-pr-action@v1
        with:
          # The name of the PR to be created.
          name: 'Update dependecies'
          # The token used to create the PR. 
          # I didn't use the `secrets.GITHUB_TOKEN` here because this token can't trigger workflow event if we push something or create a PR.
          token: ${{ secrets.WORKFLOW_GITHUB_TOKEN }}
          # Write a little script called just before the PR creation or update.
          script: |
            # Update the requested dependency version in the pubspec.yaml file
            ${{ steps.setup-yq.outputs.yq-binary }} eval ".dependencies.${{ github.event.inputs.origin }}.git.ref = \"${{  github.event.inputs.version }}\"" -i pubspec.yaml
            
            # Commit the file to include it to the PR. Don't use `git push`, it's automatic so you don't have to do it yourself.
            git add pubspec.yaml
            git commit -m "Upgrade ${{ github.event.inputs.origin }} to ${{ github.event.inputs.version }}"
```
