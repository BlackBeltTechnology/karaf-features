# Javassist Karaf Features Specification

## Purpose
Provides an OSGi Karaf feature descriptor for deploying the Javassist bytecode manipulation library into an Apache Karaf container.

## Architecture
The module defines the `javassist-${project.version}` feature repository containing a single feature: javassist (version 3.29.2-GA). The feature is self-contained with no inter-feature dependencies, providing only the javassist bundle directly from the upstream Maven artifact.

## Requirements

### Requirement: Provide Javassist bytecode manipulation bundle
The javassist feature SHALL provide the org.javassist/javassist/3.29.2-GA bundle.

#### Scenario: Installing Javassist deploys the bytecode manipulation library
- **GIVEN** an Apache Karaf container with the javassist feature repository registered
- **WHEN** the javassist feature is installed
- **THEN** the bundle mvn:org.javassist/javassist/3.29.2-GA SHALL be deployed and active

### Requirement: Feature is self-contained with no dependencies
The javassist feature SHALL have no inter-feature dependencies.

#### Scenario: Javassist has no feature prerequisites
- **GIVEN** the javassist feature repository XML is parsed by Karaf
- **WHEN** the javassist feature definition is loaded
- **THEN** it SHALL contain no feature dependency elements

### Requirement: Feature defaults to manual installation
The javassist feature SHALL declare install="false".

#### Scenario: Feature auto-install is disabled
- **GIVEN** the javassist feature repository XML is parsed by Karaf
- **WHEN** the feature definition is loaded
- **THEN** the javassist feature SHALL have its install attribute set to "false"
