# Karaf Features - Project Documentation

## Project Overview


**Repository:** BlackBeltTechnology/karaf-features
**License:** Apache License 2.0
**Java Version:** 21
**Build System:** Maven 3.9.4+ with Maven Wrapper (`./mvnw`)

1. Provides standardized Apache Karaf OSGi feature descriptors for 20+ third-party Java libraries
2. Each module produces a `feature.xml` artifact that declares OSGi bundles, their Maven coordinates, and inter-feature dependencies
3. Features are validated at build time against Karaf 4.4.7 using the karaf-maven-plugin
4. Targets the JUDO platform ecosystem — deployed to both JUDO Nexus and Maven Central
5. No Java source code — the project consists entirely of XML feature descriptors and Maven build configuration

## Code Instructions

1. First think through the problem, read the codebase for relevant files.
2. Before you make any major changes, check in with me and I will verify the plan.
3. Please every step of the way just give me a high level explanation of what changes you made.
4. Make every task and code change you do as simple as possible. We want to avoid making any massive or complex changes. Every change should impact as little code as possible. Everything is about simplicity.
5. Maintain a documentation file that describes how the architecture of the app works inside and out.
6. Never speculate about code you have not opened. If the user references a specific file, you MUST read the file before answering. Make sure to investigate and read relevant files BEFORE answering questions about the codebase. Never make any claims about code before investigating unless you are certain of the correct answer - give grounded and hallucination-free answers.
7. For implementation use TDD (Test-Driven Development): write or update tests first to define the expected behaviour, verify they fail, then write the minimal implementation to make them pass.
8. Use DRY (Don't Repeat Yourself): extract reusable logic into separate classes, utilities, or components. If the same pattern appears in multiple places, refactor it into a shared helper.

## Directory Structure

```
karaf-features/
├── pom.xml                          # Parent POM: version properties, module list, profiles
├── mvnw / mvnw.cmd                  # Maven Wrapper scripts
├── .mvn/                            # Maven Wrapper config (JVM: -Xms1024m -Xmx2048m)
├── .github/workflows/               # CI/CD: build, release, merge-pr, create-release
├── karaf-features-antlr/            # ANTLR 2/3/4
├── karaf-features-apache-arrow/     # Apache Arrow 17
├── karaf-features-apache-commons/   # 30+ Apache Commons libraries
├── karaf-features-apache-httpclient/# HttpClient 3/4
├── karaf-features-apache-poi/       # Apache POI 3/4/5
├── karaf-features-bouncycastle/     # BouncyCastle 1.69–1.81
├── karaf-features-common/           # mail, threeten, opentelemetry, brotli4j, etc.
├── karaf-features-eclipse-emf/      # Eclipse EMF 2.12–2.39
├── karaf-features-eclipse-epsilon/  # Eclipse Epsilon 2.5
├── karaf-features-eclipse-xtext/    # Eclipse Xtext 2.29/2.39
├── karaf-features-google/           # Guava, Guice, Gson, gRPC, BigQuery, Ads, etc.
├── karaf-features-jackson/          # Jackson JSON (core, jaxrs, jaxws, datatypes)
├── karaf-features-jasperreports/    # JasperReports 6/7
├── karaf-features-javassist/        # Javassist bytecode manipulation
├── karaf-features-jdbc/             # H2, PostgreSQL, Oracle, HSQLDB drivers
├── karaf-features-jxls/             # JXLS 2/3 Excel generation
├── karaf-features-subethamail/      # SubEtha Mail embedded SMTP
├── karaf-features-tinybundles/      # TinyBundles OSGi utility
├── karaf-features-xdocreport/       # XDocReport document generation
└── karaf-features-openapi-generator/# OpenAPI Generator (currently disabled)
```

## Core Modules

### Foundational (no cross-module dependencies)

| Module | Type | Purpose |
|--------|------|---------|
| `karaf-features-common/` | feature | Utility bundles: javax-mail, threeten, opentelemetry, brotli4j, perfmark, re2j, lz4, lzma, jzlib |
| `karaf-features-antlr/` | feature | ANTLR parser generators (v2, v3, v4) and StringTemplate |
| `karaf-features-bouncycastle/` | feature | BouncyCastle cryptography (versions 1.69 through 1.81) |
| `karaf-features-jackson/` | feature | Jackson JSON: core, JAX-RS provider, JAX-WS data binding, data type modules |
| `karaf-features-eclipse-emf/` | feature | Eclipse Modeling Framework (versions 2.12 through 2.39) |
| `karaf-features-javassist/` | feature | Javassist bytecode engineering library |
| `karaf-features-tinybundles/` | feature | TinyBundles for programmatic OSGi bundle creation |
| `karaf-features-subethamail/` | feature | SubEtha Mail embedded SMTP server |
| `karaf-features-jdbc/` | feature | JDBC drivers: H2, PostgreSQL, Oracle (11.2–18.3), HSQLDB (2.3–2.7) |

### Mid-level (depend on foundational modules)

| Module | Type | Purpose |
|--------|------|---------|
| `karaf-features-apache-commons/` | feature | 30+ Apache Commons libraries (beanutils, codec, collections, compress, io, lang3, csv, etc.) |
| `karaf-features-apache-httpclient/` | feature | Apache HttpClient 3 and 4; depends on apache-commons for codec |
| `karaf-features-apache-arrow/` | feature | Apache Arrow 17; depends on apache-commons (codec) and jackson (core) |
| `karaf-features-apache-poi/` | feature | Apache POI 3/4/5; depends on bouncycastle and apache-commons |

### High-level (complex dependency chains)

| Module | Type | Purpose |
|--------|------|---------|
| `karaf-features-google/` | feature | Google ecosystem: Guava (18–33), Guice (4/5/7), Gson, gRPC, BigQuery, Ads, Cloud APIs; depends on common, httpclient, jackson, bouncycastle, arrow |
| `karaf-features-jxls/` | feature | JXLS 2/3 Excel template engine; depends on apache-commons and apache-poi |
| `karaf-features-jasperreports/` | feature | JasperReports 6/7; depends on jackson, apache-commons, google (guava) |
| `karaf-features-xdocreport/` | feature | XDocReport with DOCX/ODT converters, PDF export, template engines (Freemarker, Velocity); depends on apache-commons, httpclient, poi, antlr |
| `karaf-features-eclipse-epsilon/` | feature | Eclipse Epsilon model transformation; depends on jackson, google, antlr, commons, poi, emf |
| `karaf-features-eclipse-xtext/` | feature | Eclipse Xtext DSL framework; depends on eclipse-emf, google (guava, guice), antlr |

## Technology Stack

### Core Technologies
- Apache Karaf 4.4.7 — target OSGi container
- OSGi Feature Descriptors — `feature.xml` format for declaring bundles
- Maven Feature Packaging — `<packaging>feature</packaging>` via karaf-maven-plugin

### Build & Quality
- Maven 3.9.4+ with Maven Wrapper
- karaf-maven-plugin 4.4.7 — feature generation and verification
- flatten-maven-plugin 1.3.0 — CI-friendly `${revision}` version resolution
- build-helper-maven-plugin 3.3.0 — source attachment
- sign-maven-plugin 1.1.0 — GPG artifact signing
- nexus-staging-maven-plugin 1.6.13 — deployment to Sonatype/JUDO Nexus

## Build Commands

```bash
# Full build with feature validation
./mvnw clean install

# Run tests only (feature descriptor verification)
./mvnw clean test

# Build a single module
./mvnw -pl karaf-features-google clean install

# Skip all modules (parent POM only)
./mvnw -DskipModules=true clean install

# Build with custom version
./mvnw -Drevision=2.1.0 clean install

# Release to JUDO Nexus
./mvnw -B -Drevision=2.0.2 -Psign-artifacts -Prelease-judong deploy

# Release to Maven Central
./mvnw -B -Drevision=2.0.2 -DdeployOnly -Prelease-central,sign-artifacts deploy
```

### Maven Profiles

| Profile | Purpose |
|---------|---------|
| `modules` | Activates all 19 feature modules (default, unless `-DskipModules=true`) |
| `sign-artifacts` | GPG-signs artifacts using sign-maven-plugin |
| `release-central` | Deploys to Maven Central via Sonatype OSSRH |
| `release-judong` | Deploys to JUDO Nexus repository |
| `generate-github-asciidoc-diagrams` | Generates PNG diagrams from documentation |
| `update-source-code-license` | Updates Apache 2.0 license headers in source files |

## Key Configuration Files

| File | Purpose |
|------|---------|
| `pom.xml` | Parent POM: version properties (`revision`, `karaf-version`, `jackson-version`), module list, profiles, plugin management |
| `.mvn/jvm.config` | JVM settings for Maven: `-Xms1024m -Xmx2048m -Dfile.encoding=UTF-8` |
| `.mvn/wrapper/maven-wrapper.properties` | Maven Wrapper version configuration |
| `karaf-features-*/pom.xml` | Module POMs: inherit parent, set `<packaging>feature</packaging>` |
| `karaf-features-*/src/main/feature/feature.xml` | OSGi feature descriptors — the primary source files |
| `.github/workflows/build.yml` | Main CI workflow: build, deploy, tag, release |
| `.github/workflows/release.yml` | Manual release workflow |

## Development Environment

**Required:**
- Java 21 JDK (Azul Zulu recommended)
- Maven 3.9.4+ (or use included `./mvnw`)

**Module layout:**
Every module has exactly the same structure: a `pom.xml` with `<packaging>feature</packaging>` and a `src/main/feature/feature.xml`. There is no Java source code to compile — the karaf-maven-plugin generates and verifies feature descriptors from the XML definitions.

## Git Workflow

- **Main Branch:** `develop`
- **Versioning:** CI-friendly `${revision}` property (currently `2.0.2-SNAPSHOT`)
- **Branching:** GitFlow — `develop`, `feature/JNG-xxx`, `release/x.y.z`, `bugfix/JNG-xxx`, `hotfix/JNG-xxx`, `master`
- **Commit Rule:** Every commit must reference a JIRA ticket (`JNG-xxx`)
- **CI:** GitHub Actions on self-hosted `judong` runner, 30-minute timeout
- **Deployment:** JUDO Nexus for all builds, Maven Central for release branches only

## Important Notes

1. The only meaningful source files are `feature.xml` descriptors — editing a module means editing its feature XML
2. Feature verification runs automatically during `mvn test` — it checks that all declared bundles are resolvable against Karaf 4.4.7
3. The `${revision}` property in the parent POM is the single source of truth for the project version; it gets flattened by the flatten-maven-plugin during build
4. `karaf-features-openapi-generator` exists but is commented out in the module list
5. Inter-module dependencies are declared at the feature level (one feature referencing another by name), not at the Maven module level
6. The `jackson-version` property (currently `2.19.2`) is defined centrally but used via `${jackson-version}` in individual feature.xml files through Maven resource filtering

## Related Documentation

- [README.md](README.md) — Module overview with dependency diagram
- [CONTRIBUTING.md](CONTRIBUTING.md) — Development setup and contribution guidelines
- [.github/CIFLOW.md](.github/CIFLOW.md) — CI/CD workflow documentation with diagrams
- [LICENSE.txt](LICENSE.txt) — Apache License 2.0
