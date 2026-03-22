# Testing and CI

This document describes the testing standards and continuous-integration setup used across DPC plugins. [Medieval Factions](https://github.com/Dans-Plugins/Medieval-Factions) is the reference implementation.

## Unit Tests

All DPC plugins use [Gradle](https://gradle.org/) as their build tool and test runner.

### Running Tests

**Linux / macOS**

```bash
./gradlew clean test
```

**Windows**

```bat
.\gradlew.bat clean test
```

A passing test suite produces a `BUILD SUCCESSFUL` message at the end of the output.

### Test Location

Unit tests live in `src/test/java/` (mirroring the main source tree in `src/main/java/`). Each test class should test a single class or a small, cohesive group of related classes.

### Mocking Spigot

Testing Bukkit/Spigot plugins requires careful isolation because the Spigot API is not available outside a running server. Use a mocking library (e.g. [Mockito](https://site.mockito.org/)) to stub out `Player`, `World`, `Server`, and other Bukkit objects. When behaviour cannot be unit-tested in isolation, cover it with integration tests in the Docker-based test server instead.

## Docker-Based Development Server

For integration testing and manual verification, DPC plugins provide a Docker Compose setup that runs a local Spigot server.

### Setup

1. Install [Docker Desktop](https://www.docker.com/products/docker-desktop).
2. Copy `sample.env` to `.env` and set any required variables.
3. Build the plugin:
   ```bash
   ./gradlew build
   ```
4. Start the server:
   ```bash
   ./up.sh
   ```
   The server will be accessible at `localhost:25565`.

### Plugin Hot-Reloading

DPC plugins support hot-reloading using [ServerUtils](https://www.spigotmc.org/resources/serverutils.106413/) so that you do not need to restart the server after every code change.

**Recommended approach – use the reload script:**

```bash
./reload-plugin.sh
```

**Manual approach:**

1. Build the updated jar: `./gradlew build`
2. Copy the new jar into the running container.
3. Use ServerUtils commands in-game or in the server console:
   - `/serverutils reload <PluginName>` – reload the plugin
   - `/serverutils unload <PluginName>` – unload the plugin
   - `/serverutils load <PluginName>` – load the plugin
   - `/serverutils list` – list all loaded plugins

### Stopping the Server

```bash
./down.sh
```

## Continuous Integration

DPC plugins use [GitHub Actions](https://docs.github.com/en/actions) for CI. The CI pipeline should run on every push and pull request targeting the `main` and `develop` branches.

### Recommended Workflow

A minimal CI workflow (`.github/workflows/build.yml`) should:

1. Check out the repository.
2. Set up the appropriate JDK version (17 is recommended for modern Spigot plugins).
3. Grant execute permission to the Gradle wrapper.
4. Run `./gradlew clean build` to compile and run all tests.

```yaml
name: Build

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main, develop ]

jobs:
  build:
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
```

### Pull Request Checks

- The CI build must pass before a pull request can be merged.
- Branch protection rules should be enabled on `main` and `develop` to enforce this.

## Test Coverage Expectations

| Area | Expectation |
|------|-------------|
| Pure business logic (no Bukkit dependency) | Unit-tested |
| Command handlers | Tested with mocked `CommandSender` / `Player` |
| Configuration parsing | Unit-tested |
| Event listeners | Integration-tested via Docker server where mocking is impractical |
| Database / persistence layer | Unit-tested with an in-memory store or mock, or integration-tested |

## Checklist

- [ ] `./gradlew clean test` runs without errors
- [ ] Tests live under `src/test/java/`
- [ ] Bukkit objects are mocked where necessary
- [ ] `sample.env` exists and is committed; `.env` is in `.gitignore`
- [ ] `docker-compose.yml` (or `compose.yml`) exists for the development server
- [ ] `up.sh`, `down.sh`, and `reload-plugin.sh` scripts are present and executable
- [ ] `.github/workflows/build.yml` triggers on pushes and PRs to `main` and `develop`
- [ ] Branch protection rules require the CI build to pass before merging
