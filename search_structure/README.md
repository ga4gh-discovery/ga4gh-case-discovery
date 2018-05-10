## Search Request

This document describes a search request

this is work in progress....

### Structure

`HTTP POST` request to `<base_remote_url>/search`, with an `application/json` body with the following format:

### Specification for query at a high level

```
{
  "target-api": <semantic version of API  this query is formatted for>,
  "disclaimer" : {
  	"version": "<semantic version>",
  	"text": "Disclaimer text...",
  	"terms" : "Terms text..."
  },
  "patientDescription" : {
	 “provenance”:{	
			“methods”:[
				  {
					“name”:< name of method used to generate this patient data as string>,
					“version”:<semantic version of method used to generate data>,
					“documentation”:<string pointing to github | publication | other>,
				    },
				],
			“data”:[
				{
					“name”:<name of dataset that was used in this patient data as a string>,
					“version”:<semantic version of data used>,
					“institution”:<institution data is hosted on>	
				},
				]
	  },
	"ontology" : <supported types are: phenoPackets | mme | beacon | fhir >,
	"version" : <semantic version of ontology>,
	"contentType" : <content type of ontology, for example: "Content-Type: application/vnd.ga4gh.matchmaker.v1.0+json">, 
	"patient": < description of patient in ontology specified >
  },
  "queryMetadata" : {
	"queryId" : <identifier>,
	"queryLabel" : <identifier>,
	"maximumNumberOfResultsRequested" : <limits number of results returned; is be superseded by host limits>,
	"submitter": {
	    "id" : < id > (optional),
		"name" : < First [Middle] Last >,
		"email" : < email >,
		"institution" : < name of institution > , 
		"urls" : < URLs > (optional)
	},
	"contact" : {
		"id" : < ContactPersonID > (optional),
		"name" : < First [Middle] Last >,
		"email" : <email>,
		"institution" : < name of institution >, 
		"urls" : < URLs > (optional)
	}
  },
  "query": {
    "queryOperator": { 
    	"ALL":[ <a list of components that ALL have to be `true` in results> ],
	"ANY":[ <a list of components of which ANY can to be `true` in results> ],
    }, 
    "components": [ will be different types of components. ex: gene | feature | variant | .. ]
  }
}
```

### Specification for the `disclaimer` structure

* The legal disclaimers, their versions, from querier

### Specification for the `patientDescription` structure (optional)

* This is an optional section to support usage in networks such as the Matchmaker Exchange that require a backing patient to be offered with the query.

* `provenance` is an optional section that is in-place to support the documentation of `methods` and `data` that were used to generate the patient data that is being offered.

* The structure used to describe the patient can be of multiple multiple ontologies or networks such as MME, Beacon, and extensible to others as needed. 

### Specification for the `queryMetadata` structure

* This define details of the query and the fields are self-explanatory and `required`


### Specification for the `query` structure

* The query structure has an `queryOperator` field, and a `components` field

* The `queryOperator` is a structure that applies boolean logic to individual `components`. Initially will only support `ALL` and `ANY`.

* `queryOperator`  field `ALL` list implies: ALL the component IDs in the list have to be `TRUE` in results.

* `queryOperator`  `ANY` implies: ANY of the component IDs in the list can be `TRUE` in results. This `ANY` is similar to a `OR`

* `components` will be of different types descriptions of genotypes of phenotypes that can be used to find results using standard ontologies.

* Each `component` has,
	* A `type` to specify the domain (for example 'gene', 'feature', 'variant', etc,..)
	* A `id` that will allow it to have boolean logic applied to it in the `queryOperator` field.
	* A domain specific (for example genotype would be different from phenootypes) query criteria. 



## Specifications for the `components` structure

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
	  `
	
* Of these, the `variant` structure would be: 
	```
		{
		"type": "variant",
		"id":"",
		"assembly": "",
		"referenceName": "",
		"start":"",
		"end": "",
		"referenceBases": "",
		"alternateBases": "",
		"description": ""
		}
	```

* The others,
	
	```
		{
		 "type": < gene | feature | zygosity | ..>,
		 "ontology": <name of ontology used example: HPO will be supported initially >,
		 "id": <id based on the ontology>,
		 "description": <human readable description>
		}
	```





	

