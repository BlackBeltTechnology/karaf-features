# JasperReports Karaf Features Specification

## Purpose
Provides OSGi Karaf feature descriptors for deploying JasperReports document generation library (versions 6 and 7) into an Apache Karaf container, with a shared commons feature for rendering dependencies.

## Architecture
The module defines the `jasperreports-${project.version}` feature repository containing four features: a jasperreports alias (defaulting to jasperreports7), jasperreports6 (version 6.20.5), jasperreports7 (version 7.0.3), and jasperreports-commons (shared rendering and utility dependencies). Both versioned features depend on jasperreports-commons, jackson-jaxrs, and jackson-jaxws. The jasperreports-commons feature provides a rich set of dependencies including Apache Commons libraries, Guava, ICU4J, Xalan, Batik, Xerces, and Groovy bundles.

## Requirements

### Requirement: Provide JasperReports alias pointing to version 7
The jasperreports feature SHALL declare a dependency on the jasperreports7 feature, acting as an alias to the current default version.

#### Scenario: Installing jasperreports installs version 7
- **GIVEN** an Apache Karaf container with the jasperreports feature repository registered
- **WHEN** the jasperreports feature is installed
- **THEN** the jasperreports7 feature SHALL also be installed

### Requirement: Provide JasperReports 6 with commons and Jackson dependencies
The jasperreports6 feature SHALL depend on jasperreports-commons, jackson-jaxrs, and jackson-jaxws features, and SHALL provide the BlackBelt-wrapped jasperreports/6.20.5_2 bundle.

#### Scenario: Installing JasperReports 6 pulls in all required features
- **GIVEN** an Apache Karaf container with the jasperreports feature repository registered
- **WHEN** the jasperreports6 feature is installed
- **THEN** the jasperreports-commons, jackson-jaxrs, and jackson-jaxws features SHALL be installed and the bundle mvn:hu.blackbelt.bundles.jasperreports/net.sf.jasperreports/6.20.5_2 SHALL be deployed

### Requirement: Provide JasperReports 7 with commons and Jackson dependencies
The jasperreports7 feature SHALL depend on jasperreports-commons, jackson-jaxrs, and jackson-jaxws features, and SHALL provide the BlackBelt-wrapped jasperreports/7.0.3_1 bundle.

#### Scenario: Installing JasperReports 7 pulls in all required features
- **GIVEN** an Apache Karaf container with the jasperreports feature repository registered
- **WHEN** the jasperreports7 feature is installed
- **THEN** the jasperreports-commons, jackson-jaxrs, and jackson-jaxws features SHALL be installed and the bundle mvn:hu.blackbelt.bundles.jasperreports/net.sf.jasperreports/7.0.3_1 SHALL be deployed

### Requirement: Provide shared commons feature with rendering and utility dependencies
The jasperreports-commons feature SHALL depend on apache-commons-collections4, apache-commons-pool2, apache-commons-beanutils, apache-commons-logging, apache-commons-bcel, and guava-30 features, and SHALL provide ICU4J/73.2, Xalan/2.7.3_3, Batik/1.16_1, Xerces/2.12.2_1, XMLResolver/1.2_5, and Groovy/2.5.0 (core, json, templates) bundles.

#### Scenario: Installing jasperreports-commons deploys rendering infrastructure
- **GIVEN** an Apache Karaf container with the jasperreports feature repository registered
- **WHEN** the jasperreports-commons feature is installed
- **THEN** the six dependency features SHALL be installed and all rendering bundles (ICU4J, Xalan, Batik, Xerces, XMLResolver, Groovy) SHALL be deployed
