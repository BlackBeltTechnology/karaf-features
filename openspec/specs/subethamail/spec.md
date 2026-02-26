# SubEtha Mail Karaf Features Specification

## Purpose
Provides an OSGi Karaf feature descriptor for deploying the SubEtha SMTP server library with its required JavaMail protocol bundles into an Apache Karaf container.

## Architecture
The module defines the `subethamail-${project.version}` feature repository containing a single feature: subethamail (version 3.1.7). The feature is self-contained and bundles the JavaMail protocol libraries directly (rather than depending on the javax-mail feature from the common module), deploying six bundles total: five JavaMail protocol bundles and the BlackBelt-wrapped SubEtha SMTP bundle.

## Requirements

### Requirement: Provide SubEtha SMTP bundle with embedded JavaMail bundles
The subethamail feature SHALL provide the BlackBelt-wrapped subethasmtp/3.1.7_1 bundle along with JavaMail protocol bundles: mailapi/1.6.2, javax.mail/1.6.2, smtp/1.6.2, imap/1.6.2, and pop3/1.6.2 from com.sun.mail.

#### Scenario: Installing SubEtha Mail deploys SMTP server and mail protocol bundles
- **GIVEN** an Apache Karaf container with the subethamail feature repository registered
- **WHEN** the subethamail feature is installed
- **THEN** the bundle mvn:hu.blackbelt.bundles.subethasmtp/org.subethamail.subethasmtp/3.1.7_1 SHALL be deployed along with mailapi/1.6.2, javax.mail/1.6.2, smtp/1.6.2, imap/1.6.2, and pop3/1.6.2 bundles

### Requirement: Feature is self-contained with no inter-feature dependencies
The subethamail feature SHALL have no inter-feature dependencies, embedding all required JavaMail bundles directly.

#### Scenario: SubEtha Mail declares no feature prerequisites
- **GIVEN** the subethamail feature repository XML is parsed by Karaf
- **WHEN** the subethamail feature definition is loaded
- **THEN** it SHALL contain no feature dependency elements

### Requirement: Feature defaults to manual installation
The subethamail feature SHALL declare install="false".

#### Scenario: Feature auto-install is disabled
- **GIVEN** the subethamail feature repository XML is parsed by Karaf
- **WHEN** the feature definition is loaded
- **THEN** the subethamail feature SHALL have its install attribute set to "false"
