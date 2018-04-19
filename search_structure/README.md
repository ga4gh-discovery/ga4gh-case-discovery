## Search Request

This document describes a search request

this is work in progress....

### Structure

`HTTP POST` request to `<base_remote_url>/search`, with an `application/json` body with the following format:

### Specification for query at a high level

```
{
  "disclaimer" : {
  	"version": "<semantic version>"
  	"text": "Disclaimer text...",
  	"terms" : : "Terms text...",
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
    "queryOperator": <will be applied to all of the components. Initially will be only: AND | OR >,
    "components": [ "genomicFeature" | "feature" | .. ]
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

* The `queryOperator` is `ALL` and `ANY` and is applied to all `components`.

* `queryOperator`  `ALL` implies: All the components have to be `TRUE` in results.

* `queryOperator`  `ANY` implies: ANY of the components can to be `TRUE` in results. This `ANY` is similar to a `OR`

* `components` initially supported are:  `genomicFeature` (genotype related), `feature` (phenotype related).

* Each `component` has,
	* A `type` to specify the domain (for example genotype, phenotype,..)
	* A domain specific (for example genotype would be different from phenootypes) query criteria. We will initially support `genomicFeature` and `feature` types.
	* A list of `filters` to apply to domain specific general query
	* An `filterOperator` to apply to filters. Initially these will be `ALL` (all filters have to be `TRUE` or `ANY` (where any of the filters can be `TRUE` in to be added to results)
	* For example,
	
	```
		{
		 "type": < genomicFeature | feature | ..>
		 "ontology": <name of ontology used example: HPO will be supported initially >,
		 "genomicFeatureId": <id based on the ontology>,
		 "filters": [ < a list of applicable filters > ],
		}
	
	```


## Specification for the `genomicFeature` structure

* `genomicFeature` query type will initially only be `inheritanceMode ` with filters to refine results further.

* `inheritanceMode ` can have the following `filters`

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
"filterOperator": <"ANY" | "OR" | "LT" | "GT">
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
	

