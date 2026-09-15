# Release Channels

This document describes the two release channels a Dans-Plugins plugin publishes, what each one promises a server operator, and how a build moves from one to the other. [Release Automation](RELEASE_AUTOMATION.md) covers the workflow that attaches a JAR to a release; this page covers what a release *means*.

## The two channels

| Channel | How an operator gets it | What it is |
|---|---|---|
| **Stable** | `/dpm get <plugin>` | The repository's latest published, non-prerelease GitHub release. This is what [Dan's Plugin Manager](https://github.com/Dans-Plugins/Dans-Plugin-Manager) resolves from GitHub's `/releases/latest`, so a release marked *pre-release* is never served here. |
| **Experimental** | `/dpm get <plugin> --experimental` | The release tagged `dev`: a rolling pre-release rebuilt from the default branch on every push that changes something other than documentation. It is unreleased, unreviewed code that has not been through any release check. |

A plugin's `dev` release is published by `.github/workflows/dev-release.yml` (Dans-Set-Home is a representative copy). Its build steps deliberately mirror `release.yml`, so the experimental JAR and the stable JAR are built the same way.

## What "stable" promises

A stable release is one the organisation is prepared to have installed unattended by `/dpm update` on a server it does not control. That is a promise, and it has content. A stable release:

1. **Was built from a commit that had settled.** The default branch had not changed for at least three days when the release was cut, so it is never a half-landed sequence of pull requests.
2. **Booted on a real server.** The JAR, together with the current stable release of every plugin it `depend`s on, was started on a Spigot server in CI, enabled cleanly, registered every command in its `plugin.yml`, stopped cleanly, and started a second time against the data folder it had just written.
3. **Loaded the previous stable release's data intact** — for plugins that persist state. A data folder produced by the previous stable release, running a scripted scenario, was loaded by the candidate; every entity type was counted, read back, written to, and reloaded after a restart. Plugins with no persistent state skip this check; the release notes say which applies.
4. **Did not break its dependents.** A plugin that other plugins `depend` on was verified by booting each dependent's current stable release against it.
5. **Says what it contains.** The release notes list the merged pull requests since the previous stable release, with links. Changes produced by automated development sessions are listed like any other; they are not hidden behind prose.
6. **Can be withdrawn.** If a stable release turns out to be bad, it is marked *pre-release*. GitHub then drops it from `/releases/latest`, `/dpm update` resolves to the previous stable release on its next run, and nothing is deleted. A withdrawn release stays visible with its notes so the reason can be recorded.

What stable does **not** promise:

- **Downgrade compatibility.** Rolling back to an older release is not guaranteed to preserve data written by the newer one. Back up the plugin's data folder before an upgrade you might want to reverse.
- **Correctness of every feature.** The checks above prove that a release boots, keeps data, and reloads it. Whether a feature behaves correctly in the world is what the plugin's own tests and its issue tracker are for.
- **Every Minecraft version the plugin declares.** Verification runs on the Minecraft version the organisation's own servers run. Older `api-version` targets are supported on a best-effort basis.

## How a build becomes stable

Stable releases are cut by the organisation's release automation, not by hand, once the checks above pass. The cadence is bounded: a plugin whose default branch has changed gets a stable release within roughly a week of the change settling, and at most one stable release per week unless a merged pull request is labelled `release:hotfix`.

The version number is decided from the diff since the previous stable release, with floors that cannot be lowered: a change to storage, migration, or serialization code is at least a **minor** release; a removed or renamed config key, a removed command, or a change to `api-version` is a **major** release. A maintainer can force a level by labelling a pull request `release:major`, `release:minor`, or `release:patch`, and can hold a release entirely with `release:hold` on any open pull request.

A candidate that fails a check is not released. The failure is filed as an issue on the plugin's repository, labelled `release-blocked`, with the check's log attached, and no further release is attempted until the default branch changes.

## Which plugins this applies to

The automation is being rolled out in phases. Until a plugin is on the automated promote list, its stable channel is whatever was last published by hand and may be well behind the default branch; its experimental channel is current either way. A plugin's README should say which channel its documentation describes.

## Checklist

- [ ] The repository publishes a `dev` pre-release from `.github/workflows/dev-release.yml`
- [ ] The repository's `release.yml` matches [Release Automation](RELEASE_AUTOMATION.md)
- [ ] Storage, migration, and serialization paths are identifiable (by package or directory) so the version floors can be applied
- [ ] Console-drivable or bot-drivable commands exist to create at least one of each persisted entity, so a save-compatibility scenario can be written
- [ ] The README tells operators which channel it documents
