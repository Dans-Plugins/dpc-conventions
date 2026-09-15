# Release Automation

This document describes the convention for automatically building and attaching plugin JARs to GitHub Releases. [Medieval Factions](https://github.com/Dans-Plugins/Medieval-Factions) is the reference implementation.

For what a release *means* to an operator — the stable and experimental channels, what stable promises, and how a build is promoted — see [Release Channels](RELEASE_CHANNELS.md).

## Overview

When a GitHub Release is published, a GitHub Actions workflow should build the plugin and attach the resulting JAR file to the release — unless the release was published with a JAR already attached, in which case the workflow does nothing. This ensures that every release has an artifact without requiring manual uploads, while letting the organisation's release automation publish the exact JAR that passed verification (see [Release Channels](RELEASE_CHANNELS.md)) without a second, unverified JAR being attached beside it.

Experimental builds are provided by the separate rolling `dev` pre-release, not by draft releases.

## Workflow File

Place the workflow at `.github/workflows/release.yml`.

```yaml
name: Release

on:
  release:
    types: [ published ]

permissions:
  contents: write

jobs:
  build-and-attach:
    runs-on: ubuntu-latest
    # A release published with a .jar already attached was produced by the release automation,
    # which attaches the exact JAR that passed verification. Rebuilding here would attach a second,
    # unverified JAR — and Dan's Plugin Manager installs the first .jar asset it finds.
    if: ${{ !contains(join(github.event.release.assets.*.name, ','), '.jar') }}

    steps:
      - uses: actions/checkout@v4

      - name: Set up JDK 17
        uses: actions/setup-java@v4
        with:
          java-version: '17'
          distribution: 'temurin'

      - name: Grant execute permission for gradlew
        run: chmod +x gradlew

      - name: Build with Gradle
        run: ./gradlew clean build

      - name: Upload JAR to release
        uses: softprops/action-gh-release@v2
        with:
          files: build/libs/*.jar
```

## How It Works

1. **Trigger** – The workflow fires whenever a release is published (`on: release: types: [ published ]`). This covers a release published directly and a draft that is published later. It does not fire while a release is still a draft — GitHub does not run workflows for draft releases' `created` events at all, so the older `created` trigger silently never built a draft-then-published release.
2. **Skip when a JAR is attached** – The job's `if:` inspects the published release's assets and does nothing when a `.jar` is already there. `gh release create <tag> <jar>` creates the release as a draft, uploads the asset, then publishes, so the `published` event already carries the asset. A release published by hand without an asset still gets its build.
3. **Permissions** – The workflow declares `contents: write` so the `GITHUB_TOKEN` can upload release assets. Without this, repositories that default to read-only permissions will receive a 403 error.
4. **Build** – The project is checked out, JDK 17 is configured, and `./gradlew clean build` produces the plugin JAR.
5. **Attach** – The `softprops/action-gh-release` action uploads every JAR found in `build/libs/` to the release that triggered the run. The release tag is detected automatically from the event context.

## Customisation

### Filtering JARs

If the build produces multiple JARs (e.g. a main artifact and a `-sources` or `-javadoc` artifact) and you only want to attach the main one, narrow the glob pattern:

```yaml
      - name: Upload JAR to release
        uses: softprops/action-gh-release@v2
        with:
          files: build/libs/<PluginName>-*.jar
```

### Shadow / Fat JARs

Plugins that use the Shadow plugin to produce a fat JAR may output it to a different path. Adjust the `files` pattern accordingly:

```yaml
          files: build/libs/*-all.jar
```

## Creating a Release by Hand

Stable releases are normally cut by the release automation (see [Release Channels](RELEASE_CHANNELS.md)). To create one by hand:

1. Go to the repository's **Releases** page on GitHub.
2. Click **Draft a new release**.
3. Choose or create a tag (e.g. `v1.0.0`).
4. Fill in the release title and description.
5. Click **Publish release**. Saving a draft does not trigger a build; publishing it does.

Once the release is published, the workflow runs and the built JAR appears in the release's **Assets** section within a few minutes. If you attach a JAR yourself before publishing, the workflow leaves the release alone.

## Checklist

- [ ] `.github/workflows/release.yml` exists and uses the template above
- [ ] The workflow triggers on `release` events with type `published`
- [ ] The job skips when the published release already carries a `.jar` asset
- [ ] The workflow declares `permissions: contents: write`
- [ ] The `files` glob matches the plugin's output JAR path
- [ ] A test release (or pre-release) confirms the JAR is built and attached correctly
