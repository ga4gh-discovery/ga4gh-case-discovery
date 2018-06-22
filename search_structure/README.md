## Search Request

This document describes a search request

this is work in progress....

### Structure

`HTTP POST` request to `<base_remote_url>/search`, with an `application/json` body with the following format:

### Specification for query at a high level

```
{
    "meta": {
        "request": {
            "components": {
                "queryIdentification": "1.0.0",
                "disclaimer": "1.0.0",
                "patientDescription": "1.0.0",
                "submitter": "1.0.0",
                "contact": "1.0.0",
                "search": "1.0.0",
                "-and": "1.0.0",
                "-or": "1.0.0"
            }
        }
    },

    "requires": {
        "response": {
            "components": [{
                "exists": "^1",
                "count": "^1"
            }]
        }
    },

    "components": {

        "queryIdentification" : {
            "queryID" : "<identifier>",
            "queryLabel" : "<identifier>"
        },

        "disclaimer" : {
            "text": "Disclaimer text...",
            "terms" : "Terms text..."
        },

        "patientDescription" : {
            "provenance":{
                "methods":[{
                    "name": "< name of method used to generate this patient data as string>",
                    "version": "<semantic version of method used to generate data>",
                    "documentation": "<string pointing to github | publication | other>"
                }],
                "data":[{
                    "name": "<name of dataset that was used in this patient data as a string>",
                    "version": "<semantic version of data used>",
                    "institution": "<institution data is hosted on>"
                }]
            },
            "ontology" : "<supported types are: phenoPackets | mme | beacon | fhir>",
            "version" : "<semantic version of ontology>",
            "contentType" : "<content type of ontology, for example: 'Content-Type: application/vnd.ga4gh.matchmaker.v1.0+json'">",
            "patient": "<description of patient in ontology specified>"
        },
        "submitter": {
            "id" : "< id > (optional)",
            "name" : "< First [Middle] Last >",
            "email" : "< email >",
            "institution" : "< name of institution >",
            "urls" : "< URLs > (optional)"
        },

        "contact" : {
            "id" : "< ContactPersonID > (optional)",
            "name" : "< First [Middle] Last >",
            "email" : "<email>",
            "institution" : "< name of institution >",
            "urls" : "< URLs > (optional)"
        },

        "search": [
            {
                "componentID": "< unique ID for this component >",
                "type": "gene",
                "source": "HGNC",
                "id": "FOOBAR",
                "description": "<human readable description>"
            },
            {
                "componentID": "< unique ID for this component >",
                "type": "gene",
                "source": "Ensembl",
                "id": "ENSG00000000",
                "operator": "EQ",
                "description": "<human readable description>"
            },
            {
                "componentID": "< unique ID for this component >",
                "type": "variant",
                "referenceName": "13",
                "start": 32936732,
                "referenceBases": "G",
                "alternateBases": "C",
                "assemblyId": "GRCh37",
                "description": "<human readable description>"
            },
            {
                "componentID": "< unique ID for this component >",
                "type": "feature",
                "source": "HPO",
                "id": "HP:0003577",
                "description": "<human readable description>"
            },
            {
                "componentID":" < unique ID for this component >",
                "type": "alleleFrequency",
                "source": "ExAC",
                "population": "ALL",
                "operator": "LT",
                "value": 0.01,
                "description": "<human readable description>"
            }
        ],

        "-or": [
            {
                "-and": {
                    "componentID": [
                        "A",
                        "B"
                    ]
                },
                "componentID": "C"
            }
        ]
    }
}
```

### Specification for the `meta` structure

* This is a required section which contains metadata about the request.


### Specification for the `meta`/`request` structure

* This is a required section which contains the list of components included in the request along with the version number of each of these components.

* (What if there are components here which are not in the search? And vice-versa? What if the version is missing?)


### Specification for the `requires` structure

* This is a required section which contains any requirements for the search.


### Specification for the `requires`/`response` structure

* This is a required section which contains the list of components required in the response along with minimal version for each of the components.


### Specification for the `components` structure (see `meta`/`request`/`components` above)

* This contains a list of the components of the search


### Specification for the `components`/`queryIdentification` structure

* The query identification and query label.

* The query identification should be unique for tracking purposes.

* (The query identification will be needed if we want to have some sort of callback for long running searches)


### Specification for the `components`/`disclaimer` structure

* The legal disclaimers, and terms.


### Specification for the `components``patientDescription` structure (optional)

* This is an optional section to support usage in networks such as the Matchmaker Exchange that require a backing patient to be offered with the query.

* `provenance` is an optional section that is in-place to support the documentation of `methods` and `data` that were used to generate the patient data that is being offered.

* The structure used to describe the patient can be of multiple multiple ontologies or networks such as MME, Beacon, and extensible to others as needed.

* (This needs work)


### Specification for the `components`/`submitter` structure

* The submitter for this search.


### Specification for the `components`/`contact` structure

* The contact for this search if different from the submitter.


### Specification for the `components`/`search` structure

* The search components for this search.

* Each search component will have a unique component ID (unique within the scope of this search request).

* Each search component will have a type which determines what other fields are present. The search component illustrate a sample of components and it not meant to be exhausting.

* Initially supported components will be,
  * gene
  * annotation
  * alleleFrequency
  * consequence
  * deleteriousnessPrediction
  * variant
  * zygosity
  * transcript
  * inheritanceMode
  * feature

* (Each search component needs to be specified, the idea is these can be extended over times).

* (Does version numbering resolve down to the search components? This is not defined in the `meta` section...).


### Specification for the `-or`/`-and` structure

* The section specifies how the search components are combined as a boolean search.




