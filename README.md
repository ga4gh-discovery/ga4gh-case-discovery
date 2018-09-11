# GA4GH Discovery Search API

A standard for a global federated data sharing network that allows the searching, and subsequent -optional- processing of the results on a cloud environment.

Please note this standard is work in progress.

# Preface

The Search API is a new standard for case discovery. It is robust and flexible approach, considering databases that contain many types of case associated data, without sacrificing the huge power of unilateral federated searching underpinned by an interoperable format for search and response.

# Concept

We define the two actors involved as the `client` and the `server`, following HTTP semantics.

A `client` is a system which makes `search` requests to a `server` (or many servers in a federated request). This can be done on behalf of a user, or as part of an automated workflow/pipeline.

A `server` is a database or system which contains any number of cases and associated case data on which it can perform searches and from which it can return `results`.

Unlike the [MME API](https://github.com/ga4gh/mme-apis), there's no requirement for a `client` to make `search` requests to a `server` on the basis of an existing patient record, allowing a user to construct any search they wish.

# Releases

The master branch represents a work in progess and not necessarily any individual release state.
For releases, please see the [Releases page](https://github.com/ga4gh-discovery/ga4gh-discovery-search/releases).

Clicking on the tag icon for a release will show the repository on Github at the selected release state, which will make it easier to view should you not wish to download release file bundles.

## Expected process

1. The `client` poses a question regarding a patient to the `server`, this is defined as the `search`

    The `search` contains a collection of components which define specific aspects of the search. The `search` may also optionally specify that it requires specific types of data in the `results` for the response to be useful.

2. The `server` uses the search components provided to perform a search on its data

3. The `server` returns as much or as little of the data or metadata as it is able.
    
    The `results` may include as little as an assertion that some records exist for the query criteria, much in the same way Beacon works, or it may provide rich and full variant and phenotypic data for each case that fulfils the search criteria.

## Search and Results overview

The `client` makes a `search` request to a `server` as an HTTP POST, where the `search` is defined as a JSON payload.

The main elements of the JSON search payload are `meta`, `query`, `requires`, and `logic`.

The `server` respond to the `client` with `results`, where the `results` are defined as a JSON payload

The main elements of the JSON `results` payload are `meta`, `collectionComponents` and `records`. An array of `records` each contain any number of `components` which represent case data.

In both cases, the `meta` object is related to the `search` and `results`, and not any of the `query` and `records` data.

The `components` that can be used in the `query` and `records` differ, although some are useable in both.

The `logic` may be used to define a single operator for all components (`AND` / `OR`), or define a more complex boolean logic query using components.

More details for the [Search](search_structure/README.md) and the [Results](results_structure/README.md) structures are provided on the respective pages.

JSON Schemas is be provided as a normative reference for the [Search](search_structure/README.md) and [Results](results_structure/README.md). OpenAPI specifications will be provided, but may be omitted for the initial release.

## Components

The API defines six different types of components.
- Search Meta
- Query
- Results Meta
- Collection
- Record
- Record Meta

All components are optional.

Search Meta components can represent information about the `search` request made by the client to the server, such as what record components are required in the `records` specified in the `results`.

Query components can represent criteria for filtering records. All "Record components" are currently also Query components.

Results Meta components can represent information about what the server did with the `search` in order to return the `results`.

Collection components can represent summary information about `records` returned from a search.

Record components can represent `record` data which has been deposited to the server for searching.

Record Meta components can represent data associated to a record but not actually part of the record data, for example the contact information for the clinician responsible for the case, and/or links to that case.

## Content of components

Individual components are defined in individual JSON Schema files in YAML format, and combined using JSON Schema references to construct the search and results JSON Schema.

It is recommended that search and results payloads are validated using these schemas to confirm compliance.
The YAML files may be converted into JSON files using the npm run script provided.
Further details on this are provided in the [json_schema](https://github.com/ga4gh-discovery/ga4gh-discovery-search/tree/master/json_schema/schemas_source) folder README.md.

It is expected that more components will be defined over time as the standard develops.

# Security

This specification is expected to support:

- Open data sources that do not require authentication
- Protected data sources that do require authentication

In the situation where a host requires authentication for access, the client should provide an authentication token located in a header field `X-Auth-Token`.

The authentication token may be a predefined token generated by the server specifically for the client, which they must exchange via a secure method such as GPG encryption using public and private keys.

The authentication token may be an (O-Auth 2.0 Bearer Token)[https://oauth.net/2/bearer-tokens/] provided in response to an O-Auth exchange, although how to achieve this is not specified. O-Auth 2.0 is recommended by GA4GH for authentication and authorisation purposes.

# API Version

The API version will follow the rules set out by [Semantic Versioning 2.0.0](https://semver.org/spec/v2.0.0.html).

    Given a version number MAJOR.MINOR.PATCH, increment the:

    MAJOR version when you make incompatible API changes,
    MINOR version when you add functionality in a backwards-compatible manner, and
    PATCH version when you make backwards-compatible bug fixes.

    Additional labels for pre-release and build metadata are available as extensions to the MAJOR.MINOR.PATCH format.

The MAJOR version MUST be included in the URL exposed by servers for the API, in the format of `vX`, where `X` is a major version.

The version of the API is linked to the `search` and `results` structure, and not any individual components used with the request and response, which have their own associated semantic version number.

An example API URL: `https://yournode.org/v1/search`

## Expectations

A `search` may specify which API version `results` they require (if any) in the format of an [X-Range](https://docs.npmjs.com/misc/semver#x-ranges-12x-1x-12-) string (The Major version MUST match) using a header of `X-GA4GH-Discovery-Expect` with the request. The server must either respond with a backwards compatible version of the `results`, or respond with HTTP Status Code `400`, with a text body detailing the unsupported version request, which should include which versions are supported.

If a `search` does not specify a required `results` version, the responding server must respond with the latest version they support of the major version defined in the API URL.

Note: Patch versions are backwards and forwards compatible, while Minor versions are only backwards compatible.

# Content type

The Content Type for a `search` must be `application/json`.

The Content Type for `results` should be `application/json`, although the server may not return JSON content in case of errors.

The HTTP status code should be checked before attempting to process content. Severs may include error information in a JSON payload with a non `200` HTTP status code, but the structure of this is not defined.

## Content

The HTTP POST `search` body and the `results` body should both be JSON.
See the following documents for details on the format.

* [The structure of the `search`](search_structure/README.md)
* [The structure of the `result`](result_structure/README.md)

# Reserved keys and extensions

The API reserves for its use JSON object keys which begin with an underscore "\_" or a dash "-".

The API defined structure allows for clients or servers to define their own proprietary extensions as they see fit, by prefixing JSON object keys with an underscore "\_".

A server may wish to reveal additional information about a record or summary information about the collection of records which are not defined by a component. A new component proprietary component can be included by prefixing the given component name with an underscore: `_mySpecialComponent`.

A client and a server may have a special relationship where they want to allow additional information to be searched on, which doesn't require any authentication. The maintainers of the client and the server may agree on new components, but must prefix the component names with underscores.

It is not necessary to also prefix all the fields of a proprietary component with an underscore, as it must be assumed that any JSON data that is a descendant of a key which is prefixed with an underscore is also nonstandard.

It is recommended a JSON Schema be supplied for a proprietary component to formally define the expectations, which may later be added to a repository of official GA4GH components, should the component be seen as useful by the community.

## Example usage

These example constructs are based on use-cases from the driver projects and their feedback: [examples](example_usage/README.md)

# Glossary

`server` - a system that handles `search` requests, and return a `results` response, or an error.

`client` - a system that generates `search` requests.

`search` - a `search` request is a JSON document that contains a query and associated meta data.

`results` - a `results` response is a JSON document that contains the results produced by running a query and associated meta data.

# Acknowledgments

This generalized standard was inspired, adapted, and built up from existing work by:

* Global Alliance for Genomics and Health Discovery Search API [team](https://docs.google.com/document/d/1lzN_pu8tATZXUvDtFKSG7IevE5TWLfFz0tdKfgtUSzU)
* Requirements, design, brainstorming, and ideas gathered from the GA4GH driver project community, [summerized](https://docs.google.com/document/d/1jPPVhSvmzW5kK_rKTxkjPvxlcGicHhHPgO4cqqP3CBU/edit?ts=5a84936c#heading=h.4lfahcth694x).
* Orion Buske (@buske) and Ben Hutton (@relequestual) - [MME V2 Proposal](https://github.com/ga4gh/mme-apis/blob/version2-mock/version2/overview.md)
* Ben Hutton's (@relequestual) GA4GH Search API component based architecture [proposal](https://gist.github.com/Relequestual/65c0446944519a66f8562d02b3cb4c86)
* GePh-Query API ("Jeff") by Anthony J. Brookes and his team at the [Cafe Variome](https://www.cafevariome.org) discovery platform
* The merging of concepts, content, and building of first draft by Harindra Arachchi @harindra-a
* The Matchmaker Exchange APIs [@github](https://github.com/ga4gh/mme-apis)

Thanks to all [contributors](https://github.com/ga4gh-discovery/ga4gh-discovery-search/graphs/contributors)


# Matchmaker Exchange

The [Matchmaker Exchange (MME) API](https://github.com/ga4gh/mme-apis) defined a JSON Structure representing a patient which was used to perform a "query by example" as a federated request across a network of servers (or nodes). 

While the MME network was designed to allow for easy federated case matching, due to the complexity of the problem, it is limited by design to allow each server to determine what best constitutes a match. This functionality is useful for databases of siloed cases, but it doesn't allow for broader, specifically targeted searching of cases, which can fulfil many more clinical and scientific use cases.
