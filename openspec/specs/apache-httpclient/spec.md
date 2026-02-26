# Apache HTTPClient Karaf Features Specification

## Purpose
Provides OSGi Karaf feature descriptors for deploying Apache HttpClient versions 3.x and 4.x into an Apache Karaf container as properly wired OSGi bundles.

## Architecture
The module defines the `apache-httpclient-${project.version}` feature repository containing two features: apache-httpclient3 (version 3.1.2) and apache-httpclient4 (version 4.5.14). The httpclient3 feature depends on apache-commons-codec, while httpclient4 depends on the Karaf http feature as a prerequisite and deploys both httpcore-osgi and httpclient-osgi bundles.

## Requirements

### Requirement: Provide Apache HttpClient 3 with codec dependency
The apache-httpclient3 feature SHALL provide the Geronimo-bundled commons-httpclient/3.1_2 and SHALL declare a dependency on the apache-commons-codec feature.

#### Scenario: Installing HttpClient 3 pulls in Codec
- **GIVEN** an Apache Karaf container with the apache-httpclient feature repository registered
- **WHEN** the apache-httpclient3 feature is installed
- **THEN** the bundle mvn:org.apache.geronimo.bundles/commons-httpclient/3.1_2 SHALL be deployed and the apache-commons-codec feature SHALL also be installed

### Requirement: Provide Apache HttpClient 4 with HTTP prerequisite
The apache-httpclient4 feature SHALL provide httpcore-osgi/4.4.16 and httpclient-osgi/4.5.14 bundles and SHALL declare the http feature as a prerequisite dependency.

#### Scenario: Installing HttpClient 4 ensures HTTP feature is available first
- **GIVEN** an Apache Karaf container with the apache-httpclient feature repository registered
- **WHEN** the apache-httpclient4 feature is installed
- **THEN** the http feature SHALL be installed as a prerequisite before the bundles mvn:org.apache.httpcomponents/httpcore-osgi/4.4.16 and mvn:org.apache.httpcomponents/httpclient-osgi/4.5.14 are deployed

### Requirement: All features default to manual installation
Both apache-httpclient3 and apache-httpclient4 features SHALL declare install="false".

#### Scenario: Feature auto-install is disabled
- **GIVEN** the apache-httpclient feature repository XML is parsed by Karaf
- **WHEN** the feature definitions are loaded
- **THEN** each feature SHALL have its install attribute set to "false"
