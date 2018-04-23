## Result of a search request

This document describes a result that is returned from a search request

this is work in progress...

```
{
  "reply": {
    “provenance”:{
		“api”:{
			“version”:<what version of API generated this>,
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
   "results":[{
		"ontology": < phenopackets | mme | yes-no | cloud-dos >,
		"result": <result in one of the above ontologies>
    },]
  }
}
```
