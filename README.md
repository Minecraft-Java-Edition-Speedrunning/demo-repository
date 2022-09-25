
# Official "Minecraft: Java Edition" speedrunning mod repository guide

This repository is here to server as a guide project on how to setup a repository for release and distribution of speedrunning mods. Please read this document in its entirety before setting up a new project.

## Repository naming

The naming of repository is to serve as a method of differentiating mods by both mod identify and supported versions of the game.
Mod repositories names should match the following regex.

`/mcsr-(?<mod_id>[\w-]*)-(?<min_version>\d+\.\d+(?:\.\d+)?)(:?-(?<max_version>\d+\.\d+(?:\.\d+)?))?/`

**examples:**
* `mcsr-sodium-1.16.1`
* `mcsr-voyager-1.14-1.16.5`

Parts of the regex:

`mcsr-` - all mod repositories should be prefixed with `"mcsr-"` this denotes that the repository is a mod and not some other project (for example this repository)

`(?<mod_id>[\w-]*)` - the second part of the string is a unique identifier for the mod. This identifier should be consistent across all version splits for the same mod.

`(?<min_version>\d+\.\d+(?:\.\d+)?)` - the third part of the string is the minimum (or only) version of minecraft that this repository supports.

`(:?-(?<max_version>\d+\.\d+(?:\.\d+)?))?` - the fourth part of the string is an optional maximum version of minecraft that this repository supports if a repository works across multiple versions of minecraft.

## Pull requests, branches and protection

All repositories should have a `main` branch that should at all times reflect a stable release candidate of the mod.    
This main branch should have some branch protection rules on it to protect it and keep it stable.

### Pull requests
You should set up a pull request message that is displayed whenever someone attempts to open up a pull request to help with the review process. This message should be place in `/docs/pull_request_template.md` An example message can be found in this repository at that path.
[more info](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/creating-a-pull-request-template-for-your-repository)

### Protections
* Require a pull request before merging - this can be found under `settings > Branches > (Add branch protection rule / Edit)`. This protection prevents pushes directly to the master branch and requires that changes go though a pull request.
* Dismiss stale pull request approvals when new commits are pushed  - this can be found under `Require a pull request before merging `, this prevents people from sneaking in changes after approval has completed. 
* Require status checks to pass before merging - this can be found under `settings > Branches > (Add branch protection rule / Edit)`. This protection prevents pushes directly to the master branch and requires that a pr passes specific defined checks. (we will go over how to set this check up later in the [Automatic Testing](#Automatic-building) section)
[more info](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/defining-the-mergeability-of-pull-requests/managing-a-branch-protection-rule)
* Require branches to be up to date before merging - This rule can be found under `Require status checks to pass before merging` and makes sure that the code was tested as it would be merged before it is allowed to be merged.
* Require conversation resolution before merging - Make sure we are done talking about everything before we merge anything.

**Make sure you save these settings before continuing**

## Issues

To create issue templates to help people provide useful bug reports and to help lower spam set up some issue templates. Some examples on how to do that can be found in this repository at `/.github/ISSUE_TEMPLATE/*`
[more info](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/configuring-issue-templates-for-your-repository)

## Automatic Testing

Every repository should set up automatic testing for pull requests and publishing release candidates.    
To set them up simply copy the file listed at `/.github/workflows/test.yml` from this repository over to your target repository.    
Finally go over to your repositories settings and `test` to the "Require status checks to pass before merging" section of your branch protection rules.
[more info](https://docs.github.com/en/actions/using-workflows)

## Releases

Specification on releases are relatively loose. The only criteria are as follows.
- Releases should only ever be made for a version of the mod that is legal at time of release.
- Any version of a mod that is made illegal after time of release should have its title and jar prefixed with `[ILLEGAL]`.
- Specific game versions of a mod that get superseded by another game version and which are still legal should have their release title prefixed with `[DEPRECATED]`

## Automated Releases

Automated releases are the most hands on part of this process. There are 2 main parts to this process.

1. Configuring versioning and builds on your project
2. Configuring automated builds

### Configuring versioning and builds on your project

In this repository there are samples for `gradle.properties`, `build.gradle`, and `src/resource/fabric.mod.json` make sure you read them thoroughly and integrate their contents into your project.

#### gradle.properties
`gradle.properties` contains only 2 things.
* mod_id - the id that you are going to be using to identify your mod across all versions of the game
* minecraft_version - the version of minecraft that is going to be decompiled to create the mod

#### build.gradle
`build.gradle` is the most complex file of the batch, it contains 4 sections.
* plugins - this section defines a plugin that will be used for versioning your project during local development. It should be merged with the plugins file of your existing `build.gradle` file. (Note this part is technically optional but requires modification of the next part to omit it)
* getVersionMetadata - this section defines a function that will version your project for development and release environments.
* archivesBaseName/version - this section defines some variable that are used in other parts of the build. Make sure they don't conflict with existing variables.
* processResources - this section sets up groovy template expansion for your `fabric.mod.json` file. You will need to merge it with your existing processResources section.

#### fabric.mod.json
`fabric.mod.json` is just setting up your mod versioning and id for template expansion. Make sure to replace the listed fields in your project with the ones defined here.

### Configuring automated builds

To configure automated builds on your project simply copy the file listed at `/.github/workflows/publish.yml` from this repository over to your target repository and replace `**TITLE HERE**` with a proper title for your projects releases.

### Notes about versioning

Projects versions do not follow semantic versioning and instead simply increment. This is because semantic versioning is designed for versioning of libraries that need to guaranty forwards and backwards compatibility of there api with their dependents which is something that we are not doing here.
