# Examples

Here you will find some example repositories linked by dependencies defined in a YAML file at the root of each repository.

## How it work ?

### 1. Define the shape of your projects

We will start by presenting our Git projects in the following form for some evident purposes of dependencies:

```
my-library -> my-core-code, my-dev-app
my-core-code -> my-dev-app
```

As you can see, we have 3 Git repositories that depend on each other. In our example, repository "my-core-code" needs repository "my-library" to compile. And as it is quite a pain to always have to manually edit the right dependency files, this action will take care of it if it is well configured.
If we continue our exploration, the "my-dev-app" repository also requires the "my-library" repository in addition to the "my-core-code" repository. Projects of a certain size can have pretty complex mesh links which can be a source of errors.

### 2. Understanding links between each workflows

When you publish a new release on the repository "my-library", this action will automatically create a Pull Request on the repository "my-core-code" and "my-dev-app" to update the correct files and change it's own link in dependancy file(s).
And now, when you merge the PR on the repository "my-core-code", and create a new release, it will update the PR on the repository "my-dev-app" to add this changes.

Now we can accept changes on the repository "my-dev-app" to update it's version of "my-library" and "my-core-code".

### 3. Go deeper for an even better workflow

This is be awesome to use in your projects with sementic-release to make releases even faster and erase error of dependency managments

Now you can test it by publishing theses 3 repository on your own account/organization to see how it work. (Don't forget to create the WORKFLOW_GITHUB_TOKEN secret in the settings of each repository)
