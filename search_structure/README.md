## Search Request

This document describes a search request

this is work in progress....

### Structure

`HTTP POST` request to `<base_remote_url>/search`, with an `application/json` body with the following format:

### Specification for query at a high level

```
{

  “provenance”:{
		“api”:{
			“version”:<semantic version of targeted API>,
		}	
		“methods”:[
			  {
				“name”:<method name as string>,
				“version”:<semantic version of method used to generate data>,
				“documentation”:<string pointing to github | publication | other>,
			    },
			],
		“data”:[
			{
				“name”:<name of dataset>,
				“version”:<semantic version of data used>,
				“institution”:<institution data is hosted on>	
			},
			]
  },
  "disclaimer" : {
  	"version": "<semantic version>"
  	"text": "Disclaimer text...",
  	"terms" : : "Terms text...",
  }
  "patientDescription" : {
	"ontology" : <supported types are: PhenoPackets | MME | Beacon | FHIR >,
	"version" : <semantic version of ontology>,
	"contentType" : <content type of ontology, for example: "Content-Type: application/vnd.ga4gh.matchmaker.v1.0+json">, 
	"patient": <description of patient in ontology specified>
  }
 "meta":{
	"maximumNumberOfResultsRequested" : <limits number of results returned; is be superseded by host limits>,
  }
  "query": {
    "components": { < genome | features |  >}
  }
}
```


### Specification for the `componenents` structure

* `componenents` can be either `genome` or `features`

### Specification for the `genome` subtype of the `componenents` structure

* `genome` component can have types 
	`inheritanceMode | gene | ..`

* `genome` component can have the following `filters`

	` annotation | alleleFrequency | consequence  `

