# Apache Commons Karaf Features Specification

## Purpose
Provides OSGi Karaf feature descriptors for deploying a comprehensive set of Apache Commons libraries, Apache XMLBeans, and Apache JEXL into an Apache Karaf container, with proper dependency wiring between features.

## Architecture
The module defines the `apache-commons-${project.version}` feature repository containing 27 features covering Apache Commons libraries (beanutils, collections, io, pool, dbcp2, codec, fileupload, lang, logging, configuration, math, csv, compress, text, rdf, validator, digester), Apache XMLBeans (versions 2, 3, and 5), Apache BCEL, and Apache JEXL3. Features declare inter-feature dependencies to ensure correct classloading order -- for example, apache-commons-beanutils depends on apache-commons-collections, and apache-commons-dbcp2 depends on transaction-api and apache-commons-pool2.

## Requirements

### Requirement: Provide Apache Commons Beanutils with collections dependency
The apache-commons-beanutils feature SHALL provide commons-beanutils/1.11.0 and SHALL declare a dependency on the apache-commons-collections feature.

#### Scenario: Installing Beanutils pulls in Collections
- **GIVEN** an Apache Karaf container with the apache-commons feature repository registered
- **WHEN** the apache-commons-beanutils feature is installed
- **THEN** the bundle mvn:commons-beanutils/commons-beanutils/1.11.0 SHALL be deployed and the apache-commons-collections feature SHALL also be installed

### Requirement: Provide Apache Commons DBCP2 with transaction and pool dependencies
The apache-commons-dbcp2 feature SHALL provide commons-dbcp2/2.13.0 and SHALL declare dependencies on both the transaction-api and apache-commons-pool2 features.

#### Scenario: Installing DBCP2 pulls in transaction-api and pool2
- **GIVEN** an Apache Karaf container with the apache-commons feature repository registered
- **WHEN** the apache-commons-dbcp2 feature is installed
- **THEN** the bundle mvn:org.apache.commons/commons-dbcp2/2.13.0 SHALL be deployed and both the transaction-api and apache-commons-pool2 features SHALL also be installed

### Requirement: Provide Apache Commons Codec with logging dependency
The apache-commons-codec feature SHALL provide commons-codec/1.18.0 and SHALL declare a dependency on the apache-commons-logging feature.

#### Scenario: Installing Codec pulls in Logging
- **GIVEN** an Apache Karaf container with the apache-commons feature repository registered
- **WHEN** the apache-commons-codec feature is installed
- **THEN** the bundle mvn:commons-codec/commons-codec/1.18.0 SHALL be deployed and the apache-commons-logging feature SHALL also be installed

### Requirement: Provide Apache Commons Fileupload with HTTP and IO dependencies
The apache-commons-fileupload feature SHALL provide commons-fileupload/1.6.0 and SHALL declare dependencies on the http, apache-commons-logging, and apache-commons-io features.

#### Scenario: Installing Fileupload pulls in HTTP, Logging, and IO
- **GIVEN** an Apache Karaf container with the apache-commons feature repository registered
- **WHEN** the apache-commons-fileupload feature is installed
- **THEN** the bundle mvn:commons-fileupload/commons-fileupload/1.6.0 SHALL be deployed and the http, apache-commons-logging, and apache-commons-io features SHALL also be installed

### Requirement: Provide Apache Commons Configuration2 with lang3, logging, and text dependencies
The apache-commons-configuration2 feature SHALL provide commons-configuration2/2.8.0 and SHALL declare dependencies on apache-commons-lang3, apache-commons-logging, and apache-commons-text features.

#### Scenario: Installing Configuration2 pulls in required dependencies
- **GIVEN** an Apache Karaf container with the apache-commons feature repository registered
- **WHEN** the apache-commons-configuration2 feature is installed
- **THEN** the bundle mvn:org.apache.commons/commons-configuration2/2.8.0 SHALL be deployed and the apache-commons-lang3, apache-commons-logging, and apache-commons-text features SHALL also be installed

### Requirement: Provide Apache Commons CSV with codec and IO dependencies
The apache-commons-csv feature SHALL provide commons-csv/1.14.0 and SHALL declare dependencies on apache-commons-codec and apache-commons-io features.

#### Scenario: Installing CSV pulls in Codec and IO
- **GIVEN** an Apache Karaf container with the apache-commons feature repository registered
- **WHEN** the apache-commons-csv feature is installed
- **THEN** the bundle mvn:org.apache.commons/commons-csv/1.14.0 SHALL be deployed and the apache-commons-codec and apache-commons-io features SHALL also be installed

### Requirement: Provide Apache Commons Text with lang3 dependency
The apache-commons-text feature SHALL provide commons-text/1.13.1 and SHALL declare a dependency on the apache-commons-lang3 feature.

#### Scenario: Installing Text pulls in Lang3
- **GIVEN** an Apache Karaf container with the apache-commons feature repository registered
- **WHEN** the apache-commons-text feature is installed
- **THEN** the bundle mvn:org.apache.commons/commons-text/1.13.1 SHALL be deployed and the apache-commons-lang3 feature SHALL also be installed

### Requirement: Provide Apache Commons Validator with full dependency chain
The apache-commons-validator feature SHALL provide commons-validator/1.10.0 and SHALL declare dependencies on apache-commons-collections, apache-commons-logging, apache-commons-beanutils, and apache-commons-digester features.

#### Scenario: Installing Validator pulls in all required dependencies
- **GIVEN** an Apache Karaf container with the apache-commons feature repository registered
- **WHEN** the apache-commons-validator feature is installed
- **THEN** the bundle mvn:commons-validator/commons-validator/1.10.0 SHALL be deployed and the apache-commons-collections, apache-commons-logging, apache-commons-beanutils, and apache-commons-digester features SHALL also be installed

### Requirement: Provide standalone library features without inter-feature dependencies
The apache-commons-collections (3.2.2), apache-commons-collections4 (4.5.0), apache-commons-io (2.19.0), apache-commons-pool (1.6), apache-commons-pool2 (2.12.1), apache-commons-logging (1.3.5), apache-commons-math3 (3.6.1), apache-commons-math (2.2), apache-commons-lang3 (3.17.0), apache-commons-compress (1.27.1), apache-commons-bcel (5.2), apache-xmlbeans2 (2.6.0), apache-xmlbeans3 (3.1.0), apache-xmlbeans5 (5.3.0), and apache-jexl3 (3.5.0) features SHALL each provide their respective bundle without declaring inter-feature dependencies.

#### Scenario: Installing a standalone feature deploys only its own bundle
- **GIVEN** an Apache Karaf container with the apache-commons feature repository registered
- **WHEN** the apache-commons-lang3 feature is installed
- **THEN** the bundle mvn:org.apache.commons/commons-lang3/3.17.0 SHALL be deployed without requiring any other feature
