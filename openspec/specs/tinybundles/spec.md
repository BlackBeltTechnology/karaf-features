# TinyBundles Karaf Features Specification

## Purpose
Provides an OSGi Karaf feature descriptor for deploying the OPS4J Pax TinyBundles library -- used for programmatic OSGi bundle creation -- into an Apache Karaf container along with its BND and OPS4J Base dependencies.

## Architecture
The module defines the `tinybundles-${project.version}` feature repository containing a single feature: tinybundles (version 3.0.0). The feature is self-contained with no inter-feature dependencies, deploying six bundles: bndlib/3.5.0 from biz.aQute.bnd, four OPS4J Base modules (store, io, lang, monitors at version 1.5.0), and the tinybundles/3.0.0 bundle itself.

## Requirements

### Requirement: Provide TinyBundles with BND and OPS4J Base dependencies
The tinybundles feature SHALL provide biz.aQute.bndlib/3.5.0, ops4j-base-store/1.5.0, ops4j-base-io/1.5.0, ops4j-base-lang/1.5.0, ops4j-base-monitors/1.5.0, and tinybundles/3.0.0 bundles.

#### Scenario: Installing TinyBundles deploys all required bundles
- **GIVEN** an Apache Karaf container with the tinybundles feature repository registered
- **WHEN** the tinybundles feature is installed
- **THEN** all six bundles (biz.aQute.bndlib/3.5.0, ops4j-base-store/1.5.0, ops4j-base-io/1.5.0, ops4j-base-lang/1.5.0, ops4j-base-monitors/1.5.0, tinybundles/3.0.0) SHALL be deployed

### Requirement: Feature is self-contained with no inter-feature dependencies
The tinybundles feature SHALL have no inter-feature dependencies.

#### Scenario: TinyBundles declares no feature prerequisites
- **GIVEN** the tinybundles feature repository XML is parsed by Karaf
- **WHEN** the tinybundles feature definition is loaded
- **THEN** it SHALL contain no feature dependency elements

### Requirement: Feature defaults to manual installation
The tinybundles feature SHALL declare install="false".

#### Scenario: Feature auto-install is disabled
- **GIVEN** the tinybundles feature repository XML is parsed by Karaf
- **WHEN** the feature definition is loaded
- **THEN** the tinybundles feature SHALL have its install attribute set to "false"
