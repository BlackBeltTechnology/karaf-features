# Eclipse EMF Karaf Features Specification

## Purpose
Provides OSGi Karaf feature descriptors for deploying multiple versions of the Eclipse Modeling Framework (EMF) Ecore runtime into an Apache Karaf container, covering versions from 2.12 through 2.39.

## Architecture
The module defines the `eclipse-emf-${project.version}` feature repository containing 12 features: an eclipse-emf alias (defaulting to 2.39) and 11 versioned features (eclipse-emf-2.12 through eclipse-emf-2.39). Each versioned feature provides four EMF bundles: org.eclipse.emf.common, org.eclipse.emf.ecore, org.eclipse.emf.ecore.xmi, and org.eclipse.emf.mapping.ecore2xml at version-appropriate coordinates. No inter-feature dependencies exist between versioned features.

## Requirements

### Requirement: Provide Eclipse EMF alias pointing to default version 2.39
The eclipse-emf feature SHALL declare a dependency on the eclipse-emf-2.39 feature, acting as an alias to the current default version.

#### Scenario: Installing eclipse-emf installs the 2.39 version
- **GIVEN** an Apache Karaf container with the eclipse-emf feature repository registered
- **WHEN** the eclipse-emf feature is installed
- **THEN** the eclipse-emf-2.39 feature SHALL also be installed

### Requirement: Provide four core EMF bundles in each versioned feature
Each versioned eclipse-emf feature SHALL provide exactly four bundles: org.eclipse.emf.common, org.eclipse.emf.ecore, org.eclipse.emf.ecore.xmi, and org.eclipse.emf.mapping.ecore2xml at version-appropriate coordinates.

#### Scenario: Installing eclipse-emf-2.39 deploys all four EMF bundles
- **GIVEN** an Apache Karaf container with the eclipse-emf feature repository registered
- **WHEN** the eclipse-emf-2.39 feature is installed
- **THEN** the bundles org.eclipse.emf.common/2.42.0, org.eclipse.emf.ecore/2.39.0, org.eclipse.emf.ecore.xmi/2.39.0, and org.eclipse.emf.mapping.ecore2xml/2.13.0 SHALL be deployed

### Requirement: Provide version range coverage from 2.12 to 2.39
The feature repository SHALL provide versioned features for EMF 2.12, 2.15, 2.18, 2.21, 2.27, 2.34, 2.35, 2.36, 2.37, 2.38, and 2.39 so consumers can select the appropriate version for their EMF model compatibility needs.

#### Scenario: Installing eclipse-emf-2.12 deploys the earliest supported version
- **GIVEN** an Apache Karaf container with the eclipse-emf feature repository registered
- **WHEN** the eclipse-emf-2.12 feature is installed
- **THEN** the bundles org.eclipse.emf.common/2.12.0, org.eclipse.emf.ecore/2.12.0, org.eclipse.emf.ecore.xmi/2.12.0, and org.eclipse.emf.mapping.ecore2xml/2.9.0 SHALL be deployed

### Requirement: Versioned features are self-contained
Each versioned eclipse-emf feature SHALL have no inter-feature dependencies, allowing any single version to be installed independently.

#### Scenario: Versioned EMF features declare no feature dependencies
- **GIVEN** the eclipse-emf feature repository XML is parsed by Karaf
- **WHEN** the versioned feature definitions (eclipse-emf-2.12 through eclipse-emf-2.39) are loaded
- **THEN** none of the versioned features SHALL declare any feature dependency elements
