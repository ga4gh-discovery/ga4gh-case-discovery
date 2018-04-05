# GA4GH Discovery Search API

A standard for a global federated data sharing network that allows the querying, and subsequent -optional- processing of the results on a cloud environment.

Please note this standard is work in progress.

This standard was built using existing work, and contributions by:

* Global Alliance for Genomics and Health Discovery Search API team
* Anthony J. Brookes and his team at the Cafe Variome discovery platform
* Orion Buske (@buske, https://github.com/ga4gh/mme-apis/blob/version2-mock/version2/overview.md)
* The Matchmaker Exchange APIs  (http://www.matchmakerexchange.org, https://github.com/ga4gh/mme-apis/blob/master/search-api.md)


# Concept

We would define the two parties involved as the `host` and the `querier`. 

1. The querier would would pose a question to the host. We will define this as the `question`
2. The `question` will contain an `optional` patient data structure. While the Matchmaker Exchange (MME) requires a patient to be offered with any query, other networks may not. Therefore in order to make the standard general, we propose this value be `optional` and have the host decide whether to support it.
3. The host uses the `query` section to determine the set of results to return. We will define the structure it returns as the `result`
4. Within the `query` section there are components. Some are used for searching, and some are `filters` used for sieving search results further.


# OVERVIEW

**Submit patient search request:**
`HTTP POST` to remote server: `<base_remote_url>/<api-version>/search`
For example: `https://yournode.org/v0.1/search`


## Security

This specification is expected to support:
* Open data sources that does not require authentication 
* Protected data sources that would would expect authentication tokens in message headers
* The host can decide to support incoming search request

## API Versioning

Where `<api-version>` takes the form `vX.Y`, where `X` is a major version and `Y` is a minor version. Minor versions are cross-compatible. 

For example:

`https://yournode.org/v0.1/search`


## Content type

Content type is expected to be `application/json`

For example:

`Content-Type: application/json`

## Content

* [The structure of the `question` document](search_structure/README.md)
* [The structure of the `result` document](result_structure/README.md)