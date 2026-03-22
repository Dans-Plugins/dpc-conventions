# Release Automation

This document describes the convention for automatically building and attaching plugin JARs to GitHub Releases. [Medieval Factions](https://github.com/Dans-Plugins/Medieval-Factions) is the reference implementation.

## Overview

When a maintainer creates a new GitHub Release, a GitHub Actions workflow should automatically build the plugin and attach the resulting JAR file to the release. This ensures that every release has a consistent, reproducible artifact without requiring manual uploads.

## Workflow File

Place the workflow at `.github/workflows/release.yml`.

```yaml
name: Release

on:
  release:
    types: [ created ]

jobs:
  build-and-attach:
    runs-on: ubuntu-latest

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

1. **Trigger** – The workflow fires whenever a new release is created (`on: release: types: [ created ]`). Drafts do not trigger the workflow; it runs only when a release is published.
2. **Build** – The project is checked out, JDK 17 is configured, and `./gradlew clean build` produces the plugin JAR.
3. **Attach** – The `softprops/action-gh-release` action uploads every JAR found in `build/libs/` to the release that triggered the run. The release tag is detected automatically from the event context.

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

## Creating a Release

1. Go to the repository's **Releases** page on GitHub.
2. Click **Draft a new release**.
3. Choose or create a tag (e.g. `v1.0.0`).
4. Fill in the release title and description.
5. Click **Publish release**.

Once the release is published, the workflow will run automatically. The built JAR will appear in the release's **Assets** section within a few minutes.

## Checklist

- [ ] `.github/workflows/release.yml` exists and uses the template above
- [ ] The workflow triggers on `release` events with type `created`
- [ ] The `files` glob matches the plugin's output JAR path
- [ ] A test release (or pre-release) confirms the JAR is built and attached correctly
