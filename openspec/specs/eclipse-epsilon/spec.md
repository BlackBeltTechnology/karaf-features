# Eclipse Epsilon Karaf Features Specification

## Purpose
Provides OSGi Karaf feature descriptors for deploying the Eclipse Epsilon model management platform into an Apache Karaf container with all required runtime dependencies.

## Architecture
The module defines the `eclipse-epsilon-${project.version}` feature repository containing two features: an eclipse-epsilon alias (defaulting to eclipse-epsilon-2) and the eclipse-epsilon-2 feature (version 2.5.0). The eclipse-epsilon-2 feature has a wide dependency graph spanning multiple karaf-features modules: jackson-jaxrs, guava (versions 20 and 30), antlr3, multiple Apache Commons features, apache-poi4, stringtemplate3, and eclipse-emf. It deploys the BlackBelt-wrapped Epsilon 2.8.0 bundle along with jakarta.annotation-api and snakeyaml.

## Requirements

### Requirement: Provide Eclipse Epsilon alias pointing to version 2
The eclipse-epsilon feature SHALL declare a dependency on the eclipse-epsilon-2 feature, acting as an alias.

#### Scenario: Installing eclipse-epsilon installs the version 2 feature
- **GIVEN** an Apache Karaf container with the eclipse-epsilon feature repository registered
- **WHEN** the eclipse-epsilon feature is installed
- **THEN** the eclipse-epsilon-2 feature SHALL also be installed

### Requirement: Declare all required feature dependencies for Epsilon 2
The eclipse-epsilon-2 feature SHALL declare dependencies on jackson-jaxrs, guava-20, guava-30, antlr3, apache-commons-collections, apache-commons-csv, apache-commons-lang3, apache-commons-math3, apache-poi4, stringtemplate3, and eclipse-emf features.

#### Scenario: Installing Epsilon 2 pulls in all required features
- **GIVEN** an Apache Karaf container with the eclipse-epsilon feature repository registered
- **WHEN** the eclipse-epsilon-2 feature is installed
- **THEN** all eleven dependency features (jackson-jaxrs, guava-20, guava-30, antlr3, apache-commons-collections, apache-commons-csv, apache-commons-lang3, apache-commons-math3, apache-poi4, stringtemplate3, eclipse-emf) SHALL also be installed

### Requirement: Provide Epsilon runtime bundle and supplemental libraries
The eclipse-epsilon-2 feature SHALL provide the BlackBelt-wrapped Epsilon bundle (org.eclipse.epsilon/2.8.0_1), the jakarta.annotation-api/1.3.5 bundle at start-level 10, and the snakeyaml/2.2 bundle at start-level 35.

#### Scenario: Epsilon feature deploys runtime and annotation bundles
- **GIVEN** an Apache Karaf container with the eclipse-epsilon feature repository registered
- **WHEN** the eclipse-epsilon-2 feature is installed
- **THEN** the bundle mvn:hu.blackbelt.bundles.eclipse-epsilon/org.eclipse.epsilon/2.8.0_1 SHALL be deployed along with jakarta.annotation-api/1.3.5 at start-level 10 and snakeyaml/2.2 at start-level 35
