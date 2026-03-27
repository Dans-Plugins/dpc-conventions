# Contributing Standards

Every DPC plugin repository must have a `CONTRIBUTING.md` file in its root. This document defines what that file should contain and provides a template.

See the [Medieval Factions CONTRIBUTING.md](https://github.com/Dans-Plugins/Medieval-Factions/blob/main/CONTRIBUTING.md) for a complete reference example.

## Required Sections

### 1. Thank You

A short, welcoming opening that thanks the reader for their interest.

### 2. Links

Quick links to the project website and at least one place where contributors can ask questions. For DPC projects this is typically Discord; for other projects use GitHub Issues, Discussions, or another appropriate support channel.

```markdown
## Links

- [Website](https://dansplugins.com)
<!-- Include the following line only for DPC projects: -->
- [Discord](https://discord.gg/xXtuAQ2)
```

### 3. Requirements

A brief list of what the contributor needs before they start:

- A GitHub account
- Git installed locally
- A Java IDE or text editor
- A basic understanding of Java (or Kotlin, as appropriate)

### 4. Getting Started

Step-by-step instructions for forking and setting up the project locally:

1. Create a GitHub account if you don't have one.
2. Fork the repository using the **Fork** button.
3. Clone your fork: `git clone https://github.com/<your-username>/<plugin-name>.git`
4. Open the project in your IDE.
5. Build the plugin: `./gradlew build`
6. If the build fails, open an issue.

### 5. Identifying What to Work On

Explain how work items are tracked:

- **Issues** – All work items are tracked as GitHub issues.
- **Milestones** – Issues are grouped into milestones that represent upcoming releases.

Link to the repository's issues page and milestones page.

### 6. Making Changes

Clear workflow for creating a contribution:

1. Make sure an issue exists for the work. Create one if it doesn't.
2. Check out the `develop` branch: `git checkout develop`
3. Create a feature branch: `git checkout -b <descriptive-branch-name>`
4. Make your changes.
5. Test your changes (see [Testing](#testing)).
6. Commit: `git commit -m "Brief description of changes"`
7. Push to your fork: `git push origin <branch-name>`
8. Open a pull request against the `develop` branch of the upstream repository. Include a description and link the related issue using `#<number>`.
9. Address any review feedback.
10. Once approved, the maintainer will merge the PR into `develop`.

#### Language Files

If your change adds or modifies any user-facing strings, update the relevant language file(s) in `src/main/resources/lang/`. If you are adding a new language, create a new file there.

### 7. Testing

Explain how to verify that changes work:

- Describe how to run the automated test suite (see [Testing and CI](TESTING_AND_CI.md)).
- Describe how to run a local Spigot server (Docker is preferred).

### 8. Questions

Encourage contributors to ask questions and, for DPC projects, provide the Discord server invite link.

## Template

Below is a minimal `CONTRIBUTING.md` template that satisfies all the requirements above.

````markdown
# Contributing

## Thank You

Thank you for your interest in contributing to <Plugin Name>! This guide will help you get started.

## Links

- [Website](https://dansplugins.com)
<!-- Include the following line only for DPC projects: -->
- [Discord](https://discord.gg/xXtuAQ2)

## Requirements

- A GitHub account
- Git installed on your local machine
- A Java IDE or text editor
- A basic understanding of Java

## Getting Started

1. [Sign up for GitHub](https://github.com/signup) if you don't have an account.
2. Fork the repository by clicking **Fork** at the top right of the repo page.
3. Clone your fork: `git clone https://github.com/<your-username>/<plugin>.git`
4. Open the project in your IDE.
5. Build the plugin: `./gradlew build`
   If you encounter errors, please open an issue.

## Identifying What to Work On

### Issues

Work items are tracked as [GitHub issues](<issues-link>).

### Milestones

Issues are grouped into [milestones](<milestones-link>) representing upcoming releases.

## Making Changes

1. Make sure an issue exists for the work. If not, create one.
2. Switch to `develop`: `git checkout develop`
3. Create a branch: `git checkout -b <branch-name>`
4. Make your changes.
5. Test your changes.
6. Commit: `git commit -m "Description of changes"`
7. Push: `git push origin <branch-name>`
8. Open a pull request against `develop`, link the related issue with `#<number>`.
9. Address review feedback.

### Language Files

Update `src/main/resources/lang/` for any user-facing string changes.

## Testing

Run the unit tests with:

Linux: `./gradlew clean test`  
Windows: `.\gradlew.bat clean test`

For manual testing, start a local Spigot server:

    docker compose up

## Questions

<!-- For DPC projects, use the following line: -->
Ask in the [Discord server](https://discord.gg/xXtuAQ2).

<!-- For non-DPC projects, use something like: -->
Open a GitHub Discussion or issue in this repository.
````

## Checklist

- [ ] `CONTRIBUTING.md` exists in the repository root
- [ ] Thank-you opening paragraph
- [ ] Links to website and Discord (Discord for DPC projects only)
- [ ] Requirements list
- [ ] Getting-started steps with `git clone` and `./gradlew build`
- [ ] Explanation of issues and milestones
- [ ] Branch-based workflow pointing to `develop`
- [ ] Language-file instructions
- [ ] Testing instructions (automated + Docker)
- [ ] Questions / contact section
