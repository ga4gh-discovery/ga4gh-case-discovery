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

# Preface

The Matchmaker Exchange (MME) API defined a JSON Structure representing a patient, which was used to perform a "query by example" as a federated request across a network of servers (or nodes). 

While the MME network was designed to allow for easy federated case matching, due to the complexity of the problem, it is limited by design to allow each server to determine what best constitutes a match. This functionality is useful for databases of siloed cases, but it doesn't allow for broader, specifically targeted searching of cases, which can fulfil many more clinical and scientific use cases.

A new standard for case discovery, the Search API. A robust and flexible approach, considering databases which contain many types of case associated data, without sacrificing the huge power of unilateral federated searching underpinned by an interoperable format for query and response.

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


## Example usage

These example constructs are based on use-cases from the driver projects and their feedback: [examples](example_usage/README.md)
