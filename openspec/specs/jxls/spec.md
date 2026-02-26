# JXLS Karaf Features Specification

## Purpose
Provides OSGi Karaf feature descriptors for deploying JXLS Excel report generation library (versions 2 and 3) into an Apache Karaf container, with dependencies on Apache POI and JEXL.

## Architecture
The module defines the `jxls-${project.version}` feature repository containing three features: a jxls alias (defaulting to jxls3), jxls2 (version 2.10.0), and jxls3 (version 3.0.0). Both versioned features depend on apache-jexl3 and apache-commons-beanutils, but differ in their POI dependency: jxls2 uses apache-poi4 while jxls3 uses apache-poi5. JXLS bundles are provided via BlackBelt-wrapped artifacts.

## Requirements

### Requirement: Provide JXLS alias pointing to version 3
The jxls feature SHALL declare a dependency on the jxls3 feature, acting as an alias to the current default version.

#### Scenario: Installing jxls installs the version 3 feature
- **GIVEN** an Apache Karaf container with the jxls feature repository registered
- **WHEN** the jxls feature is installed
- **THEN** the jxls3 feature SHALL also be installed

### Requirement: Provide JXLS 2 with POI 4 and JEXL3 dependencies
The jxls2 feature SHALL depend on apache-jexl3, apache-poi4, and apache-commons-beanutils features, and SHALL provide the JXLS 2.10.0 bundle and logback-core/1.2.3 bundle.

#### Scenario: Installing JXLS 2 pulls in POI 4 and expression language support
- **GIVEN** an Apache Karaf container with the jxls feature repository registered
- **WHEN** the jxls2 feature is installed
- **THEN** the apache-jexl3, apache-poi4, and apache-commons-beanutils features SHALL be installed and the bundles mvn:hu.blackbelt.bundles.jxls/org.jxls/2.10.0_1 and mvn:ch.qos.logback/logback-core/1.2.3 SHALL be deployed

### Requirement: Provide JXLS 3 with POI 5 and JEXL3 dependencies
The jxls3 feature SHALL depend on apache-jexl3, apache-poi5, and apache-commons-beanutils features, and SHALL provide the JXLS 3.0.0 bundle.

#### Scenario: Installing JXLS 3 pulls in POI 5 and expression language support
- **GIVEN** an Apache Karaf container with the jxls feature repository registered
- **WHEN** the jxls3 feature is installed
- **THEN** the apache-jexl3, apache-poi5, and apache-commons-beanutils features SHALL be installed and the bundle mvn:hu.blackbelt.bundles.jxls/org.jxls/3.0.0_1 SHALL be deployed

### Requirement: JXLS 2 and JXLS 3 use different POI versions
The jxls2 feature SHALL depend on apache-poi4 while the jxls3 feature SHALL depend on apache-poi5.

#### Scenario: Each JXLS version uses the appropriate POI dependency
- **GIVEN** the jxls feature repository XML is parsed by Karaf
- **WHEN** the feature definitions are loaded
- **THEN** jxls2 SHALL reference apache-poi4 and jxls3 SHALL reference apache-poi5
