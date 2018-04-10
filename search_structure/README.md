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
  },
  "patientDescription" : {
	"ontology" : <supported types are: PhenoPackets | MME | Beacon | FHIR >,
	"version" : <semantic version of ontology>,
	"contentType" : <content type of ontology, for example: "Content-Type: application/vnd.ga4gh.matchmaker.v1.0+json">, 
	"patient": <description of patient in ontology specified>
  },
  "queryMetadata" : {
	"queryId" : <identifier>,
	"queryType" : "once"|"periodic",
	"queryStart" : <Date, Time>,
	"queryStop" : <Date, Time>,
	"queryLabel" : <identifier>,
	"maximumNumberOfResultsRequested" : <limits number of results returned; is be superseded by host limits>,
	"queryResultLevel" : "Exists"|"Counts"|"Records", "submitter" : {
	"id" : "SubmitterPersonID",
	"name" : "First [Middle] Last",
	"email" : "",
	"institution" : "AffiliationOfSubmitterPerson", "urls" : ["SubmitterPersonURL",...]
	}
	"contact" : {
	"id" : "ContactPersonID",
	"name" : "First [Middle] Last",
	"email" : "",
	"institution" : "AffiliationOfContactPerson", "urls" : ["ContactPersonURL",...]
	}
  },
  "query": {
    "components": { 
		"genome":[{
					"genomicFeature": 
						{
						"ontology": <example: HPO>,
						"id": <example HPO Id>,
						"label": <text description>
						},
					"operator": <will be applied to whole list of filters>,
					"filters": [<list of filters>,]
				},],
		"features":[{
				"feature":{ 	
					"ontology":<example HPO>,
					"id":<id>,
					"label":<label>,
					},
				"operator": <will be applied to whole list of filters>,
				"filters":[<list of filters to apply to above feature. For example "ageOfOnset">,]
			},]
    		}
  }
}
```

### Specification for the `provenance` structure (optional)

* This is an optional section that is in-place to support the documentation of `methods` and `data` that were used to generate the patient data that is being offered.

* The `API` section describes the API version this query is targeted to

### Specification for the `patientDescription` structure  (optional)

* This is an optional section that is in-place to describe the optional patient data structure that is offered with the query.

* It is meant to support multiple ontologies or networks such as MME, Beacon, and extensible to others as needed.


### Specification for the `components` structure

* `components` can be either `genome` or `features`

## Specification for the `genome` subtype of the `components` structure

* `genome` component can have type 
	`inheritanceMode `
 
 ```
{
"ontology": <name of ontology used example: HPO>,
"id": <id>,
"label" :<label>
}
```


* `inheritanceMode ` of the `genome` component can have the following `filters`

	` annotation | alleleFrequency | consequence | deleteriousnessPrediction | variant | zygosity`
	

## Specification for the `features` subtype of the `components` structure

* The `features` field is a list of feature and filters to apply to that feature, combinations relevant to phenotypes
```
"features":[
{
	"feature":{},
	"filters":[{},..]
},]
```

### Supported `operators`

* Will be applied to whole list of filters

```
"operator": <"ANY" | "OR" | "LT" | "GT">
```


### Specification for the `filters` structure

 * A filter has the following structure
 
 ```
{
"ontology": <name of ontology/source used example: HPO or HGNC or ENSG etc>,
"annotation": <type example: annotation or gene>,
"terms" : [ <a list of terms. This could HPO, gene IDs etc. Each has a "id" and "label" field>]
}
 ```

### Specification for the `term` structure

* `gene`: the `id` must be an exact match
	```
	{
	"ontology":<ontology of term. Example: HGNC>,
	"id":<id>,
	"label":<label>
	}
	```
* `zygosity`
	```
	{
	"ontology":<ontology of term. Example: SO code>,
	"id":<id>,
	"label":<label>
	}
	```
* `consequence`
	```
	{
	"ontology":<ontology of term. Example: SO code>,
	"id":<id>,
	"label":<label>
	}
	```
* `variant`: 
```
{
"assembly": "",
"referenceName": "",
"start":"",
"end": "",
"referenceBases": ",
"alternateBases": ""
}
```
	

