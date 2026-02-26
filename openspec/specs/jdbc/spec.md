# JDBC Drivers Karaf Features Specification

## Purpose
Provides OSGi Karaf feature descriptors for deploying various JDBC database drivers -- H2, PostgreSQL, Oracle (multiple versions), and HSQLDB (multiple versions) -- into an Apache Karaf container, using the wrap protocol for non-OSGi driver JARs.

## Architecture
The module defines the `jdbc-${project.version}` feature repository containing 14 features organized by database: jdbc-h2 (1.4.200), jdbc-postgresql (42.7.6), Oracle drivers spanning versions 11.2 through 18.3 (with a jdbc-oracle alias), and HSQLDB drivers spanning versions 2.3 through 2.7 (with a jdbc-hsqldb alias). PostgreSQL and Oracle features use the Karaf wrap protocol to convert non-OSGi JARs into bundles, requiring the wrap and/or transaction-api prerequisite features.

## Requirements

### Requirement: Provide H2 JDBC driver as native OSGi bundle
The jdbc-h2 feature SHALL provide the com.h2database/h2/1.4.200 bundle without requiring the wrap protocol.

#### Scenario: Installing H2 driver deploys the H2 bundle
- **GIVEN** an Apache Karaf container with the jdbc feature repository registered
- **WHEN** the jdbc-h2 feature is installed
- **THEN** the bundle mvn:com.h2database/h2/1.4.200 SHALL be deployed

### Requirement: Provide PostgreSQL JDBC driver with wrap and transaction-api prerequisites
The jdbc-postgresql feature SHALL use the wrap protocol for the postgresql/42.7.6 driver JAR and SHALL declare wrap and transaction-api as prerequisite dependencies.

#### Scenario: Installing PostgreSQL driver wraps the JDBC JAR
- **GIVEN** an Apache Karaf container with the jdbc feature repository registered
- **WHEN** the jdbc-postgresql feature is installed
- **THEN** the wrap and transaction-api features SHALL be installed as prerequisites and the PostgreSQL 42.7.6 driver SHALL be deployed as a wrapped bundle with Bundle-SymbolicName=org.postgresql

### Requirement: Provide Oracle JDBC drivers for versions 11.2, 12.1, 12.2, and 18.3
The feature repository SHALL provide jdbc-oracle_11_2 (ojdbc6/11.2.0.4), jdbc-oracle_12_1 (ojdbc7/12.1.0.2), jdbc-oracle_12_2 (ojdbc8/12.2.0.1), and jdbc-oracle_18_3 (ojdbc8/18.3.0.0) features, all using the wrap protocol with the wrap prerequisite feature.

#### Scenario: Installing Oracle 18.3 driver wraps the JDBC JAR
- **GIVEN** an Apache Karaf container with the jdbc feature repository registered
- **WHEN** the jdbc-oracle_18_3 feature is installed
- **THEN** the wrap feature SHALL be installed as a prerequisite and the Oracle ojdbc8/18.3.0.0 driver SHALL be deployed as a wrapped bundle with Bundle-SymbolicName=oracle

### Requirement: Provide Oracle JDBC alias pointing to version 18.3
The jdbc-oracle feature SHALL declare a dependency on jdbc-oracle_18_3, acting as the default Oracle driver alias.

#### Scenario: Installing jdbc-oracle installs Oracle 18.3
- **GIVEN** an Apache Karaf container with the jdbc feature repository registered
- **WHEN** the jdbc-oracle feature is installed
- **THEN** the jdbc-oracle_18_3 feature SHALL also be installed

### Requirement: Provide HSQLDB drivers for versions 2.3 through 2.7
The feature repository SHALL provide jdbc-hsqldb_2_3 (2.3.6), jdbc-hsqldb_2_4 (2.4.1), jdbc-hsqldb_2_5 (2.5.2), jdbc-hsqldb_2_6 (2.6.0), and jdbc-hsqldb_2_7 (2.7.1) features, each providing the hsqldb bundle directly.

#### Scenario: Installing HSQLDB 2.7 deploys the HSQLDB bundle
- **GIVEN** an Apache Karaf container with the jdbc feature repository registered
- **WHEN** the jdbc-hsqldb_2_7 feature is installed
- **THEN** the bundle mvn:org.hsqldb/hsqldb/2.7.1 SHALL be deployed

### Requirement: Provide HSQLDB alias pointing to version 2.7
The jdbc-hsqldb feature SHALL declare a dependency on jdbc-hsqldb_2_7, acting as the default HSQLDB driver alias.

#### Scenario: Installing jdbc-hsqldb installs HSQLDB 2.7
- **GIVEN** an Apache Karaf container with the jdbc feature repository registered
- **WHEN** the jdbc-hsqldb feature is installed
- **THEN** the jdbc-hsqldb_2_7 feature SHALL also be installed
