## Result of a search request

This document describes a result that is returned from a search request

this is work in progress...

```
{
    "meta": {
        "response": {
            "collectionComponents": {
                "queryIdentification": "1.0.0",
                "disclaimer": "1.0.0",
                "exists": "1.0.0",
                "count": "1.0.0"
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
        "count": 23
    }
}


### Specification for the `meta` structure (required)

* This section which contains metadata about the request.


### Specification for the `meta`/`response` structure (required)

* This section which contains the list of collection components included in the response along with the version number of each of these collection components.

* Having a collection component listed here does not mean it is required in the `collectionComponents` section, however it is required to have a version number for a collection component if it is present in the `collectionComponents` section.


### Specification for the `meta`/`request` structure (optional)

* This section list the component IDs of the components used to fulfill the search request.


### Specification for the `meta`/`provenance` structure (optional)

* ???


### Specification for the `collectionComponents` structure (required)

* This section which contains a list of the collection components for the response.


### Specification for the `collectionComponents`/`queryIdentification` structure (required)

* This section which contains the query identification that was submitted with the search request (required).


### Specification for the `collectionComponents`/`disclaimer` structure (optional)

* The legal disclaimers and terms of use.


### Specification for the `collectionComponents`/`*` structure (optional)

* Initially supported components will be:
  * exists
  * count
  * phenopackets
  * mme
  * cloud-dos
  * any GA4GH patient data model

