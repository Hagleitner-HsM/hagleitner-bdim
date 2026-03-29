# Hagleitner Business Data Interchange Model
The Business Data Interchange Model (BDIM) defines a shared contract for exchanging business data between participating systems and services. It provides a service-independent model for entities, their structure, validation rules, and exchange conventions, so that systems such as CRM, ERP, IoT, and other backend services can communicate consistently over interfaces such as REST APIs and Azure Service Bus messaging.

This repository contains JSON schemas and JSON sample files for the common schemas, as well as JSON schemas and JSON examples for individual messages.

## Versioning 
The Business Data Interchange Model (BDIM) is versioned at the level of the entire published schema set, not at the level of individual entity schemas. Each published version contains all schemas and all accompanying documentation that belong to the same contractual baseline.

A published BDIM version is stored in a dedicated version folder in the Git repository, for example: `/bdim/v1.2/`

This folder contains the complete set of JSON Schemas and the corresponding Markdown documentation for that version. The same structure is used for publication, so all artifacts of a version are published under a matching URL path, for example: `https://schema.hagleitner.com/bdim/v1.2/...`

## Development environment
To view and modify the JSON schemas, JSON example files, and documentation Markdown files, the following environment is recommended:
* Visual Studio Code
* Visual Studio Extension: JSON Schema validation tool
* Visual Studio Extension: JSON Schema Viewer
* Visual Studio Extension: Markdown preview enhanced

