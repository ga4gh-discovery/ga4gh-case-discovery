# GA4GH Case Discovery implementation within Café Variome
_Colin Veal, Dhiwagaran Thangavelu, Vagelis Ladas, Marc Wadsley, Tim Beck, Anthony Brookes_

[Café Variome] is designed to discover ‘entities’ across a federated network using Boolean queries on various diverse attributes. Though it is optimised for fast search it can perform some analyses and data extraction. Any API it uses needs to allow complex logical queries across many different types of and datatypes.

### Implementation
The overall JSON structure, the use of components and logic was relatively easy to utilise into our existing code.

As each component is a self-contained query entity it was easy to translate to our underlying models and each component can be handled separately rather than having to parse the entire query and semantically identify each query entity. 

The component format has made it simplistic to add new components and integrate them into the complex logic using json pointers.  For example, we have added an EAV component (parameters: Attribute, Operator, Value) and a phenotype similarity matching component (parameters: list of HPO terms, similarity method, operator, similarity score).  

We process each component separately based on its type (phenotype, variant, …) allowing us to use different database types, which we can easily switch or add to.  Currently we use the following:

* EAV - [Elasticsearch]
* Phenotype - [Elasticsearch] or [Neo4j]
* Similarity Matching - [Neo4j]
* Variants - [bcftools] or [Elasticsearch]

The Boolean logic is handled the same way regardless of which engine is used for each component.

### Issues
- The initial list of components was limited
- Café Variome can query across different datasets with different access levels, the current response type is too restrictive.

### Extensions and Changes
Added a bearer token and token issuer section to allow us to use Cafe Variome access controls


```json
{
	"cv_security": {
		"user_token": "efeffeger1213",  # bearer token
		"network_key": "577995aef9ee9baf26f87da37f",  // Cafe Variome specific information
		"installation_key": "gtjh435622e",			// Cafe Variome specific information
		"authentication_server": "https://keycloak.cv.org"  // token issuer - whitelisted locally
	}
}
```

Added phenotype similarity matching component

```json
{
	"sim": {
		"method" : "relative",
		"r-paramter" : 0.7,
		"s- paramter" : 1,
		"hpo_terms" : [
			"HP:0000234",
			"HP:0000486",
			"HP:0001263"
		]
	}
}

```

Added EAV query component, this takes any attribute and value with operators: `IS, IS LIKE, IS NOT, IS NOT LIKE, =, !=, <, >, <=, >=`

```json
{
	"eav": [
		{
			"attribute": "MMSE",
			"operator": ">",
			"value": "20"
		},
		{
			"attribute": "diagnosis",
			"operator": "is not",
			"value": "AD"
		}
	]
}
```

The response is returned as an array of datasets with indication of the user/dataset access level. 

```json
{
	"sources": {
		"dataset1_user_permitted": { //name of dataset
			"status": "permitted", //status indicates a users permission level
			"count": 0
		},
		"dataset2_user_permitted": {
			"status": "permitted",
			"count": 100
		},
		"dataset3_counts_under_threshold": {
			"status": "threshold"
		},
		"dataset4_user_not_permitted": {
			"status": "restricted"
		},
		"linked_dataset5": {
			"linked": "https://external.link.org"
		}
	}
}
```




   [Elasticsearch]: <https://elastic.co>
   [Neo4j]: <https://neo4j.com>
   [bcftools]: <https://https://samtools.github.io/bcftools>
   [Café Variome]: <https://www.cafevariome.org>

