# Eclipse Xtext Karaf Features Specification

## Purpose
Provides OSGi Karaf feature descriptors for deploying the Eclipse Xtext language framework runtime into an Apache Karaf container, including an Eclipse Equinox compatibility layer.

## Architecture
The module defines the `eclipse-xtext-${project.version}` feature repository containing four features: eclipse-compatibility (providing Equinox supplement), an eclipse-xtext alias (defaulting to 2_39), and two versioned features -- eclipse-xtext-2_29 (version 2.29.0) and eclipse-xtext-2_39 (version 2.39.0). Both versioned features share a common dependency set: eclipse-compatibility, eclipse-emf, guava-30, guice-5, antlr3, and pax-cdi-core. Each versioned feature deploys ASM, ClassGraph, and BlackBelt-wrapped Xtext/Xbase bundles.

## Requirements

### Requirement: Provide Eclipse Equinox compatibility feature
The eclipse-compatibility feature SHALL provide the org.eclipse.equinox.supplement/1.10.600 bundle to ensure Eclipse platform compatibility in Karaf.

#### Scenario: Installing Eclipse compatibility deploys Equinox supplement
- **GIVEN** an Apache Karaf container with the eclipse-xtext feature repository registered
- **WHEN** the eclipse-compatibility feature is installed
- **THEN** the bundle mvn:org.eclipse.platform/org.eclipse.equinox.supplement/1.10.600 SHALL be deployed

### Requirement: Provide Eclipse Xtext alias pointing to version 2.39
The eclipse-xtext feature SHALL declare a dependency on the eclipse-xtext-2_39 feature, acting as an alias to the current default version.

#### Scenario: Installing eclipse-xtext installs the 2.39 version
- **GIVEN** an Apache Karaf container with the eclipse-xtext feature repository registered
- **WHEN** the eclipse-xtext feature is installed
- **THEN** the eclipse-xtext-2_39 feature SHALL also be installed

### Requirement: Declare all required feature dependencies for Xtext 2.39
The eclipse-xtext-2_39 feature SHALL declare dependencies on eclipse-compatibility, eclipse-emf, guava-30, guice-5, antlr3, and pax-cdi-core features.

#### Scenario: Installing Xtext 2.39 pulls in all required features
- **GIVEN** an Apache Karaf container with the eclipse-xtext feature repository registered
- **WHEN** the eclipse-xtext-2_39 feature is installed
- **THEN** the eclipse-compatibility, eclipse-emf, guava-30, guice-5, antlr3, and pax-cdi-core features SHALL also be installed

### Requirement: Provide Xtext 2.39 runtime bundles
The eclipse-xtext-2_39 feature SHALL provide ASM/9.8, classgraph/4.8.43, jakarta.inject-api/2.0.1, and BlackBelt-wrapped Xtext/2.39.0_1 and Xbase/2.39.0_1 bundles.

#### Scenario: Installing Xtext 2.39 deploys all runtime bundles
- **GIVEN** an Apache Karaf container with the eclipse-xtext feature repository registered
- **WHEN** the eclipse-xtext-2_39 feature is installed
- **THEN** the bundles asm/9.8, classgraph/4.8.43, jakarta.inject-api/2.0.1, org.eclipse.xtext/2.39.0_1, and org.eclipse.xbase/2.39.0_1 SHALL be deployed

### Requirement: Provide Xtext 2.29 with version-appropriate bundles
The eclipse-xtext-2_29 feature SHALL provide ASM/9.4, classgraph/4.8.43, and BlackBelt-wrapped Xtext/2.29.0_1 and Xbase/2.29.0_1 bundles with the same feature dependencies as 2.39.

#### Scenario: Installing Xtext 2.29 deploys older version bundles
- **GIVEN** an Apache Karaf container with the eclipse-xtext feature repository registered
- **WHEN** the eclipse-xtext-2_29 feature is installed
- **THEN** the bundles asm/9.4, classgraph/4.8.43, org.eclipse.xtext/2.29.0_1, and org.eclipse.xbase/2.29.0_1 SHALL be deployed
