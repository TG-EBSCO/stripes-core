# Stripes release procedure

<!-- ../../okapi/doc/md2toc -l 2 release-procedure.md -->
* [Version numbers, branches and tags](#version-numbers-branches-and-tags)
* [Before you release](#before-you-release)
* [Release procedure](#release-procedure)
* [Working towards the next release](#working-towards-the-next-release)
* [Notes on dependencies](#notes-on-dependencies)
* [Notes on testing](#notes-on-testing)
* [Note on access to the NPM repository](#note-on-access-to-the-npm-repository)

NOTE. This document is subject to revision.


## Version numbers, branches and tags

The libraries that make up Stripes adhere to [the FOLIO
version-numbering scheme](http://dev.folio.org/community/contrib-code#version-numbers), which is essentially identical to [Semantic Versioning](http://semver.org/). All releases are tagged in git.

Each release is tagged with a name beginning with `v` and followed by the version number -- for example, `v2.3.5`.

Patch releases can be done on a branch whose name begins with `b` and is followed by the major and minor version -- for example, `b2.3`. From this branch, we will make some number of tagged releases, each with a different trivial version number (one for each patch release: bug-fixes and security patch, for example, `v2.3.1`). If no such branch exists for the minor version you're seeking to patch and `master` has since moved on with commits you prefer not to include, please create a branch from the tag.


## Before you release

Please make sure that your code runs clean. Specifically:

* It should pass `yarn lint` with no errors.
* It should not emit any warnings on the JavaScript console.

Make sure that your repository's `CHANGELOG.md` is up to date:

* Bullet point for each bug fixed/new feature added/etc.
* Each bullet point ends by stating the ID of the Jira issue, when there is one. If there is not, consider creating one and referencing it, but there is no need to make a religion out of this.

You may need to check the git history to ensure that all major changes are covered by the change-log entries.

Ensure that each of the Jira issues listed in the change-log is tagged to the number of the release that it's going to be a part of. Issues are generally tagged to the minor release following the most recently released versions, but will need to be re-targeted to the next _major_ release if a backwards-incompatible change is to be included.

## Release procedure

* Increment the version number in `package.json`, if this has not already been done -- bumping the major version if there are backwards-incompatible changes, and the minor version if all changes are backwards-compatible.
* Make any necessary additions to the project's `CHANGELOG.md` describing how the new release differs from the previous one. The purpose of the change-log is to allow a module developer to answer the question "Do I need to upgrade to the new version of this package?", so aim for a high-level overview rather than enumerating every change, and concentrate on API-visible rather than internal changes.
* Set the date of the release in the change-log, adding a link to the tag and another to the full set of differences from the previous release as tracked on GitHub: follow the formatting of earlier change-log entries.
* Commit the `package.json` and `CHANGELOG.md` changes with the message "Release vVERSION". For example, `git commit -m "Release v2.3.0" .`
* Create a tag for the specific version to be released (`git tag v2.3.0`).
* Publish the package to the npm repository using `npm publish`. (You will need credentials to do this: see note below.)
* Push the changed module back to git (`git push`).
* Push the new release tag back to git (`git push origin tag v2.3.0`).


## Working towards the next release

Decide what the version number of the next release is likely to be -- almost always a minor-version bump from the release that has just been made.

In the Jira project, create a new version with this number, so that issues can be associated with it.

Create a new entry at the top of the change-log for the forthcoming version, so there is somewhere to add entries. But do not include a date for the entry: instead, mark it as "IN PROGRESS", as in [the in-progress `stripes-core` change-log from before v0.5.0](https://github.com/folio-org/stripes-core/blob/e058702cb19b32f607f7fb40b15ddf00cd6b45ad/CHANGELOG.md).


## Notes on dependencies

* Each stripes platform (`stripes-sample-platform`, etc.) depends directly on `stripes-core` and a set of application modules.
* Each application module (`users`, `items`, etc.) has `stripes-core` as a peer-dependency.
* `stripes-core` depends directly on `stripes-connect`, `stripes-components`, etc.
* [**Optional.** Guerilla dependencies. Application modules, such as Users and Items, that consume the released module's API and need the new facility should update the relevant peer-dependency to their `package.json`, specifying the new version. In general, Stripes modules should use the widest possible range of version compatibility for the packages they depend on.]


## Notes on testing

XXX to be done. See http://dev.folio.org/doc/automation


## Note on access to the NPM repository

Before you can do `yarn publish`, you will need access to the Index Data/FOLIO NPM repository at `repository.folio.org`. Get these credentials from an administrator. Once you have them, login as follows:
```
$ npm config set @folio:registry https://repository.folio.org/repository/npm-folio/
$ npm adduser --registry=https://repository.folio.org/repository/npm-folio/
Username: mike
Password: ********
Email: (this IS public) mike@indexdata.com
Logged in as mike on https://repository.folio.org/repository/npm-folio/.
$
```
You will then be able to release packages in the relevant repository:
```
$ npm publish
+ @folio/stripes-components@0.0.2
$
```
