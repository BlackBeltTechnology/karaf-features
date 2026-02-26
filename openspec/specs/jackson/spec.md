# Jackson Karaf Features Specification

## Purpose
Provides OSGi Karaf feature descriptors for deploying Jackson JSON processing libraries (core, JAX-RS, JAX-WS, and additional data types) into an Apache Karaf container with proper dependency wiring and start-level configuration.

## Architecture
The module defines the `jackson-${project.version}` feature repository containing four features: jackson-core, jackson-jaxrs, jackson-jaxws, and jackson-datatypes. Features form a dependency hierarchy where jackson-core is the base, jackson-jaxrs and jackson-jaxws depend on jackson-core, and jackson-datatypes depends on jackson-jaxrs. Bundle start levels are configured at level 10 for API specs and level 35 for implementation bundles.

## Requirements

### Requirement: Provide Jackson core bundles at start-level 35
The jackson-core feature SHALL provide jackson-core, jackson-annotations, jackson-databind, and jackson-datatype-jsr310 bundles all at version ${jackson-version} with start-level 35.

#### Scenario: Installing Jackson core deploys all core bundles
- **GIVEN** an Apache Karaf container with the jackson feature repository registered
- **WHEN** the jackson-core feature is installed
- **THEN** the bundles jackson-core, jackson-annotations, jackson-databind, and jackson-datatype-jsr310 from com.fasterxml.jackson SHALL be deployed at start-level 35

### Requirement: Provide Jackson JAX-RS support with core dependency
The jackson-jaxrs feature SHALL depend on jackson-core and SHALL provide the JAX-RS 2.1 API spec bundle at start-level 10 and the jackson-jaxrs-base and jackson-jaxrs-json-provider bundles at start-level 35.

#### Scenario: Installing Jackson JAX-RS pulls in core and API spec
- **GIVEN** an Apache Karaf container with the jackson feature repository registered
- **WHEN** the jackson-jaxrs feature is installed
- **THEN** the jackson-core feature SHALL be installed, the servicemix jaxrs-api-2.1 bundle SHALL be deployed at start-level 10, and jackson-jaxrs-base and jackson-jaxrs-json-provider bundles SHALL be deployed at start-level 35

### Requirement: Provide Jackson JAX-WS XML support with core dependency
The jackson-jaxws feature SHALL depend on jackson-core and SHALL provide the jackson-dataformat-xml bundle along with StAX API, stax2-api, and JAX-WS API dependency bundles.

#### Scenario: Installing Jackson JAX-WS deploys XML format support
- **GIVEN** an Apache Karaf container with the jackson feature repository registered
- **WHEN** the jackson-jaxws feature is installed
- **THEN** the jackson-core feature SHALL be installed and the bundle mvn:com.fasterxml.jackson.dataformat/jackson-dataformat-xml/${jackson-version} SHALL be deployed at start-level 35

### Requirement: Provide Jackson DataTypes with JAX-RS dependency
The jackson-datatypes feature SHALL depend on jackson-jaxrs and SHALL provide modules for parameter-names, jdk8, jsr310, joda, and jsr353 data types along with joda-time/2.14.0 and javax.json/1.1.4 bundles.

#### Scenario: Installing Jackson DataTypes pulls in JAX-RS and supplemental bundles
- **GIVEN** an Apache Karaf container with the jackson feature repository registered
- **WHEN** the jackson-datatypes feature is installed
- **THEN** the jackson-jaxrs feature SHALL be installed, and all data type modules (parameter-names, jdk8, jsr310, joda, jsr353) plus joda-time and javax.json bundles SHALL be deployed
