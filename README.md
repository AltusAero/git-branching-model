# Git Branching Model

This guide is designed for

We have 5 types of branches: [**Master**](#master), [**Staging**](#staging), [**Feature**](#feature), [**Hotfix**](#hostfix), and [**LTS**](#lts)

Contributors are welcome to make pull requests to [**Staging**](#staging), [**Feature**](#feature), [**Hotfix**](#hostfix), and [**LTS**](#lts).
Contributors are welcome to make pull requests to create new [**Feature**](#feature), [**Hotfix**](#hostfix), and [**LTS**](#lts) branches.
No code is required to create a pull request for a new [**Feature**](#feature) branch if it is a mere proposal.

Maintainers manage the merging between branches and any releases that follow.


## Definitions & Terminology

The term "**stable**" is used when all tests pass and the branch is deemed reliable in the sense of the project's scope (not on its dependencies or platform).

"**LTS.md**" is a file in the root of a repository with a list of long-term support in a format that can be easily human & machine parsed, if the project has an LTS release. The project's **README.md** should link to this file, if it exists. The file should be in markdown table format with `LTS`, `Started`, `Expires`, and `Notes` columns. The `LTS` column should match the lts branch's name, minus the 'lts-' prefix and the `Started` and `Expires` columns should be in YYYY-MM-DD format. Example:

| LTS           | Started       | Expires    | Notes                                                      |
| ------------- | ------------- | ---------- | ---------------------------------------------------------- |
| 1.x           | 2017-08-01    | 2019-08-01 | The first LTS release                                      |
| 2.x           | 2018-01-31    | 2020-01-31 | The second LTS release. This is an example for a [link](#) |


## Branches

### Master

Name: `master`

The public, default, go-to branch that we know and love. Git tags are generated whenever new commits are merged to this branch. This branch will be tagged in accordance to [Semantic Versioning 2.0.0](http://semver.org/spec/v2.0.0.html), even if it means rapid-release of major versions.

### Staging

Name: `staging`

The branch where future features and releases are prepared for master. While the branch is under constant development, this branch aims to be *stable* at all times (thus encourages quality commits and contributions). This branch must be *stable* before being merged onto master.

This branch must also contain an up-to-date **LTS.md** file as well, if any LTS releases are available.

### Feature

Name: `feature-NAME`, where `NAME` is an alias or title of the feature.

A branch for new features, especially those that require long-term development, discussion, and/or wait on a particular dependency/event (such as a platform update). New feature branches should be created by branching from [**Staging**](#staging).

Once the new feature is ready to be implemented, the current [**Staging**](#staging) branch should be re-merged onto the branch to handle any conflicts on the feature branch rather than the [**Staging**](#staging) branch. Once successfully merged to [**Staging**](#staging) the branch should not be updated anymore and is kept **only for archival purposes**.

### Hotfix

Name: `hotfix-NAME` where `NAME` is an alias or title of the fix.

A branch for any emergencies fixes, usually for security purposes. Once *stable*, this branch is merged directly onto [**Master**](#master), [**Staging**](#staging), and any active [**LTS**](#lts) (where fix is applicable) then **deleted**.

### LTS

Name: `lts-MAJOR_VERSION.x`, where `MAJOR_VERSION` is the major version of the lts

An optional branch for any LTS releases created from [**Master**](#master). LTS releases can be useful if the project requires long-term reliability of APIs for dependents. The information for an LTS.md branch can be found in **LTS.md** at the root of the repository.

LTS branches may have unique configuration and scripts to simplify builds and releases. An example for an npm package:
  ```json
  "publishConfig": {
    "tag": "1.x"
  },
  "scripts": {
    "prepublishOnly": "node tools/special-build-for-lts-1.x.js"
  }
  ```
  - In this example:
    - `publishConfig.tag` allows you to run `npm publish` without worrying about accidentally overwriting the default distribution tag ('latest') upon publishing. Run `npm info npm dist-tags` to see a real-world example for how this can be useful.
    - `scripts.prepublishOnly` useful for unique builds for an LTS branch - triggers on `npm publish`

Once support has been dropped, the branch should not be updated anymore and is kept **only for archival purposes**.


## Goals
  - make contributing and maintaining projects as simple as possible, especially for those new to `git`.
  - should be 'welcoming' to innovation so that people can feel comfortable (at any experience level) to make a contribution of any size.


## Notes
Maintainers may find `git branch --set-upstream-to` handy. See the following example:
```sh
# Set local staging to track origin's staging
git checkout staging
git branch --set-upstream-to origin/staging

# Set local master to track origin's master
git checkout master
git branch --set-upstream-to origin/master

# check which local branch is watching which origin branch
git branch -vv
```
