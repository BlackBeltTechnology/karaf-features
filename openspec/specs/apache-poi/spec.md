# Apache POI Karaf Features Specification

## Purpose
Provides OSGi Karaf feature descriptors for deploying multiple major versions of Apache POI (versions 3, 4, and 5) into an Apache Karaf container, with appropriate dependency features for each version.

## Architecture
The module defines the `apache-poi-${project.version}` feature repository containing three features: apache-poi3 (version 3.17), apache-poi4 (version 4.1.2), and apache-poi5 (version 5.4.1). Each feature depends on bouncycastle, apache-commons-codec, and apache-commons-collections4 as common dependencies, with increasing additional dependencies for newer versions. All POI bundles are provided via BlackBelt-wrapped artifacts (hu.blackbelt.bundles.poi/org.apache.poi).

## Requirements

### Requirement: Provide Apache POI 3 with minimal dependencies
The apache-poi3 feature SHALL provide the POI 3.17 bundle and SHALL declare dependencies on bouncycastle, apache-xmlbeans2, apache-commons-codec, and apache-commons-collections4 features.

#### Scenario: Installing POI 3 deploys correct bundle and dependencies
- **GIVEN** an Apache Karaf container with the apache-poi feature repository registered
- **WHEN** the apache-poi3 feature is installed
- **THEN** the bundle mvn:hu.blackbelt.bundles.poi/org.apache.poi/3.17_2 SHALL be deployed and the bouncycastle, apache-xmlbeans2, apache-commons-codec, and apache-commons-collections4 features SHALL also be installed

### Requirement: Provide Apache POI 4 with extended dependencies
The apache-poi4 feature SHALL provide the POI 4.1.2 bundle and SHALL declare dependencies on bouncycastle, apache-xmlbeans3, apache-commons-codec, apache-commons-collections4, apache-commons-compress, apache-commons-math3, apache-commons-bcel, and apache-commons-io features.

#### Scenario: Installing POI 4 deploys correct bundle and all dependencies
- **GIVEN** an Apache Karaf container with the apache-poi feature repository registered
- **WHEN** the apache-poi4 feature is installed
- **THEN** the bundle mvn:hu.blackbelt.bundles.poi/org.apache.poi/4.1.2_3 SHALL be deployed and all eight dependency features SHALL also be installed

### Requirement: Provide Apache POI 5 with XMLBeans 5 and additional rendering bundles
The apache-poi5 feature SHALL provide the POI 5.4.1 bundle along with xalan, batik, xerces, and xmlresolver ServiceMix bundles, and SHALL declare dependencies on bouncycastle, apache-xmlbeans5, apache-commons-codec, apache-commons-collections4, apache-commons-compress, apache-commons-math3, apache-commons-bcel, and apache-commons-io features.

#### Scenario: Installing POI 5 deploys rendering bundles alongside POI
- **GIVEN** an Apache Karaf container with the apache-poi feature repository registered
- **WHEN** the apache-poi5 feature is installed
- **THEN** the bundle mvn:hu.blackbelt.bundles.poi/org.apache.poi/5.4.1_2 SHALL be deployed along with xalan/2.7.2_2, batik/1.16_1, xerces/2.12.2_1, and xmlresolver/1.2_5 ServiceMix bundles

### Requirement: POI version features use version-appropriate XMLBeans
The apache-poi3 feature SHALL depend on apache-xmlbeans2, apache-poi4 SHALL depend on apache-xmlbeans3, and apache-poi5 SHALL depend on apache-xmlbeans5.

#### Scenario: Each POI version uses the correct XMLBeans dependency
- **GIVEN** the apache-poi feature repository XML is parsed by Karaf
- **WHEN** the feature definitions are loaded
- **THEN** apache-poi3 SHALL reference apache-xmlbeans2, apache-poi4 SHALL reference apache-xmlbeans3, and apache-poi5 SHALL reference apache-xmlbeans5
