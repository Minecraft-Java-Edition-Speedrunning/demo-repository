
# Official Minecraft: Java Edition speedrunning mod repository guide

This repository is here to server as a guide project on how to setup a repository for release and distribution of speedrunning mods. Please read this document in its entirety before setting up a new project.

## Repository naming

The naming of repository is to serve as a method of differentiating mods by both mod identify and supported versions of the game.
Mod repositories names should match the following regex.

`/mcsr-(?<mod_id>\w*)-(?<min_version>\d+\.\d+(?:\.\d+)?)(:?-(?<max_version>\d+\.\d+(?:\.\d+)?))?/`

**examples:**
* `mcsr-sodium-1.16.1`
* `mcsr-voyager-1.14-1.16.5`

Parts of the regex:

`mcsr-` - all mod repositories should be prefixed with `"mcsr-"` this denotes that the repository is a mod and not some other project (for example this repository)

`(?<mod_id>\w*)` - the second part of the string is a unique identifier for the mod. This identifier should be consistent across all version splits for the same mod.

`(?<min_version>\d+\.\d+(?:\.\d+)?)` - the third part of the string is the minimum (or only) version of minecraft that this repository supports.

`(:?-(?<max_version>\d+\.\d+(?:\.\d+)?))?` - the fourth part of the string is an optional maximum version of minecraft that this repository supports if a repository works across multiple versions of minecraft.

## Pull requests, branches and protection

All repositories should have a `main` branch that should at all times reflect a stable release candidate of the mod.    
This main branch should have some branch protection rules on it to protect it and keep it stable.
**Protections**
* Require a pull request before merging - this can be found under `settings > Branches > (Add branch protection rule / Edit)`. This protection prevents pushes directly to the master branch and requires that changes go though a pull request.
* Require status checks to pass before merging - this can be found under `settings > Branches > (Add branch protection rule / Edit)`. This protection prevents pushes directly to the master branch and requires that a pr passes specific defined checks. (we will go over how to set this check up later)
<!--- figure out check name and put it here --->
[more info](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/defining-the-mergeability-of-pull-requests/managing-a-branch-protection-rule)

In addition to protecting the branches you should also set up a pull request message that is displayed whenever someone attempts to open up a pull request. This message should be place in `/docs/pull_request_template.md` and an example message can be found in this repository at that path.
[more info](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/creating-a-pull-request-template-for-your-repository)

## Automatic building

Every repository should set up automatic builds for testing pull requests and publishing release candidates. Some example workflows can be found in this project but may need to be modified depending on the target version of the game.   
<!--- write more specifics on auto setting up releases --->
<!--- write more specifics on pr checks --->
[more info](https://docs.github.com/en/actions/using-workflows)

## Releases

Specification on releases are relatively loose. The only criteria are as follows.
- Releases should only ever be made for a version of the mod that is legal at time of release.
- Any version of a mod that is made illegal after time of release should have its title and jar prefixed with `[ILLEGAL]`.
- Specific game versions of a mod that get superseded by another game version and which are still legal should have their release title prefixed with `[DEPRECATED]`
