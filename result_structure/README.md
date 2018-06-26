## Response to a search request

This document describes a response that is returned from a search request

Ideas/inspiration _"stolen"_ from [@Relequestual's](https://github.com/Relequestual) [GA4GH Search API Proposal - Components](https://gist.github.com/Relequestual/65c0446944519a66f8562d02b3cb4c86)  and [@Buske's](https://github.com/Buske) [mockup of MME v2](https://github.com/ga4gh/mme-apis/blob/version2-mock/version2/overview.md).


### Specification for response at a high level.

```
{
    "meta": {
        "response": {
            "collectionComponents": {
                "queryIdentification": "1.0.0",
                "disclaimer": "1.0.0",
                "exists": "1.0.0",
                "counts": "1.0.0",
                "records": "1.0.0"
            }
        },

        "request": {
            "componentsUsed": [ "< componentID >", "< componentID >"]
        },

        "provenance": {
            "???": "???"
        }
    },
    "collectionComponents": {

        "queryIdentification" : {
            "queryID" : "<identifier>"
        },

        "disclaimer" : {
            "text": "Disclaimer text...",
            "terms" : "Terms text..."
        },

        "exists": {
            "assertion": true
        },

        "counts": {
            "found": 23,
            "returned": 10
        },

        "records": [

        ]
    }
}
```


### Specification for the `meta` structure (required)

* Contains metadata about the request.


### Specification for the `meta`/`response` structure (required)

* Contains the list of collection components included in the response along with the version number of each of these collection components.

* Having a collection component listed here does not mean it is required in the `collectionComponents` section, however it is required to have a version number for a collection component if it is present in the `collectionComponents` section.

* Questions:
  * Do we want to call this `collectionComponents` or `components`?
  * Version numbers are for components,  but not the records items in the `records` components,do we need those to be versioned as well? For example:

```
{
    "meta": {
        "request": {
            "collectionComponents": {
                "records": {
                    "mme": "1.0.0"
                },
            }
        }
    }
}
```


### Specification for the `meta`/`request` structure (optional)

* This section list the component IDs of the components used to fulfill the search request.

* Questions:
  * Do we want to add information on how the search was run?
    * Were boolean operators used?
    * How many results were found by each component ID (roll into `counts`)?


### Specification for the `meta`/`provenance` structure (optional)

* Questions:
  * What is `provenance`?
  * Does `provenance` belong here or in `collectionComponents`?


### Specification for the `collectionComponents` structure (required)

* This section contains a list of the collection components for the response.


### Specification for the `collectionComponents`/`queryIdentification` structure (required)

* Contains the query identification that was submitted with the search request (required).


### Specification for the `collectionComponents`/`disclaimer` structure (optional)

* The legal disclaimers and terms of use.

* Questions:
  * Do we want to mandate how these are handled by the `host` and the `querier`?
  * The motivation for the above question is to define the boundary between protocol and policy?


### Specification for the `collectionComponents`/`*` structure (optional)

* Initially supported collection components will be:
  * exists
  * counts
  * records

* Initially supported `records` will be:
  * phenopackets
  * mme
  * cloud-dos
  * any GA4GH patient data model

* Questions:
  * Do we want to be able to return arbitrary collection components, for example _mme_ was requested but we only support _exists_?
  * Do we want to require support for a minimal list of components?
  * What is the process for defining new component/record types?
  * Do we want to support defined sets of components/records for certain communities (MME)?
  * Do we allow _private_ components?
  * Do we represent scoring?

