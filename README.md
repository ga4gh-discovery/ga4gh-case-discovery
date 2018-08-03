# GA4GH Discovery Search API

A standard for a global federated data sharing network that allows the querying, and subsequent -optional- processing of the results on a cloud environment.

Please note this standard is work in progress.

This generalized standard was inspired, adapted, and built up from existing work by:

* Global Alliance for Genomics and Health Discovery Search API team
* Requirements, design, brainstorming, and ideas gathered from the GA4GH driver project community, summerized [here](https://docs.google.com/document/d/1jPPVhSvmzW5kK_rKTxkjPvxlcGicHhHPgO4cqqP3CBU/edit?ts=5a84936c#heading=h.4lfahcth694x).
* Orion Buske (@buske) and Ben Hutton (@relequestual) - [MME V2 Proposal](https://github.com/ga4gh/mme-apis/blob/version2-mock/version2/overview.md)
* GePh-Query API ("Jeff") by Anthony J. Brookes and his team at the [Cafe Variome](https://www.cafevariome.org) discovery platform
* The merging of concepts, content, and building of first draft by Harindra Arachchi @harindra-a
* The Matchmaker Exchange APIs [@github](https://github.com/ga4gh/mme-apis/blob/master/search-api.md)


# Concept

We define the two parties involved as the `host` and the `querier`. 

1. The querier would pose a question regarding a patient to the host. We will define this as the `question`
2. The `question` will contain an `optional` patient data structure. While the Matchmaker Exchange (MME) requires a backing patient to be offered with any query, other networks may not. Therefore in order to make the standard general, we propose this value be `optional` and have the host decide whether to support that specific question. When they do not support, they will return a HTTP message such as `Not supported (status code: 512)`
3. The host uses the `query` section to find a set of results to return. We will define the structure it returns as the `result`
4. Within the `query` section there is a `components` section and an `operator` section that is applied to all the components.
5. `components` are used as the search criteria. Each `component` has one or more associated `filters` that apply specific rules to limit/sieve the search results of that specific `component` further
6. The object of the `operator` section is to designate whether `ALL` or `ANY` of the components to use in the query.
7. Within the top level `queryMetadata` structure, along with query details for tracking, there is field named `maximumNumberOfResultsRequested` that allows the querier to specify a limit on the number of results returned. This value can be superseded by any limit the host is able to support
8. The return results maybe scored, and/or sorted, by some host-internal mechanism, as expected from a typical database query, but it is not expected or guaranteed.


# OVERVIEW

**Submit patient search request:**
`HTTP POST` to remote server: `<base_remote_url>/<api-version>/search`
For example: `https://yournode.org/v1/search`


## Security

This specification is expected to support:
* Open data sources that does not require authentication
* Protected data sources that would would expect authentication tokens in message headers
* The host can decide to support the incoming search request. We will define specific standard HTTP responses to clarify when they do not.
* Details for authentication and authorization will be in header fields. We envision a GA4GH decentralized (as-needed) broker based system that is being developed for this in the future. Till it is ready, we see a simple token exchange model (as MME does) or open (as Beacon) does to get things started.

# API Version

The API version will follow the rules set out by [Semantic Versioning 2.0.0](https://semver.org/spec/v2.0.0.html).

    Given a version number MAJOR.MINOR.PATCH, increment the:

    MAJOR version when you make incompatible API changes,
    MINOR version when you add functionality in a backwards-compatible manner, and
    PATCH version when you make backwards-compatible bug fixes.

    Additional labels for pre-release and build metadata are available as extensions to the MAJOR.MINOR.PATCH format.

The MAJOR version MUST be included in the URL exposed by servers for the API, in the format of `vX`, where `X` is a major version. 

The version of the API is linked to the request and response structure, and not any individual components used with the request and response, which have their own associated semantic version number.

An example API URL: `https://yournode.org/v1/search`




## Content type

Content type is expected to be `application/json`

For example:

`Content-Type: application/json`


## Content

* [The structure of the `question` document](search_structure/README.md)
* [The structure of the `result` document](result_structure/README.md)


## Example usage

These example constructs are based on use-cases from the driver projects and their feedback: [examples](example_usage/README.md)
