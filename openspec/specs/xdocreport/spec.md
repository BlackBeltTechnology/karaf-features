# XDocReport Karaf Features Specification

## Purpose
Provides OSGi Karaf feature descriptors for deploying the XDocReport document generation and conversion framework into an Apache Karaf container, supporting DOCX, ODT, ODP, ODS, and PPTX document formats with PDF and XHTML conversion capabilities.

## Architecture
The module defines the `xdocreport-${project.version}` feature repository containing approximately 20 features organized in layers: foundation (xdocreport-jaxb4, xdocreport-odfdom, xdocreport-openpdf-extension), core (xdocreport-core depending on xdocreport-odfdom), document handlers (docx, odt, odp, ods, pptx, textstyling-wiki), template engines (freemarker, velocity), converters (docx-docx4j, docx-xwpf, odt-odfdom), and format converters (odfdom-converter-core, odfdom-converter-pdf-openpdf, odfdom-converter-xhtml, poi-xwpf-converter-core, poi-xwpf-converter-pdf-openpdf, poi-xwpf-converter-xhtml). Features extensively depend on other karaf-features modules including apache-commons, apache-poi, apache-httpclient, jackson, antlr, and bouncycastle.

## Requirements

### Requirement: Provide JAXB 4 API support
The xdocreport-jaxb4 feature SHALL provide jakarta.xml.bind-api/4.0.2, jaxb-osgi/2.3.9, jakarta.activation-api/2.1.3, and javax-el-api-3.0.0 bundles.

#### Scenario: Installing JAXB4 deploys XML binding API bundles
- **GIVEN** an Apache Karaf container with the xdocreport feature repository registered
- **WHEN** the xdocreport-jaxb4 feature is installed
- **THEN** the bundles jakarta.xml.bind-api/4.0.2, jaxb-osgi/2.3.9, jakarta.activation-api/2.1.3, and javax-el-api-3.0.0/3.0.0_1 SHALL be deployed

### Requirement: Provide ODFDOM document handling with extensive dependencies
The xdocreport-odfdom feature SHALL depend on apache-commons-io, apache-commons-codec, apache-httpclient4, apache-commons-compress, apache-commons-csv, apache-commons-lang3, apache-commons-validator, and apache-commons-bcel features, and SHALL provide bundles for JSON, Jena, java-rdfa, Xerces, XMLResolver, Xalan, Jackson core, Dexx collections, JSON-LD, Commons RDF, Thrift, and ODFDOM.

#### Scenario: Installing ODFDOM pulls in Apache Commons and document processing bundles
- **GIVEN** an Apache Karaf container with the xdocreport feature repository registered
- **WHEN** the xdocreport-odfdom feature is installed
- **THEN** the eight dependency features SHALL be installed and the bundle mvn:hu.blackbelt.bundles.odfdom/org.odftoolkit.odfdom-java/0.12.0_1 SHALL be deployed along with all supplemental bundles

### Requirement: Provide XDocReport core with ODFDOM dependency
The xdocreport-core feature SHALL depend on the xdocreport-odfdom feature and SHALL provide fr.opensagres.xdocreport.template/2.1.0, fr.opensagres.xdocreport.document/2.1.0, fr.opensagres.xdocreport.converter/2.1.0, and fr.opensagres.xdocreport.core/2.1.0 bundles.

#### Scenario: Installing XDocReport core deploys the framework bundles
- **GIVEN** an Apache Karaf container with the xdocreport feature repository registered
- **WHEN** the xdocreport-core feature is installed
- **THEN** the xdocreport-odfdom feature SHALL be installed and the four XDocReport core bundles (template, document, converter, core) at version 2.1.0 SHALL be deployed

### Requirement: Provide DOCX conversion via docx4j with POI 5, ANTLR 2, and JAXB 4
The xdocreport-converter-docx-docx4j feature SHALL depend on xdocreport-core, apache-poi5, apache-commons-lang, antlr2, apache-httpclient4, and xdocreport-jaxb4 features, and SHALL provide the docx4j/8.3.14_1 bundle and the converter bundle fr.opensagres.xdocreport.converter.docx.docx4j/2.1.0.

#### Scenario: Installing DOCX-docx4j converter pulls in all required features
- **GIVEN** an Apache Karaf container with the xdocreport feature repository registered
- **WHEN** the xdocreport-converter-docx-docx4j feature is installed
- **THEN** the xdocreport-core, apache-poi5, apache-commons-lang, antlr2, apache-httpclient4, and xdocreport-jaxb4 features SHALL be installed and the bundle mvn:hu.blackbelt.bundles.docx4j/org.docx4j/8.3.14_1 SHALL be deployed

### Requirement: Provide DOCX conversion via XWPF with POI-based converters
The xdocreport-converter-docx-xwpf feature SHALL depend on xdocreport-core, xdocreport-poi-xwpf-converter-core, xdocreport-poi-xwpf-converter-pdf-openpdf, and xdocreport-poi-xwpf-converter-xhtml features.

#### Scenario: Installing DOCX-XWPF converter pulls in all converter features
- **GIVEN** an Apache Karaf container with the xdocreport feature repository registered
- **WHEN** the xdocreport-converter-docx-xwpf feature is installed
- **THEN** all four dependency features SHALL be installed and the bundle mvn:fr.opensagres.xdocreport/fr.opensagres.xdocreport.converter.docx.xwpf/2.1.0 SHALL be deployed

### Requirement: Provide document format handlers for DOCX, ODT, ODP, ODS, and PPTX
The xdocreport-document-docx, xdocreport-document-odt, xdocreport-document-odp, xdocreport-document-ods, and xdocreport-document-pptx features SHALL each depend on xdocreport-core and provide their respective fr.opensagres.xdocreport.document.* bundle at version 2.1.0.

#### Scenario: Installing DOCX document handler deploys the document bundle
- **GIVEN** an Apache Karaf container with the xdocreport feature repository registered
- **WHEN** the xdocreport-document-docx feature is installed
- **THEN** the xdocreport-core feature SHALL be installed and the bundle mvn:fr.opensagres.xdocreport/fr.opensagres.xdocreport.document.docx/2.1.0 SHALL be deployed

### Requirement: Provide Freemarker and Velocity template engines
The xdocreport-template-freemarker feature SHALL depend on xdocreport-core and provide freemarker/2.3.33_1 and the template.freemarker/2.1.0 bundles. The xdocreport-template-velocity feature SHALL depend on xdocreport-core and apache-commons-lang3 and provide velocity/2.4.1_1 and the template.velocity/2.1.0 bundles.

#### Scenario: Installing Freemarker template engine deploys template support
- **GIVEN** an Apache Karaf container with the xdocreport feature repository registered
- **WHEN** the xdocreport-template-freemarker feature is installed
- **THEN** the xdocreport-core feature SHALL be installed and the bundles freemarker/2.3.33_1 and fr.opensagres.xdocreport.template.freemarker/2.1.0 SHALL be deployed

### Requirement: Provide OpenPDF extension for PDF output
The xdocreport-openpdf-extension feature SHALL provide openpdf/2.4.0 and fr.opensagres.xdocreport.openpdf.extension/2.1.0 bundles.

#### Scenario: Installing OpenPDF extension deploys PDF rendering support
- **GIVEN** an Apache Karaf container with the xdocreport feature repository registered
- **WHEN** the xdocreport-openpdf-extension feature is installed
- **THEN** the bundles mvn:com.github.librepdf/openpdf/2.4.0 and mvn:fr.opensagres.xdocreport/fr.opensagres.xdocreport.openpdf.extension/2.1.0 SHALL be deployed

### Requirement: Provide POI XWPF converter core with XMLBeans 5 and POI 5
The xdocreport-poi-xwpf-converter-core feature SHALL depend on apache-xmlbeans5, apache-poi5, and xdocreport-openpdf-extension features.

#### Scenario: Installing POI XWPF converter core pulls in POI 5 and XMLBeans 5
- **GIVEN** an Apache Karaf container with the xdocreport feature repository registered
- **WHEN** the xdocreport-poi-xwpf-converter-core feature is installed
- **THEN** the apache-xmlbeans5, apache-poi5, and xdocreport-openpdf-extension features SHALL be installed and the bundle mvn:fr.opensagres.xdocreport/fr.opensagres.poi.xwpf.converter.core/2.1.0 SHALL be deployed
