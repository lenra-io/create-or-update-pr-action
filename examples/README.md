# Examples

Here you will find some example repositories linked by dependencies defined in a YAML file at the root of each repository.

## How it work ?

### 1. Define the shape of your projects

We will start by presenting our Git projects in the following form for some evident purposes of dependencies:

```
Repo-A -> Repo-B, Repo-C
Repo-B -> Repo-C
```

As you can see, we have 3 Git repositories that depend on each other. In our example, repository "B" needs repository "A" to compile. And as it is quite a pain to always have to manually edit the right dependency files, this action will take care of it if it is well configured.
If we continue our exploration, the "C" repository also requires the "A" repository in addition to the "B" repository. Projects of a certain size can have pretty complex mesh links which can be a source of errors.

When you publish a new release on the repository "A", this action will automatically create a Pull Request on the repository "B" and "C" to update the correct files and change it's own link in dependancy file(s).
And now, when you merge the PR on the repository "B", and create a new release, it will update the PR on the repository "C" to add this changes.

Now we can accept changes on the repository "C" to update it's version of "A" and "B".

This can be awesome to use with sementic-release to make it even faster.

Now you can test it by publishing theses 3 repository on your own account/organization to see how it work. (Don't forget to create the WORKFLOW_GITHUB_TOKEN secret in the settings of each repository)
