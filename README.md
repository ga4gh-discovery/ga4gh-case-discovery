# GA4GH Discovery Search API

A standard for a global federated data sharing network that allows the querying, and subsequent -optional- processing of the results on a cloud environment.

Please note this standard is work in progress.

This generalized standard was inspired, adapted, and built up from existing work by:

* Global Alliance for Genomics and Health Discovery Search API [team](https://docs.google.com/document/d/1lzN_pu8tATZXUvDtFKSG7IevE5TWLfFz0tdKfgtUSzU)
* Requirements, design, brainstorming, and ideas gathered from the GA4GH driver project community, [summerized](https://docs.google.com/document/d/1jPPVhSvmzW5kK_rKTxkjPvxlcGicHhHPgO4cqqP3CBU/edit?ts=5a84936c#heading=h.4lfahcth694x).
* Orion Buske (@buske) and Ben Hutton (@relequestual) - [MME V2 Proposal](https://github.com/ga4gh/mme-apis/blob/version2-mock/version2/overview.md)
* Ben Hutton's (@relequestual) GA4GH Search API component based architecture [proposal](https://gist.github.com/Relequestual/65c0446944519a66f8562d02b3cb4c86)
* GePh-Query API ("Jeff") by Anthony J. Brookes and his team at the [Cafe Variome](https://www.cafevariome.org) discovery platform
* The merging of concepts, content, and building of first draft by Harindra Arachchi @harindra-a
* The Matchmaker Exchange APIs [@github](https://github.com/ga4gh/mme-apis/blob/master/search-api.md)


# Concept

We define the two actors involved as the `server` and the `client`, following HTTP semantics.

A server is a database or system which contains any number of cases and associated case data, which it is willing to perform searches on, and return data on.

A client is a system which makes search requests to a server (or many servers in a federated request) on behalf of a user and displays any results to the user, or based on instructions as part of a workflow (like a pipeline).

Unlike the MME API, there's no requirement for a client to make requests to a server on the basis of an existing patient record, allowing a user to construct any query they wish.

## Expected process

1. The client poses a question regarding a patient to the server. We define this as the `query`

    The query contains a collection of components which define specific aspects of the query. They query may also optionally specify that it requires specific types of data in the response for the response to be useful.

2. The server uses the query components provided to perform a search on its data

3. The server returns as much or as little of the data or metadata as it is able.
    
    The response may include as little as an assertion that some records exist for the query criteria, much in the same way Beacon works, or it may provide rich and full variant and phenotypic data for each case that fulfils the query criteria.

## Request and Response overview

The request a client makes to a server is over an HTTP POST, where the query is define in a JSON payload.

The main elements of the JSON query payload are `components`, `operators`, and `meta`.

The main elements of the JSON response payload are `records` and `meta`. An array of `records` will each contain any number of `components` which represent case data.

In both cases, the `meta` object is related to the query and response itself, and not any of the query or subject data.

The `components` that can be used in the query and response differ, although some are useable in both.

The `operators` may be used to define a single operator for all components (`AND` / `OR`), or define a more complex boolean logic query using components.

More details for the Request and the Response structure and meaning are provided on the respective pages.

JSON Schemas will be provided as a normative reference for the request and response.
OpenAPI specifications will be provided, but may be omitted for the initial release.

## Security

This specification is expected to support:
* Open data sources that does not require authentication
* Protected data sources that would would expect authentication tokens in message headers
* The host can decide to support the incoming search request. We will define specific standard HTTP responses to clarify when they do not.
* Details for authentication and authorization will be in header fields. We envision a GA4GH decentralized (as-needed) broker based system that is being developed for this in the future. Till it is ready, we see a simple token exchange model (as MME does) or open (as Beacon) does to get things started.


## API Version

Where `<api-version>` takes the form `vX`, where `X` is a major version. Minor versions are cross-compatible. The target semantic version will be specified within the `question` to help the host identify the target of the `question`. The URL of the host must specify the major version that it supports. 

For example:

`https://yournode.org/v1/search`


## Content type

Content type is expected to be `application/json`

For example:

`Content-Type: application/json`


## Content

* [The structure of the `question` document](search_structure/README.md)
* [The structure of the `result` document](result_structure/README.md)

# Reserved keys and extensions

The API reserves for its use, JSON object keys which begin with an underscore "_" or a dash "-".

The API defined structure allows for clients or servers to define their own proprietary extensions as they see fit, by prefixing JSON object keys with an underscore "_".

A server may wish to reveal additional information about a record or summary information about the collection of records which are not defined by a component. A new component proprietary component can be included by prefixing the given component name with an underscore: `_mySpecialComponent`.

A client and a server may have a special relationship where they want to allow additional information to be searched on, which doesn't require any authentication. The maintainers of the client and the server may agree on new components, but must prefix the component names with underscores.

It is not necessary to also prefix all the fields of a proprietary component with an underscore, as it must be assumed that any JSON data that is a descendant of a key which is prefixed with an underscore is also nonstandard.

It is recommended a JSON Schema be supplied for a proprietary component to formally define the expectations, which may later be added to a repository of official GA4GH components, should the component be seen as useful by the community.

## Example usage

These example constructs are based on use-cases from the driver projects and their feedback: [examples](example_usage/README.md)
