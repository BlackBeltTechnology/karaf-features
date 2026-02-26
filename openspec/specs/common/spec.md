# Common Karaf Features Specification

## Purpose
Provides OSGi Karaf feature descriptors for deploying commonly used utility libraries -- including JavaMail, compression codecs, ThreeTen date/time, OpenTelemetry, PerfMark, and RE2J -- into an Apache Karaf container.

## Architecture
The module defines the `common-${project.version}` feature repository containing 11 features. The javax-mail feature is an alias for javax-mail-1.6.2, and two JavaMail versions (1.6.2 and 1.6.8) are available. Additional standalone features provide PerfMark (0.27.0), ThreeTen (1.7.1), Brotli4j (1.18.0), JZLib (1.1.3), LZMA Java (1.3), LZ4 Java (1.8.0), OpenTelemetry (1.51.0), and RE2J (1.8). Most bundles are provided via BlackBelt-wrapped artifacts. All features are independent with no inter-feature dependencies except the javax-mail alias.

## Requirements

### Requirement: Provide JavaMail 1.6.2 with full protocol bundles
The javax-mail-1.6.2 feature SHALL provide javax.mail-api/1.6.2, mailapi/1.6.2, javax.mail/1.6.2, smtp/1.6.2, imap/1.6.2, and pop3/1.6.2 bundles from com.sun.mail.

#### Scenario: Installing JavaMail 1.6.2 deploys all mail protocol bundles
- **GIVEN** an Apache Karaf container with the common feature repository registered
- **WHEN** the javax-mail-1.6.2 feature is installed
- **THEN** all six mail bundles (javax.mail-api, mailapi, javax.mail, smtp, imap, pop3) at version 1.6.2 SHALL be deployed

### Requirement: Provide JavaMail alias pointing to default version
The javax-mail feature SHALL declare a dependency on javax-mail-1.6.2, acting as an alias to the default JavaMail version.

#### Scenario: Installing javax-mail installs the 1.6.2 version
- **GIVEN** an Apache Karaf container with the common feature repository registered
- **WHEN** the javax-mail feature is installed
- **THEN** the javax-mail-1.6.2 feature SHALL also be installed

### Requirement: Provide JavaMail 1.6.8 with full protocol bundles
The javax-mail-1.6.8 feature SHALL provide mailapi/1.6.8, javax.mail/1.6.8, smtp/1.6.8, imap/1.6.8, and pop3/1.6.8 bundles from com.sun.mail.

#### Scenario: Installing JavaMail 1.6.8 deploys all mail protocol bundles
- **GIVEN** an Apache Karaf container with the common feature repository registered
- **WHEN** the javax-mail-1.6.8 feature is installed
- **THEN** all five mail bundles (mailapi, javax.mail, smtp, imap, pop3) at version 1.6.8 SHALL be deployed

### Requirement: Provide PerfMark with error_prone_annotations
The perfmark feature SHALL provide the PerfMark 0.27.0 bundle along with a wrapped error_prone_annotations/2.41.0 bundle.

#### Scenario: Installing PerfMark deploys performance marking library
- **GIVEN** an Apache Karaf container with the common feature repository registered
- **WHEN** the perfmark feature is installed
- **THEN** the bundle mvn:hu.blackbelt.bundles.perfmark/io.perfmark/0.27.0_0 and wrap:mvn:com.google.errorprone/error_prone_annotations/2.41.0 SHALL be deployed

### Requirement: Provide standalone utility features
The threeten (1.7.1), brotli4j (1.18.0), jzlib (1.1.3), lzma-java (1.3), lz4-java (1.8.0), opentelemetry (1.51.0), and re2j (1.8) features SHALL each provide their respective single BlackBelt-wrapped bundle without inter-feature dependencies.

#### Scenario: Installing OpenTelemetry deploys only its own bundle
- **GIVEN** an Apache Karaf container with the common feature repository registered
- **WHEN** the opentelemetry feature is installed
- **THEN** the bundle mvn:hu.blackbelt.bundles.opentelemetry/io.opentelemetry/1.51.0_1 SHALL be deployed without requiring any other feature
