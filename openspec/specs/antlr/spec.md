# ANTLR Karaf Features Specification

## Purpose
Provides OSGi Karaf feature descriptors for deploying multiple versions of the ANTLR parser generator runtime and the StringTemplate templating engine into an Apache Karaf container.

## Architecture
The module defines four independent features in the `antlr-${project.version}` feature repository: antlr2 (version 2.7), antlr3 (version 3.4), antlr4 (version 4.5.1), and stringtemplate3 (version 3.2). Each feature is self-contained with no inter-feature dependencies, wrapping upstream Maven artifacts as OSGi bundles.

## Requirements

### Requirement: Provide ANTLR 2 runtime bundle
The antlr2 feature SHALL provide the ANTLR 2.7.7 runtime via the ServiceMix-wrapped bundle org.apache.servicemix.bundles.antlr/2.7.7_3.

#### Scenario: Installing ANTLR 2 feature
- **GIVEN** an Apache Karaf container with the antlr feature repository registered
- **WHEN** the antlr2 feature is installed
- **THEN** the bundle mvn:org.apache.servicemix.bundles/org.apache.servicemix.bundles.antlr/2.7.7_3 SHALL be deployed and active

### Requirement: Provide ANTLR 3 runtime bundle
The antlr3 feature SHALL provide the ANTLR 3.4 runtime via the ServiceMix-wrapped bundle org.apache.servicemix.bundles.antlr/3.4_1.

#### Scenario: Installing ANTLR 3 feature
- **GIVEN** an Apache Karaf container with the antlr feature repository registered
- **WHEN** the antlr3 feature is installed
- **THEN** the bundle mvn:org.apache.servicemix.bundles/org.apache.servicemix.bundles.antlr/3.4_1 SHALL be deployed and active

### Requirement: Provide ANTLR 4 runtime bundle
The antlr4 feature SHALL provide the ANTLR 4.5.1 runtime via the bundle org.antlr/antlr4-runtime/4.5.1-1.

#### Scenario: Installing ANTLR 4 feature
- **GIVEN** an Apache Karaf container with the antlr feature repository registered
- **WHEN** the antlr4 feature is installed
- **THEN** the bundle mvn:org.antlr/antlr4-runtime/4.5.1-1 SHALL be deployed and active

### Requirement: Provide StringTemplate 3 runtime bundle
The stringtemplate3 feature SHALL provide the StringTemplate 3.2 library via the ServiceMix-wrapped bundle org.apache.servicemix.bundles.stringtemplate/3.2_5.

#### Scenario: Installing StringTemplate 3 feature
- **GIVEN** an Apache Karaf container with the antlr feature repository registered
- **WHEN** the stringtemplate3 feature is installed
- **THEN** the bundle mvn:org.apache.servicemix.bundles/org.apache.servicemix.bundles.stringtemplate/3.2_5 SHALL be deployed and active

### Requirement: All features default to manual installation
All four features (antlr2, antlr3, antlr4, stringtemplate3) SHALL declare install="false" so they are not auto-installed.

#### Scenario: Feature auto-install is disabled
- **GIVEN** the antlr feature repository XML is parsed by Karaf
- **WHEN** the feature definitions are loaded
- **THEN** each feature SHALL have its install attribute set to "false"
