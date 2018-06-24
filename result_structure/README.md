## Result of a search request

This document describes a result that is returned from a search request

This is work in progress...

```
{
    "meta": {
        "response": {
            "collectionComponents": {
                "queryIdentification": "1.0.0",
                "disclaimer": "1.0.0",
                "exists": "1.0.0",
                "counts": "1.0.0"
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
            "returned": 23
        }
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


### Specification for the `collectionComponents`/`*` structure (optional)

* Initially supported components will be:
  * exists
  * counts
  * phenopackets
  * mme
  * cloud-dos
  * any GA4GH patient data model

* Questions:
  * Do we want a results count limit?
  * Do we want to support offsets in retrievals (start and limit)?
  * Do we want to require support for a minimal list of components?
  * What is the process for adding components?
  * Do we want to support defined sets components for certain communities (MME)?
  * Do we allow custom components?


