# GA4GH Discovery - Case Discovery API

**This readme is being updated to reflect the name and scope change of the project. Please stand by.**

A standard for a global federated data sharing network that allows the searching, and subsequent -optional- processing of the results in a cloud environment.

# Preface

The Search API is a new standard for data search. It is robust and flexible approach, considering databases that contain many types of case associated data, without sacrificing the huge power of unilateral federated searching underpinned by an interoperable format for search and response.

# Current Status

This standard is a work in progress, and we are making revisions based on feedback from the GA4GH 2018 plenary in Basel.

# Contributing

The project has not as yet defined a formal guide for contributing, however the easiest way to get involved is to join our regular videoconference calls.

If you wish to join, please email [Rishi Nag](mailto:rishi.nag@ga4gh.org?subject=Joining%20Discovery%20Search%20API%20calls)

You can view the minutes of our previous meetings on this [Google Docs document](https://docs.google.com/document/d/1lzN_pu8tATZXUvDtFKSG7IevE5TWLfFz0tdKfgtUSzU)

# Releases

The master branch represents a work in progress and not necessarily any individual release state.
For releases, please see the [Releases page](https://github.com/ga4gh-discovery/ga4gh-discovery-search/releases).

Clicking on the tag icon for a release will show the repository on Github at the selected release state, which will make it easier to view should you not wish to download release file bundles.

# Specification

The specification document (a normative document for this specification) can be found in the root of this repo: [specification](specification.md).
The specification folder contains an overall readme.md which will introduce you to the concepts and basic aspects of the specification.
The request and response format for the Search API are split into separate files which are linked to from the main specification document.

# JSON Schema

[JSON Schema](http://json-schema.org/) is a JSON based vocabulary that allows for the annotation and validation of JSON documents.
The bulk of the Search API specification is defining the request (search) and response (results) HTTP body structures.
JSON Schema documents are provided as a normative reference for the Search API specification, and can be found in the [json_schema](json_schema) folder of this repository.

# Future Work and Plans

The first stable release of the Search API is designed as a minimal viable specification.

Future work needs to be fleshed out in order to allow the building of a roadmap with rough estimations.

## Testing

It is possible to create search and result format validation tests using the JSON Schema documents, however there is not a compliance testing suite to check that a search is handled correctly which returns the correct results.

We would like to have a fully features test suite which is able to confirm compliance with all aspects of the specification as far as possible.

We would also like to develop continuous integration tests to check and confirm that all the JSON Schema documents are valid, and any JSON within the specification is valid JSON.

## Contributing

We would like to develop a full contributing guidelines document, which includes instructions on how to develop new non-standard components, and set out a process for making components recognised as standard.

It is expected that systems or groups will want to add new components that are not defined with the initial release, as it only includes basic components required for a minimal working specification.

## Tooling

We would like to develop tooling which allows for the creation of human readable documentation based on the JSON Schema files.

It is expected that someone implementing this Search API would be comfortable reading YAML, but they may not yet be familiar with JSON Schema.

## Open API

Open API is fast becoming the definitive API documentation format, and is already being used by other project in GA4GH.

Althought OpenAPI does use a substantial part of the JSON Schema specification for defining payloads, it is not currently possible to drop in all JSON Schema documents in place of an OpenAPI JSON Schema object. The OpenAPI team have announced plans to allow the use of standard JSON Schema in a future release.

Tooling can be developed to convert some JSON Schema documents to be used within an OpenAPI specification. As the Search API will only have one endpoint, it's not clear how much value will be added by using an OpenAPI definition in addition to currently provided JSON Schema documents.

## Authentication

The Search API defines the use of a proprietary header for authentication.

If the use case presents that the Search API requires more standard authentication processes, we should consider using the Authorization header, following closer to HTTP semantics. This would allow for multiple possible authentication methods, but is more difficult to implement.

## Collaboration

Multiple GA4GH Workstream projects or products are defining object models which represent genomic data.

While other projects may require a complete release process for updated data definitions, we would like to create a central hub of "components" for use within the Search API (or any other project that wishes to use them). Additions or modifications will not effect the overall workings of the Search API.

Implementers of the Search API, may choose to pick and use new components put forward by any other group, as and when they are ready to do so.

When other GA4GH products mature to release, where the product is a format specification we should seek to provide a tool to translate between the new format and Discovery components where possible.

## Use of Non-Standard Component Schemas

JSON Schema can be used to annotate a JSON document as well as provide validation.

It is possible to create a workflow in which, non-standard components can be returned to provide additional data in relation to a search, and displayed to the end user of a system which uses the Search API, without the requirement to know the details of the non-standard component beforehand.

If a non-standard component also provides a JSON Schema URL, the JSON data within the non-standard component has a context and knowledge about the non-standard component which can be used to meaningfully present the data.

[schemastore.org](http://schemastore.org) hosts over 140 JSON Schema files for known JSON and YAML formatted files frequently used by developers. The popular code editor VSCode from Microsoft, utilises this JSON Schema store, automatically downloading schema files for validation and annotation of file types recognised by their file name conventions.
