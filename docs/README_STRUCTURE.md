# README Structure

Every DPC plugin repository should have a `README.md` that follows the structure described below. [Medieval Factions](https://github.com/Dans-Plugins/Medieval-Factions) is the reference implementation.

## Required Sections

The sections below must appear in this order.

### 1. Description

A short paragraph explaining what the plugin does and why it exists. Include any notable version history or major contributors if relevant.

```markdown
## Description

<Plugin name> is a Minecraft plugin that <short description of what it does>.
```

### 2. Installation

Step-by-step instructions for getting the plugin running on a server. Split into sub-sections as needed (e.g. first-time installation, optional integrations, companion plugins).

```markdown
## Installation

### First Time Installation

1. Download the plugin from [SpigotMC](<link>).
2. Place the jar in the `plugins` folder of your server.
3. Restart your server.
```

If the plugin integrates with other plugins (e.g. Dynmap, PlaceholderAPI) or has official expansions, list them here with links.

### 3. Usage

Links to user-facing documentation. At minimum this should point to a User Guide and a Commands reference. Additional guides (configuration, flags, database querying, etc.) should be linked here too.

```markdown
## Usage

### Documentation

- [User Guide](USER_GUIDE.md) – Getting started and common scenarios
- [Commands Reference](COMMANDS.md) – Complete list of all commands
- [Configuration Guide](CONFIG.md) – Detailed configuration options

### Wiki & Additional Resources

- [Wiki Guide](<wiki link>)
- [FAQ](<wiki link>)
```

### 4. Support

Where users can get help. For DPC projects, include the Discord link; omit it for non-DPC projects. Always include a link to open a bug report.

```markdown
## Support

<!-- Include the following line only for DPC projects: -->
You can find the support Discord server [here](https://discord.gg/xXtuAQ2).

### Experiencing a bug?

Please fill out a bug report [here](<issues link>).

- [Known Bugs](<issues link with bug label>)
```

### 5. Contributing

Links to the `CONTRIBUTING.md` file and to any developer notes in the wiki.

```markdown
## Contributing

- [CONTRIBUTING.md](CONTRIBUTING.md)
- [Notes for Developers](<wiki link>)
```

### 6. Testing

Instructions for running the unit-test suite. Include both Linux and Windows commands.

````markdown
## Testing

### Unit Tests

Linux:

    ./gradlew clean test

Windows:

    .\gradlew.bat clean test

If you see `BUILD SUCCESSFUL`, the tests have passed.
````

### 7. Development

Instructions for spinning up a local development environment (Docker, hot-reloading, etc.).

```markdown
## Development

### Test Server with Plugin Hot-Reloading

A Docker-based test server is available for development.

#### Setup

1. Copy `sample.env` to `.env` and configure as needed.
2. Build the plugin: `./gradlew build`
3. Start the test server: `./up.sh`

#### Stopping the Test Server

    ./down.sh
```

### 8. Authors and Acknowledgement

A table listing all major contributors and their contributions. Translators should be listed in a separate sub-table.

```markdown
## Authors and Acknowledgement

### Developers

| Name | Main Contributions |
|------|--------------------|
| <name> | <contribution> |

### Translators

| Name | Language(s) |
|------|-------------|
| <name> | <language> |

<Optional paragraph about the origin story of the plugin.>
```

### 9. License

If the repository already contains a `LICENSE` file, include a section that references it. **Do not add, change, or remove a `LICENSE` file.** These conventions are sometimes applied to forks where the original project has its own license (or intentionally has none), so licensing decisions must be left to the repository owner.

```markdown
## License

This project is licensed under the [<license name>](LICENSE).

See the [LICENSE](LICENSE) file for full details.
```

If no `LICENSE` file exists, omit this section entirely.

### 10. Project Status

A brief statement of the current development status and a link to the bStats page (if the plugin collects metrics).

```markdown
## Project Status

This project is in active development.

### bStats

You can view the bStats page for the plugin [here](<bstats link>).
```

## Optional Sections

The following sections are encouraged when relevant:

- **Roadmap** – Link to milestones or planned-features issues.
- **Changelog** – Link to `CHANGELOG.md` if one is maintained.
- **External API / Add-on Development** – If the plugin exposes a public API for other developers.

## Checklist

Use the following checklist when reviewing a plugin README:

- [ ] Description section present
- [ ] Installation section with numbered steps
- [ ] Usage section with links to User Guide and Commands Reference
- [ ] Support section with Discord link (DPC projects only) and bug-report link
- [ ] Contributing section with link to `CONTRIBUTING.md`
- [ ] Testing section with Gradle commands
- [ ] Development section with Docker setup instructions
- [ ] Authors and Acknowledgement table
- [ ] License section references existing `LICENSE` file (if one exists; omit section if not)
- [ ] Project Status section with bStats link
