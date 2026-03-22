# Documentation Practices

This document describes the documentation standards for DPC plugins. The guiding principle is that every plugin should be as easy to use and contribute to as [Medieval Factions](https://github.com/Dans-Plugins/Medieval-Factions).

## Two-Tier Documentation Model

DPC plugins use a **two-tier model**: key reference documents live in the repository itself (so they are versioned alongside the code), while supplementary guides and community content live in the GitHub Wiki.

### In-Repository Documents

The following files belong in the root of the repository (alongside `README.md`):

| File | Purpose | Required? |
|------|---------|-----------|
| `README.md` | Project overview, quick-start, and index of all other docs | **Yes** |
| `CONTRIBUTING.md` | How to fork, build, test, and submit a pull request | **Yes** |
| `LICENSE` | Full text of the GPL-3.0 license | **Yes** |
| `USER_GUIDE.md` | End-user getting-started guide and common usage scenarios | **Yes** |
| `COMMANDS.md` | Complete reference of every command, its syntax, and required permissions | **Yes** |
| `CONFIG.md` | Explanation of every configuration option with default values and examples | **Yes** |
| `CHANGELOG.md` | Release-by-release summary of changes | Recommended |
| `<FEATURE>_FLAGS.md` | Reference for any flag or toggle system the plugin exposes (e.g. `FACTION_FLAGS.md`) | When applicable |
| `DATABASE_QUERYING.md` | Instructions for querying the plugin's data store directly | When applicable |

### GitHub Wiki Pages

The wiki is best for content that changes frequently, is community-maintained, or links to external resources:

| Wiki Page | Purpose |
|-----------|---------|
| Guide | Narrative walkthrough for new users |
| FAQ | Answers to common questions |
| Placeholders | PlaceholderAPI placeholder reference |
| Developer Notes | Architecture overview and coding conventions for contributors |
| External API Documentation | Public API reference for add-on developers |

## Writing Style

- Use plain, concise language. Assume the reader is a Minecraft server operator, not a developer.
- Use numbered lists for sequential steps and bullet lists for unordered items.
- Use fenced code blocks (triple backticks) for commands, config snippets, and file paths.
- Link between documents rather than repeating the same content in multiple places.

## USER_GUIDE.md

The user guide should answer: *"I just installed this plugin – what do I do next?"*

Recommended structure:

1. **Prerequisites** – Any required plugins or server setup.
2. **First Steps** – The very first thing a player or admin should do after installation.
3. **Common Scenarios** – Walkthrough of the most-used features as short step-by-step examples.
4. **Permissions** – A table of permission nodes, their default value, and a short description.

## COMMANDS.md

The commands reference should be a complete, scannable list of every command the plugin adds.

Recommended structure per command:

```markdown
### /examplecommand <required> [optional]

**Description:** What the command does.  
**Permission:** `plugin.command.example`  
**Usage:** `/examplecommand <argument>`  
**Example:** `/examplecommand myvalue`
```

Group related commands under sub-headings (e.g. `## Faction Commands`, `## Admin Commands`).

## CONFIG.md

The configuration guide should explain every option in the plugin's `config.yml`.

Recommended structure:

````markdown
## option-name

**Type:** boolean / integer / string  
**Default:** `true`  
**Description:** What this option controls and when you would change it.

**Example:**

```yaml
option-name: false
```
````

List options in the same order they appear in the default `config.yml` so that users can follow along.

## CHANGELOG.md

Follow [Keep a Changelog](https://keepachangelog.com/en/1.0.0/) conventions:

- One `## [version] – YYYY-MM-DD` heading per release.
- Changes grouped under `Added`, `Changed`, `Deprecated`, `Removed`, `Fixed`, `Security`.
- Unreleased changes listed under `## [Unreleased]` at the top.

## Keeping Documentation Up to Date

- Documentation changes that accompany a code change should be included in the **same pull request**.
- Pull request templates should include a documentation checklist item.
- Reviewers should reject pull requests that add user-facing features without updating the relevant documentation files.
