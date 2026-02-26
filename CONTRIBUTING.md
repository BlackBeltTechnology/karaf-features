# Contributing to karaf-features

This guide covers the development setup, build commands, and contribution workflow for the karaf-features project.

## Development Environment

### Required Software

| Tool | Version | Notes |
|------|---------|-------|
| JDK | 21 | [Azul Zulu](https://www.azul.com/downloads/?version=java-21-lts&package=jdk) recommended |
| Maven | 3.9.4+ | Or use the included Maven Wrapper (`./mvnw`) |

Verify your setup:

```bash
java -version
# Expected: openjdk version "21.x.x" ...

./mvnw -version
# Expected: Apache Maven 3.9.x ...
```

## Build Commands

```bash
# Run tests (validates feature descriptors against Karaf 4.4.7)
./mvnw clean test

# Full build — compile, validate, and install to local repository
./mvnw clean install

# Build a single module
./mvnw -pl karaf-features-jackson clean install

# Skip all feature modules (parent POM only)
./mvnw -DskipModules=true clean install
```

## Project Structure

This is a Maven multi-module project where each module produces a Karaf feature descriptor (`feature.xml`). There is no Java source code — only XML feature definitions that declare OSGi bundles and their dependencies.

```
karaf-features-<name>/
├── pom.xml                    # packaging: feature
└── src/main/feature/
    └── feature.xml            # OSGi feature descriptor
```

See [README.md](README.md) for a complete list of modules and their dependency relationships.

## Submitting an Issue

Before opening a new issue, search the [issue tracker](https://github.com/BlackBeltTechnology/karaf-features/issues) — your problem may already be resolved or discussed.

To help us reproduce and fix bugs quickly, please include:

- Output of `java -version` and `mvn -version`
- The relevant `pom.xml` or `.flattened-pom.xml`
- A minimal use-case that demonstrates the failure

We require a minimal reproduction to keep triage efficient. File new issues using the [issue form](https://github.com/BlackBeltTechnology/karaf-features/issues/new/choose).

## Submitting a Pull Request

This project uses [GitHub's standard forking model](https://guides.github.com/activities/forking/). Fork the repository and submit pull requests from your fork.

> **Important:** Every commit must reference a JIRA ticket number (`JNG-xxx`). There is no commit without a ticket number.

For details about the CI/CD pipeline that runs on your PR, see the [CI Flow documentation](.github/CIFLOW.md).
