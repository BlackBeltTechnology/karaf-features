# Apache Arrow Karaf Features Specification

## Purpose
Provides an OSGi Karaf feature descriptor for deploying Apache Arrow 17.0.0 columnar data processing library into an Apache Karaf container with its required dependencies.

## Architecture
The module defines the `apache-arrow-${project.version}` feature repository containing a single feature: apache-arrow (version 17.0.0). This feature depends on the apache-commons-codec and jackson-core features from other karaf-features modules, and bundles FindBugs JSR-305 annotations, Google FlatBuffers, and the BlackBelt-wrapped Arrow bundle.

## Requirements

### Requirement: Provide Apache Arrow bundle with codec and Jackson dependencies
The apache-arrow feature SHALL provide the Arrow 17.0.0 bundle and SHALL declare dependencies on the apache-commons-codec and jackson-core features.

#### Scenario: Installing Arrow pulls in Codec and Jackson Core
- **GIVEN** an Apache Karaf container with the apache-arrow feature repository registered
- **WHEN** the apache-arrow feature is installed
- **THEN** the bundle mvn:hu.blackbelt.bundles.arrow/org.apache.arrow/17.0.0_1 SHALL be deployed and the apache-commons-codec and jackson-core features SHALL also be installed

### Requirement: Provide supplemental annotation and serialization bundles
The apache-arrow feature SHALL provide the FindBugs JSR-305 annotations bundle (jsr305/3.0.2) and Google FlatBuffers bundle (flatbuffers-java/24.12.23) alongside the Arrow bundle.

#### Scenario: Arrow feature deploys JSR-305 and FlatBuffers bundles
- **GIVEN** an Apache Karaf container with the apache-arrow feature repository registered
- **WHEN** the apache-arrow feature is installed
- **THEN** the bundles mvn:com.google.code.findbugs/jsr305/3.0.2 and mvn:com.google.flatbuffers/flatbuffers-java/24.12.23 SHALL be deployed

### Requirement: Feature defaults to manual installation
The apache-arrow feature SHALL declare install="false".

#### Scenario: Feature auto-install is disabled
- **GIVEN** the apache-arrow feature repository XML is parsed by Karaf
- **WHEN** the feature definitions are loaded
- **THEN** the apache-arrow feature SHALL have its install attribute set to "false"
