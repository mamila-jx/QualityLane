This GitHub repository contains documentation only.

The full source code is hosted on Codeberg (https://codeberg.org/mamila/QualityLane), an EU-based hosting service, chosen for data sovereignty and security considerations.

# QualityLane Plugin

QualityLane is a Gradle plugin for standardizing quality checks across Kotlin and Android projects. It centralizes common verification tasks such as Detekt, KtLint, Lint, Sonar configuration, and test execution so that app modules can apply a single plugin and a single configuration model.

## What this plugin does

The plugin provides a reusable Gradle configuration for:

- Detekt code analysis
- KtLint formatting and static checks
- Android Lint integration
- SonarQube property configuration
- test execution configuration
- a single `quality` task that groups the enabled checks

This keeps repetitive quality setup out of each project and makes it easier to enforce a consistent quality gate.

## Plugin identifier

```kotlin
id("dev.mamila.qualitylane")
```

## Installation

Add the plugin to the consuming project.

```kotlin
plugins {
    id("dev.mamila.qualitylane") version "1.0.0"
}
```

If you are using `pluginManagement`, ensure the repository is available:

```kotlin
pluginManagement {
    repositories {
        gradlePluginPortal()
        google()
        mavenCentral()
    }
}
```

## Configuration

The plugin exposes a configuration block named `qualityLane`.

```kotlin
qualityLane {
    detekt {
        enabled.set(true)
        ignoreTests.set(false)
    }

    sonar {
        enabled.set(true)
        projectName.set("My App")
        projectKey.set("my-app")
        hostUrl.set("https://sonar.example.com")
        token.set("${System.getenv("SONAR_TOKEN")}")
        organization.set("my-org")
    }

    lint {
        enabled.set(true)
        abortOnError.set(true)
        warningsAsErrors.set(false)
    }

    ktLint {
        enabled.set(true)
    }

    runTests {
        enabled.set(true)
        ignoreFails.set(false)
    }
}
```

## Available extension options

### detekt

- `enabled: Property<Boolean>`
- `configFile: RegularFileProperty`
- `ignoreTests: Property<Boolean>`

### sonar

- `enabled: Property<Boolean>`
- `projectName: Property<String>`
- `projectKey: Property<String>`
- `hostUrl: Property<String>`
- `token: Property<String>`
- `organization: Property<String>`

### lint

- `enabled: Property<Boolean>`
- `abortOnError: Property<Boolean>`
- `warningsAsErrors: Property<Boolean>`

### ktLint

- `enabled: Property<Boolean>`

### runTests

- `enabled: Property<Boolean>`
- `ignoreFails: Property<Boolean>`

## Tasks provided by the plugin

The plugin registers and configures the following tasks:

- `quality` — aggregate task for enabled verification checks
- `detekt` — runs Detekt analysis
- `ktlint` — runs KtLint over the project source files
- `lint` — integrates Android Lint when an Android plugin is applied
- `sonar` — configures SonarQube analysis when the Sonar plugin is available
- standard Gradle test tasks such as `test` — the plugin applies the configured test behavior

The `quality` task depends on each enabled check after the project is evaluated.

## Typical usage examples

### Run all enabled quality checks

```bash
./gradlew quality
```

### Run only the project test suite

```bash
./gradlew test
```

### Run Detekt only

```bash
./gradlew detekt
```

### Run KtLint only

```bash
./gradlew ktlint
```

## Local testing

If you want to validate the plugin locally before publishing it anywhere, publish it to your local Maven cache and consume it from another Gradle project.

From this repository, run:

```bash
./gradlew publishToMavenLocal
```

Then in the consuming project, add `mavenLocal()` to `pluginManagement`:

```kotlin
pluginManagement {
    repositories {
        mavenLocal()
        gradlePluginPortal()
        google()
        mavenCentral()
    }
}
```

and apply the plugin by ID:

```kotlin
plugins {
    id("dev.mamila.qualitylane") version "1.0.0"
}
```

This is useful for local integration testing while the plugin is still a prototype. It does not replace a public artifact repository, but it is the easiest way to validate the plugin in a real downstream project before publishing it.

## Notes and limitations

- The plugin is designed around a Gradle plugin model and is intended to be consumed by version rather than by embedding the plugin code directly in an app repository.
- The plugin currently targets the configuration patterns that are already present in the repository and is intended to be used as a reusable quality gate.
- Sonar configuration depends on the Sonar plugin and the available Gradle environment. If the configured report files are not generated, the plugin will not inject invalid paths.
- Lint configuration is applied when an Android Gradle plugin is present in the project.

## Repository layout

This repository contains the Gradle plugin project and the files needed to build and publish it. The plugin is defined in the `plugin` module, and the root build coordinates the Gradle project setup.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.

## Contributing

Contributions are welcome. Keep changes focused and ensure the plugin remains easy to configure, version, and consume in downstream projects.
