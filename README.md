# Create or update PR Action

A simple but powerful GitHub Action to link workflows and manage dependencies between projects.


## Usage: 

To learn accurately how to use it, you must see the [guide here](/examples)

## Quick Example:

```yaml
      # Call `Create or update PR` GitHub action
      - name: create_pr
        uses: lenra-io/create-or-update-pr-action@main
        with:
          # The name of the PR to be created.
          name: 'Update dependencies'
          # The token used to create the PR. 
          # I didn't use the `secrets.GITHUB_TOKEN` here because this token can't trigger workflow event if we push something or create a PR.
          token: ${{ secrets.WORKFLOW_GITHUB_TOKEN }}
          # The message to comment on PR after creation (Can be Markdown)
          message: |
            # Upgrade ${{ github.event.inputs.origin }} to ${{ github.event.inputs.version }}
          # Write a little script called just before the PR creation or update.
          # This example will upgrade the version inside of the pubspec.yaml and publish it in a new PR
          script: |
            # Update the requested dependency version in the pubspec.yaml file
            yq eval ".dependencies.${{ github.event.inputs.origin }}.git.ref = \"${{  github.event.inputs.version }}\"" -i pubspec.yaml
            
            # Commit the file to include it to the PR. Don't use `git push`, it's automatic so you don't have to do it yourself.
            git add pubspec.yaml
            git commit -m "build(deps-dev): Upgrade ${{ github.event.inputs.origin }} to ${{ github.event.inputs.version }}"
```

If you need more informations, you can look at this [dev.to post](https://dev.to/lenradevelopers/lenras-automatic-management-of-dependencies-i28)
