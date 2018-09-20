# GA4GH Discovery Search API

A standard for a global federated data sharing network that allows the searching, and subsequent -optional- processing of the results in a cloud environment.

Please note this standard is a work in progress.

# Preface

The Search API is a new standard for data search. It is robust and flexible approach, considering databases that contain many types of case associated data, without sacrificing the huge power of unilateral federated searching underpinned by an interoperable format for search and response.

# Conventions and Terminology

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL
NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and
"OPTIONAL" in this document are to be interpreted as described in
[RFC 2119](https://tools.ietf.org/html/rfc2119).

# Concepts

We define the two actors involved as the `client` and the `server`, following HTTP semantics.

A `client` is a system which makes `search` requests to a `server` (or many servers in a federated system). This can be done on behalf of a user or as part of an automated workflow/pipeline.

A `server` is a system that contains data on which it can perform searches and from which it can return `results` responses.

A `search` request is a JSON payload that contains a query and associated meta data.

A `results` response is a JSON payload that contains the results produced by running a query and associated meta data.

This specification does not define the data model of any data stored on the `server`, nor does it define how this data is indexed, searched, or ranked. For example a `server` could store and index annotations for specific genomic coordinates, patient cases, animal models, VCF files, phenotypes for disease ontologies, etc...

A system or application may take on the role of a `server` and a `client`, allowing its data to be discoverable, while providing access to other discoverable data to its users.

## Expected process

1. The `client` sends a `search` request to a `server`.

    The `search` request contains metadata and a collection of components which define specific aspects of the query. The `search` may also optionally specify that it requires specific types of data in the `results` for the response to be useful.

2. The `server` uses the query components provided to perform a search on its data.

3. The `server` returns a `results` response with as much or as little of the data or metadata as it is able.
    
    The `results` may include as little as an assertion that some data exist for the query criteria, much in the same way Beacon works, or it may provide, for example, rich and full variant and phenotypic data that fulfils the search criteria.

## Search and Results overview

The `client` makes a `search` request to a `server` as an HTTP POST, where the `search` is defined as a JSON payload.

The main objects of the `search` request are `meta`, `query`, `requires`, and `logic`.

The `server` responds to the `client` with `results`, where the `results` are defined as a JSON payload

The main objects of the `results` response are `meta`, `collectionComponents` and `records`. An array of `records` each contain any number of `components` which represent data.

In both cases, the `meta` object is related to the `search` and `results`, and not any of the `query` and `records` data.

The `components` that can be used in the `query` and `records` differ, though some are useable in both.

The `logic` may be used to define a single operator for all components (`AND` / `OR`), or define a more complex boolean logic query using components.

More details for the [Search](specification/search-structure.md) and the [Results](specification/results-structure.md) structures are provided on the respective pages.

JSON Schemas is be provided as a normative reference for the [Search](specification/search-structure.md) and [Results](specification/results-structure.md). OpenAPI specifications will be provided, but may be omitted for the initial release.

## Components

The API defines six different types of components:
- Search Meta
- Query
- Results Meta
- Collection
- Record
- Record Meta

All components are optional.

Search Meta components can represent information about the `search` request made by the `client` to the `server`, such as what record components are required in the `records` specified in the `results` returned by the `server` to the `client`.

Query components can represent criteria for searching and filtering data. All "Record components" are currently also Query components. This is not expected to remain the case as components are developed in the future.

Results Meta components can represent information about what the server did with the `search` in order to return the `results`.

Collection components can represent summary information about `records` returned from a search.

Record components can represent `record` data which has been deposited to the server for searching.

Record Meta components can represent data associated to a record but not actually part of the record data, for example the contact information for the clinician responsible for the case, and/or links to that case.

## Content of components

Individual components are defined in individual JSON Schema files in YAML format, and combined using JSON Schema references to construct the search and results JSON Schema.

It is RECOMMENDED that search and results payloads are validated using these schemas to confirm compliance.
The YAML files may be converted into JSON files using the npm run script provided.
Further details on this are provided in the [json_schema](json_schema/schemas_source) folder README.md.

It is expected that more components will be developed in the future as the standard evolves.

# Security

This specification is expected to support:
- Open data sources that do not require authentication
- Protected data sources that do require authentication

In the situation where a `server` requires authentication for access, the `client` MUST provide an authentication token located in a header field `X-Auth-Token`.

The authentication token MAY be a predefined token generated by the `server` specifically for the `client`, which they MUST exchange via a secure method such as GPG encryption using public and private keys.

The authentication token MAY be an (O-Auth 2.0 Bearer Token)[https://oauth.net/2/bearer-tokens/] provided in response to an O-Auth exchange, although how to achieve this is not specified. O-Auth 2.0 is recommended by GA4GH for authentication and authorisation purposes.

# API Version

The API version will follow the rules set out by [Semantic Versioning 2.0.0](https://semver.org/spec/v2.0.0.html).

    Given a version number MAJOR.MINOR.PATCH, increment the:

    MAJOR version when you make incompatible API changes,
    MINOR version when you add functionality in a backwards-compatible manner, and
    PATCH version when you make backwards-compatible bug fixes.

    Additional labels for pre-release and build metadata are available as extensions to the MAJOR.MINOR.PATCH format.

The MAJOR version MUST be included in the URL exposed by servers for the API, in the format of `vX`, where `X` is a major version.

The version of the API is linked to the `search` request and `results` response, and not any individual components used with the `request` and `response`, which have their own associated semantic version number.

An example API URL: `https://yournode.org/v1/search`

## Expectations

A `search` request MAY specify which API version `results` response they require (if any) in the format of an [X-Range](https://docs.npmjs.com/misc/semver#x-ranges-12x-1x-12-) string (The Major version MUST match) using a header of `X-GA4GH-Discovery-Expect` with the request. The `server` MUST either respond with a backwards compatible version of the `results`, or respond with HTTP Status Code `422` which MAY include a text body or JSON body detailing the unsupported version request, which SHOULD include which versions are supported.

If a `search` request does not specify a required `results` response version, the `server` MUST respond with the latest version they support of the major version defined in the API URL.

Note: Patch versions are backwards and forwards compatible, while Minor versions are only backwards compatible, and not forwards compatible.

## Errors

A possible text based error response body: `Error: System does not support returning version 1.1. Please upgrade to support responses in version 1.2`

A JSON based error response body MAY follow the error object format defined by the (jsonapi specification)[http://jsonapi.org/format/#errors]. For example:

```
{
  "errors": [
    {
      "status": "422",
      "title":  "Unsupported API response version",
      "detail": "System does not support returning version 1.1. Please upgrade to support responses in version 1.2",
      "meta": {
        "supportedVersions": [
          "1.2.0",
          "1.2.1",
          "1.2.3",
          "1.2.4",
          "1.3.0"
        ]
      }
    }
  ]
}
```

The above example makes use of the standard fields provided by jsonapi, and the meta section which supports non-standard fields.

If an error is caused by the content of the JSON as opposed to a version missmatch, it's possible to use the jsonapi error format to point to specific parts of a JSON to indicate what is causing the error. The source pointer uses JSON Pointer [RFC6901](https://tools.ietf.org/html/rfc6901), which is already used in other parts of the `search` and `result` format.

```
HTTP/1.1 422 Unprocessable Entity
Content-Type: application/vnd.api+json

{
  "errors": [
    {
      "status": "422",
      "source": { "pointer": "/data/attributes/first-name" },
      "title":  "Invalid Attribute",
      "detail": "First name must contain at least three characters."
    }
  ]
}
```

# Content type

The Content Type for a `search` HTTP request MUST be `application/json`.

The Content Type for `results` response MUST be `application/json` and an `200` HTTP status code if the `search` request was successful. The server MAY return non JSON body content if the outcome is not successful.

The HTTP status code should be checked before attempting to process the `results` response. `Severs` may include error information with a non `200` HTTP status code, but the structure for this is not defined.

## Content

The HTTP POST `search` request and the `results` response HTTP body are both be JSON payloads.
See the following documents for details on the format:
* [The structure of the `search`](specification/search-structure.md)
* [The structure of the `result`](specification/results-structure.md)

These additional documents are normative for this specification.

# Reserved keys and extensions

The API reserves for its use JSON object keys which begin with an underscore "\_" or a dash "-".

Keys that start with a dash "-" are reserved for use in the `logic` object, and MUST NOT be used when defining new keys for components or extensions. For example "-AND", which means that "AND" cannot be used as a key anywhere else in the API.

The API defined structure allows for clients or servers to define their own proprietary extensions as they see fit, by prefixing JSON object keys with an underscore "\_".

A server MAY include additional information about a record or summary information about the collection of records which are not defined by a component. A non-standard or proprietary component can be included by prefixing the given component name with an underscore, for example `_mySpecialComponent`. This provides maximum flexibility for both the host AND server to evolve independently, but still be part of the collective. Further, diverse nodes have different levels of comfort and permissions to share data. This standard does not impose sharing policy decisions, as it is a matter of that implementing nodes internal policy. 

By allowing flexibility for individual nodes to evolve as they see fit, but still be part of the collective, and ideally donating back those non-standard or proprietary components back to the collective, we create a primary mechanism of generating new components. This mechanism will guarantee components are in existence ONLY when they are needed. 

A client and a server may have a special relationship where they want to allow additional information to be searched on, which doesn't require any authentication. The maintainers of the client and the server may agree on new components, but must prefix the component names with underscores.

The `server` MUST NOT prefix all the fields of non-standard component (or any components) with an underscore, as it is assumed that any JSON data that is a descendant of a key which is prefixed with an underscore is also non-standard.

It is RECOMMENDED that a JSON Schema be supplied for a non-standard or proprietary component to formally define the expectations, which may later be added to a repository of official GA4GH components, should the component be seen as useful by the community.

It is RECOMMENDED that non-standard or proprietary component names use organisational prefixes (after the use of an underscore) in order to avoid potential name clashes at a later point. Fully fledged components may also need to have their names use an organisational prefix.

## Example usage

These example constructs are based on use-cases from the driver projects and their feedback: [examples](example_usage/README.md)

The linked example usage document is included as an informative reference.

# Acknowledgments

This generalized standard was inspired, adapted, and built up from existing work by:

* Global Alliance for Genomics and Health Discovery Search API [team](https://docs.google.com/document/d/1lzN_pu8tATZXUvDtFKSG7IevE5TWLfFz0tdKfgtUSzU)
* Requirements, design, brainstorming, and ideas gathered from the GA4GH driver project community, [summerized](https://docs.google.com/document/d/1jPPVhSvmzW5kK_rKTxkjPvxlcGicHhHPgO4cqqP3CBU/edit?ts=5a84936c#heading=h.4lfahcth694x).
* Orion Buske (@buske) and Ben Hutton (@relequestual) - [MME V2 Proposal](https://github.com/ga4gh/mme-apis/blob/version2-mock/version2/overview.md)
* Ben Hutton's (@relequestual) GA4GH Search API component based architecture [proposal](https://gist.github.com/Relequestual/65c0446944519a66f8562d02b3cb4c86)
* GePh-Query API ("Jeff") by Anthony J. Brookes and his team at the [Cafe Variome](https://www.cafevariome.org) discovery platform
* The merging of concepts, content, and building of first draft by Harindra Arachchi, François Schiettecatte 
* The Matchmaker Exchange APIs [@github](https://github.com/ga4gh/mme-apis)

Thanks to all [contributors](https://github.com/ga4gh-discovery/ga4gh-discovery-search/graphs/contributors)

# Matchmaker Exchange

This standard is deeply informed by the [Matchmaker Exchange (MME) API](https://github.com/ga4gh/mme-apis), both in lessons learned during development and in the operation of this API in practice.
