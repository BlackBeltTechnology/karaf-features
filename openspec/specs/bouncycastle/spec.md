# BouncyCastle Karaf Features Specification

## Purpose
Provides OSGi Karaf feature descriptors for deploying multiple versions of the BouncyCastle cryptography library into an Apache Karaf container, with an alias feature that points to the current default version.

## Architecture
The module defines the `bouncycastle-${project.version}` feature repository containing five features: a bouncycastle alias (defaulting to 1.81), and four versioned features -- bouncycastle-1.69, bouncycastle-1.70, bouncycastle-1.78 (version 1.78.1), and bouncycastle-1.81. Versions 1.69 and 1.70 use jdk15on artifacts while versions 1.78 and 1.81 use jdk18on artifacts. Each versioned feature provides four bundles: bcprov, bcmail, bcpkix, and bcutil.

## Requirements

### Requirement: Provide BouncyCastle alias pointing to default version 1.81
The bouncycastle feature SHALL declare a dependency on the bouncycastle-1.81 feature, acting as an alias to the current default version.

#### Scenario: Installing bouncycastle installs the 1.81 version
- **GIVEN** an Apache Karaf container with the bouncycastle feature repository registered
- **WHEN** the bouncycastle feature is installed
- **THEN** the bouncycastle-1.81 feature SHALL also be installed

### Requirement: Provide BouncyCastle 1.69 with JDK 15 artifacts
The bouncycastle-1.69 feature SHALL provide bcprov-jdk15on/1.69, bcmail-jdk15on/1.69, bcpkix-jdk15on/1.69, and bcutil-jdk15on/1.69 bundles.

#### Scenario: Installing BouncyCastle 1.69 deploys all four crypto bundles
- **GIVEN** an Apache Karaf container with the bouncycastle feature repository registered
- **WHEN** the bouncycastle-1.69 feature is installed
- **THEN** the bundles bcprov-jdk15on, bcmail-jdk15on, bcpkix-jdk15on, and bcutil-jdk15on at version 1.69 SHALL be deployed

### Requirement: Provide BouncyCastle 1.81 with JDK 18 artifacts
The bouncycastle-1.81 feature SHALL provide bcprov-jdk18on/1.81, bcmail-jdk18on/1.81, bcpkix-jdk18on/1.81, and bcutil-jdk18on/1.81 bundles.

#### Scenario: Installing BouncyCastle 1.81 deploys all four crypto bundles
- **GIVEN** an Apache Karaf container with the bouncycastle feature repository registered
- **WHEN** the bouncycastle-1.81 feature is installed
- **THEN** the bundles bcprov-jdk18on, bcmail-jdk18on, bcpkix-jdk18on, and bcutil-jdk18on at version 1.81 SHALL be deployed

### Requirement: JDK artifact naming tracks BouncyCastle version
The bouncycastle-1.69 and bouncycastle-1.70 features SHALL use jdk15on artifacts, while bouncycastle-1.78 and bouncycastle-1.81 features SHALL use jdk18on artifacts.

#### Scenario: Older versions use jdk15on and newer versions use jdk18on
- **GIVEN** the bouncycastle feature repository XML is parsed by Karaf
- **WHEN** the feature definitions are loaded
- **THEN** bouncycastle-1.69 and bouncycastle-1.70 SHALL reference org.bouncycastle/*-jdk15on artifacts, and bouncycastle-1.78 and bouncycastle-1.81 SHALL reference org.bouncycastle/*-jdk18on artifacts
