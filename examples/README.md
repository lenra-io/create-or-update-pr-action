# Examples

Here you will find some example repositories linked by dependencies defined in a YAML file at the root of each repository.

## How it work ?

### 1. Define the shape of your projects

We will start by presenting our Git projects in the following form for some evident purposes of dependencies:

```
protocol-lib: string-lib
core-code: string-lib, protocol-lib
desktop-app: core-code
mobile-app: core-code
web-app: core-code
```

As you can see, we have a total of 6 Git repositories that depend on each other. In our example, repository "core-code" that will contain the main code of the app that will be shared on each platform (desktop-app, mobile-app, and web-app). That core-code needs the repositories "string-lib" and "protocol-lib" to compile. And as it is quite a pain to always have to manually edit the right dependency files, this action will take care of it if it's well configured.
If we continue our exploration, the "protocol-lib" repository also requires the "string-lib" repository.
Projects of a certain size can have pretty complex mesh links which can be a source of errors.

### 2. Understanding links between each workflows

When you publish a new release on the repository "string-lib", this action will automatically create a Pull Request on the repository "protocol-lib" and "core-code" to update the correct files and change its own link in dependency file(s).
And now, when you merge the PR on the repository "protocol-lib", and create a new release, it will update the previously created PR on the repository "core-code" to add this changes if it's not already merged.

Now we can accept changes on the repository "core-code" to update its version of "string-lib" and "protocol-lib".
This will batch upgrade at once the version in "desktop-app", "mobile-app", and "web-app".

### 3. Go deeper for an even better workflow

This is be awesome to use in your projects with "sementic-release" to make releases even faster and erase error of dependency managments

Now you can test it by publishing theses 3 repository on your own account/organization to see how it work. (Don't forget to create the WORKFLOW_GITHUB_TOKEN secret in the settings of each repository)
