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
    "components": { 
		"genome":{
			"inheritanceMode": [
					{
					"ontology": <example: HPO>,
					"id": <example HPO Id>,
					"label": ""
					},],
			"filters": [<list of filters>,]
				},
		"features":[{
				"feature":{ 	
					"ontology":<example HPO>,
					"id":<id>,
					"label":<label>,
					}
				"filters":[<list of filters to apply to above feature. For example "ageOfOnset">,]
			},]
    		}
  }
}
```


### Specification for the `components` structure

* `components` can be either `genome` or `features`

## Specification for the `genome` subtype of the `components` structure

* `genome` component can have type 
	`inheritanceMode `

* `inheritanceMode ` of the `genome` component can have the following `filters`

	` annotation | alleleFrequency | consequence | deleteriousnessPrediction | variant | zygosity`
	

## Specification for the `features` subtype of the `components` structure

* The `features` field is a list of feature + filter combinations relevant to phenotypes


### Specification for the `filters` structure

 * A filter has the following structure
 
 ```
 	{
 		"ontology": <name of ontology/source used example: HPO or HGNC or ENSG etc>,
 		"annotation": <this could be "gene" for example,
        	"operator": "ANY" | "OR" | "LT" | "GT"
 		"terms" : [ <a list of terms. This could HPO, gene IDs etc. Each has a "Id" and "Label" field>]
 
 ```



	

