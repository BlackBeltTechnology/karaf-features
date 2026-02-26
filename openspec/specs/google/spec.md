# Google Karaf Features Specification

## Purpose
Provides OSGi Karaf feature descriptors for deploying a comprehensive set of Google libraries -- including Guava, Guice, Gson, Protobuf, JimFS, Netty, OpenCensus, and the Google Cloud/API/Ads/BigQuery client stack -- into an Apache Karaf container.

## Architecture
The module defines the `google-${project.version}` feature repository containing over 30 features organized into several groups: JimFS (versions 1.1, 1.2, 1.3 with alias), Guava (versions 18 through 33 with alias), Guice (versions 4, 5, 7 with alias), Gson, JSR-305, OpenCensus, Protobuf (versions 3 and 4), Netty client, Google HTTP client, Google Auth, Google API client, Google API extension, Google API gRPC, Google API GCloud, Google AppEngine, Google Ads, Google BigQuery, Google GData, and Google FlatBuffers. Features form a deep dependency hierarchy with google-api-bigquery and google-api-gcloud at the top of the stack, depending transitively on auth, HTTP client, gRPC, protobuf, Guava, and many other features.

## Requirements

### Requirement: Provide Guava version alias defaulting to 20.0
The guava feature SHALL declare a dependency on guava-20, and the jimfs feature SHALL declare a dependency on jimfs-1.3, acting as aliases to their default versions.

#### Scenario: Installing guava alias installs guava-20
- **GIVEN** an Apache Karaf container with the google feature repository registered
- **WHEN** the guava feature is installed
- **THEN** the guava-20 feature SHALL also be installed

### Requirement: Provide Guava versions 18 through 33
The feature repository SHALL provide guava-18 through guava-33 features, where versions 18-26 provide only the guava bundle and versions 27+ additionally provide the failureaccess/1.0.3 bundle.

#### Scenario: Installing Guava 33 deploys guava and failureaccess bundles
- **GIVEN** an Apache Karaf container with the google feature repository registered
- **WHEN** the guava-33 feature is installed
- **THEN** the bundles mvn:com.google.guava/failureaccess/1.0.3 and mvn:com.google.guava/guava/33.4.8-jre SHALL be deployed

### Requirement: Provide Google Guice versions with appropriate Guava dependencies
The guice-4 feature SHALL depend on guava-27, guice-5 SHALL depend on guava-30 and jsr305, and guice-7 SHALL depend on guava-31 and jsr305.

#### Scenario: Installing Guice 5 pulls in Guava 30 and JSR-305
- **GIVEN** an Apache Karaf container with the google feature repository registered
- **WHEN** the guice-5 feature is installed
- **THEN** the guava-30 and jsr305 features SHALL be installed and the bundle mvn:com.google.inject/guice/5.1.0 SHALL be deployed

### Requirement: Provide Google Protobuf 3 with Guava and Gson dependencies
The google-protobuf-3 feature SHALL depend on guava-32, gson, and jsr305, and SHALL provide protobuf-java/3.25.8, protobuf-java-util/3.25.8, and protobuf-javanano/3.1.0 bundles at start-level 35.

#### Scenario: Installing Protobuf 3 deploys all protobuf bundles
- **GIVEN** an Apache Karaf container with the google feature repository registered
- **WHEN** the google-protobuf-3 feature is installed
- **THEN** the bundles protobuf-java/3.25.8, protobuf-java-util/3.25.8, and protobuf-javanano/3.1.0 SHALL be deployed at start-level 35

### Requirement: Provide Netty client feature with BouncyCastle dependency
The netty-client feature SHALL depend on the bouncycastle feature and SHALL provide Netty bundles (common, transport, handler, buffer, resolver, codec, codec-http, codec-http2, transport-native-unix-common, tcnative-classes) at version 4.1.111.Final with start-level 40.

#### Scenario: Installing Netty client deploys all Netty bundles
- **GIVEN** an Apache Karaf container with the google feature repository registered
- **WHEN** the netty-client feature is installed
- **THEN** the bouncycastle feature SHALL be installed and ten Netty bundles at version 4.1.111.Final SHALL be deployed at start-level 40

### Requirement: Provide Google HTTP client with transitive API dependencies
The google-http-client feature SHALL depend on apache-httpclient4, opencensus, gson, and jackson-jaxrs features, and SHALL provide the BlackBelt-wrapped google-http-client/1.47.0_2 bundle.

#### Scenario: Installing Google HTTP client pulls in HTTP and serialization features
- **GIVEN** an Apache Karaf container with the google feature repository registered
- **WHEN** the google-http-client feature is installed
- **THEN** the apache-httpclient4, opencensus, gson, and jackson-jaxrs features SHALL be installed and the bundle mvn:hu.blackbelt.bundles.google-http-client/com.google.http-client/1.47.0_2 SHALL be deployed

### Requirement: Provide Google API BigQuery with full dependency chain
The google-api-bigquery feature SHALL depend on threeten, apache-arrow, google-api-client, google-api-grpc, google-api-extension, and google-api-gcloud features.

#### Scenario: Installing BigQuery pulls in the complete Google API stack
- **GIVEN** an Apache Karaf container with the google feature repository registered
- **WHEN** the google-api-bigquery feature is installed
- **THEN** the threeten, apache-arrow, google-api-client, google-api-grpc, google-api-extension, and google-api-gcloud features SHALL be installed and the bundle mvn:hu.blackbelt.bundles.google-api-bigquery/com.google.api-bigquery/2.54.0_2 SHALL be deployed

### Requirement: Provide Google API gRPC with networking and compression dependencies
The google-api-grpc feature SHALL depend on google-auth, perfmark, bouncycastle, netty-client, google-protobuf-3, brotli4j, jzlib, lz4-java, lzma-java, and guava-32 features.

#### Scenario: Installing gRPC pulls in auth, networking, and compression features
- **GIVEN** an Apache Karaf container with the google feature repository registered
- **WHEN** the google-api-grpc feature is installed
- **THEN** all ten dependency features SHALL be installed and the bundle mvn:hu.blackbelt.bundles.google-api-grpc/com.google.api-grpc/1.70.0_4 SHALL be deployed
