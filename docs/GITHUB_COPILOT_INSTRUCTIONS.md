# GitHub Copilot Instructions

Every DPC plugin repository must have a `.github/copilot-instructions.md` file. This file is automatically loaded by GitHub Copilot and the GitHub Coding Agent to give them the context they need to make contributions that align with DPC conventions.

See [GitHub's documentation on customising Copilot](https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot) for more details on how this file works.

## Purpose

Without a `.github/copilot-instructions.md` file, Copilot has no awareness of:

- The DPC coding conventions and standards
- The plugin's architecture and design decisions
- The project's technology stack and build tooling
- Community norms (branch model, issue workflow, language files, etc.)

Adding this file ensures that AI-assisted contributions stay consistent with the rest of the project from the very first suggestion.

## Required Content

The file must cover the following areas:

### 1. Link to DPC Conventions

Include a prominent link back to this conventions repository so that the agent can read the full standards:

```markdown
This repository follows the DPC (Dans Plugins Community) conventions defined at
https://github.com/Dans-Plugins/dpc-conventions.
```

### 2. Technology Stack

Briefly describe the languages, frameworks, and build tools the plugin uses. For example:

```markdown
## Technology Stack

- Language: Java (or Kotlin)
- Build tool: Gradle (Groovy DSL)
- Target platform: Spigot / Paper (Minecraft plugin)
- Test framework: JUnit 5
```

### 3. Project Structure

A short description of the key source directories and what they contain, so the agent can navigate the codebase without guessing.

### 4. Coding Conventions

Highlight any project-specific conventions that go beyond the DPC-wide standards, such as:

- Package naming scheme
- Preferred patterns for commands, listeners, or services
- Language-file usage for all user-facing strings

### 5. Workflow Reminders

Reinforce the DPC contribution workflow:

- All changes should be made on a feature branch off `main`
- Pull requests target `main`
- Every pull request should reference a GitHub issue

## Template

Below is a minimal `.github/copilot-instructions.md` template that satisfies all the requirements above.

````markdown
# Copilot Instructions

This repository follows the DPC (Dans Plugins Community) conventions defined at
https://github.com/Dans-Plugins/dpc-conventions. Read those conventions before
making any changes.

## Technology Stack

- Language: Java
- Build tool: Gradle (Groovy DSL)
- Target platform: Spigot / Paper
- Test framework: JUnit 5

## Project Structure

- `src/main/java/` – Plugin source code
- `src/main/resources/` – `plugin.yml`, `config.yml`, and `lang/` files
- `src/test/java/` – Unit tests

## Coding Conventions

- Use the `lang/` resource files for every user-facing string; never hard-code
  messages in Java.
- Follow the existing package structure when adding new classes.
- Annotate every command executor and event listener with `@Override` where applicable.

## Contribution Workflow

- Branch from `main` for all changes.
- Open a pull request against `main`.
- Reference the related GitHub issue in every pull request description.
````

## Checklist

- [ ] `.github/copilot-instructions.md` exists in the repository
- [ ] File includes a link to `https://github.com/Dans-Plugins/dpc-conventions`
- [ ] Technology stack is described
- [ ] Project structure is described
- [ ] Project-specific coding conventions are noted
- [ ] DPC contribution workflow is summarised
