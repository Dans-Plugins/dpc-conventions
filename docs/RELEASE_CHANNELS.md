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
3. **Loaded the previous stable release's data intact** — for plugins that persist state. The previous stable release was booted on the same kind of server, its data was created (by console-driven scenario where the plugin offers one, and by whatever the plugin writes on its own), and the candidate was then booted over that data: it had to enable, migrate cleanly, report the same loaded-entity counts, keep every file, and do all of that again after its own restart. A plugin with persistent state is **not released** if any part of this fails, and this check is never waived. Plugins with no persistent state skip it; the release notes say which applied. For Medieval Factions the data is a real, anonymised server database (14 factions, 2,628 claims, 69 relationships, 11 locks, a gate) plus a fixture recorded by two bot players (claims made by walking, an alliance, a locked chest, a working gate), booted through five restart cycles and a JSON↔database migration round trip; the previous stable release must also have closed its database cleanly, and a database that does not close on shutdown is a failed check. For the other plugins the data is what the plugin itself writes and what a console scenario creates; bot-driven coverage is extended plugin by plugin.
4. **Did not break its dependents.** For a plugin that other plugins `depend` on, every dependent's current stable release is booted against the candidate — and first against the current stable release, so a dependent that is already broken is reported as such (with an issue on its own repository) rather than blocking the candidate. Only a dependent that works today and would stop working is a reason not to release.
5. **Had already been running on real servers.** No stable release is made from code that has not been running as a development build on at least one real server for three days, as reported by the fleet's usage reporting; when that cannot be confirmed, no release is made. (Servers are never identified; what is confirmed is that startups of that build were reported.)
6. **Says what it contains.** The release notes list the merged pull requests since the previous stable release, with links. Changes produced by automated development sessions are listed like any other; they are not hidden behind prose.
7. **Can be withdrawn.** If a stable release turns out to be bad, it is marked *pre-release*. GitHub then drops it from `/releases/latest`, `/dpm update` resolves to the previous stable release on its next run, and nothing is deleted. A withdrawn release stays visible with its notes so the reason can be recorded.

What stable does **not** promise:

- **Downgrade compatibility.** Rolling back to an older release is not guaranteed to preserve data written by the newer one. Back up the plugin's data folder before an upgrade you might want to reverse.
- **Correctness of every feature.** The checks above prove that a release boots, keeps data, and reloads it. Whether a feature behaves correctly in the world is what the plugin's own tests and its issue tracker are for.
- **Every Minecraft version the plugin declares.** Verification runs on the Minecraft version the organisation's own servers run. Older `api-version` targets are supported on a best-effort basis.

## How a build becomes stable

Stable releases are cut by the organisation's release automation, not by hand, once the checks above pass. The cadence is bounded: a plugin whose default branch has changed gets a stable release within roughly a week of the change settling, and at most one stable release per week unless a merged pull request is labelled `release:hotfix`.

The version number is decided from the change itself, never from a label or from the number a person left in the build file. The previous stable JAR and the candidate JAR are compared on everything an operator or a dependent plugin can observe — `api-version`, commands and aliases, permissions, default config keys and values, dependencies, and (for plugins other plugins compile against) the public Java API — and the changed source files are matched against the plugin's storage code. Anything removed, renamed, or newly required is a **major** release; anything added, any changed default, or any change to storage code is at least a **minor** release; otherwise **patch**. That mechanical result is a floor. A review step then reads the full diff and may raise the level — for a changed on-disk format, a command whose behaviour changed, a default whose meaning changed — but can never lower it. The reasoning and the evidence are printed in every release's notes under *Version*. A maintainer can hold a release entirely with `release:hold` on any open pull request.

A candidate that fails a check is not released. The failure is filed as an issue on the plugin's repository, labelled `release-blocked`, with the check's log attached, and no further release is attempted until the default branch changes.

## Save integrity comes first

The organisation's standing rule for every plugin is that a stable release must never cost an operator their players' data. Every check above exists for that reason, and the order of precedence when they disagree is fixed: a doubt about data blocks the release; a failed data check is never waived, however long the fix takes; and a stable release reported to lose or corrupt data is withdrawn first and investigated second.

What an operator should still do, because no check covers every case:

- **Back up the plugin's data folder before every upgrade** — and, for database-backed plugins, the database — especially before a **major** release.
- **Read the release's *Version* and *Upgrade notes* sections.** A major release names what changed and what to do about it.
- **Report data problems on the plugin's repository immediately.** A report is enough to withdraw a release; the fix comes after.

## Which plugins this applies to

The automation is being rolled out in phases. As of September 2026 every plugin with a flat-file or no data store is on the automated promote list; the database-backed plugins join as their save-compatibility scenarios are written and pass. Until a plugin is on the list, its stable channel is whatever was last published by hand and may be well behind the default branch; its experimental channel is current either way. A plugin's README should say which channel its documentation describes.

## The SpigotMC listing

A plugin's SpigotMC page mirrors the **stable** channel and nothing else: the version it shows is the latest non-prerelease GitHub release, and its download is that release's jar (as an *external download* pointing at the GitHub asset, so the bytes are the ones the gates verified). Experimental builds are never posted there.

SpigotMC has no API for posting, so a stable release is not finished until someone has posted the update by hand. When the automation publishes a release for a listed plugin it opens a `Post <version> to SpigotMC` issue on the repository with the three things the form needs — version string, external download URL, release notes in BBCode — and the issue is closed once the page shows the version. A page that is more than a few days behind the stable channel is a defect, not a backlog.

The page's description is written from the README, not the other way round: what the plugin does, how to install it (this page or `/dpm get`), the links block (source, releases, wiki, dansplugins.com, Discord, bStats), the two release channels, the usage-reporting paragraph where the stable build reports, and the license. Its *tested versions* field states the Minecraft version the release gate booted the build on. Old descriptions that name retired sites, pre-transfer repository paths or Minecraft versions the build no longer targets are replaced, not edited around.

The README links the page in its install step (see [README structure](README_STRUCTURE.md)); a plugin that has no page says so nowhere and simply links the GitHub releases page.

## Checklist

- [ ] The repository publishes a `dev` pre-release from `.github/workflows/dev-release.yml`
- [ ] The repository's `release.yml` matches [Release Automation](RELEASE_AUTOMATION.md)
- [ ] Storage, migration, and serialization paths are identifiable (by package or directory) so the version floors can be applied
- [ ] Console-drivable or bot-drivable commands exist to create at least one of each persisted entity, so a save-compatibility scenario can be written
- [ ] The README tells operators which channel it documents
- [ ] If the plugin has a SpigotMC page, the README links it and the page shows the latest stable release
