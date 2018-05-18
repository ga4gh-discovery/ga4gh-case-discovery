# GA4GH Discovery Search API Usage Examples

These examples were gathered from the Discovery WS driver project community and can be found [here.](https://docs.google.com/spreadsheets/d/1Vxfo7hssqsMtJeLUWTj5HRTAkE0q8bcl10V_N-SjwpE/edit#gid=0)

These are still in progress..more to come!

## Use case example 1: Query on if a case exists with a variant in the given gene
```

	Query:
		Do you have an entry with this gene?		
	
	Answer format:
		yes | no	

	Submitter: 
		Nara Sobreira (GeneMatcher, Johns Hopkins)		
																					
```

The `query` can be represented by,

```
{
  "target-api": v1.0.0,
  "disclaimer" : {
  	"version": "v0.0.1",
  	"text": "some disclaimer text from the queries",
  	"terms" : "Terms of usage are.."
  },
  
  "patientDescription" : { .... },
  
  "queryMetadata" : {
	"queryId" : 1,
	"queryLabel" : "search by gene name",
	"maximumNumberOfResultsRequested" : 10,
	"submitter": {,
		"name" : "John Genome",
		"email" : "johh@genome.org",
		"institution" : "Genome Center" 
	},
	"contact" : {
		"name" : "John Genome",
		"email" :"johh@genome.org",
		"institution" : "Genome Center"
	}
  },
  "query": {
    "queryOperator": {
    	"ALL":["A"]
    }
    "components": [
    	 {
	 "component_id": "A",
	 "ontology":"ENSEMBL",
	 "id":"ENSG00000155657",
	 "label":"TTN"
 	}
    ]
  }
}

```


A the simplest, with minimum required values, the `reply` would be, (assuming there was a patient, with a variant in the gene specified)

```

{
  "reply": {
   "provenance":{},
   "disclaimer" : {
		"version": "v0.0.1"
		"text": "we hereby..",
		"terms" : "the terms are such..",
 	 },
   "results":{
   	"exists": "true",
	"count": 1,
	"records": []
   }
}

```


## Use case example 2: Query on variant and gene
```

	Query:
		do you have an entry with variant A320T in PCYT1A?		
	
	Answer format:
		yes | no  AND  url

	Submitter: 
		Nara Sobreira (GeneMatcher, Johns Hopkins)		
																					
```

The `query` can be represented by,

```
{
  "target-api": v1.0.0,
  "disclaimer" : {
  	"version": "v0.0.1",
  	"text": "some disclaimer text from the queries",
  	"terms" : "Terms of usage are.."
  },
  
  "patientDescription" : { .... },
  
  "queryMetadata" : {
	"queryId" : 1,
	"queryLabel" : "search by gene name",
	"maximumNumberOfResultsRequested" : 10,
	"submitter": {,
		"name" : "John Genome",
		"email" : "johh@genome.org",
		"institution" : "Genome Center" 
	},
	"contact" : {
		"name" : "John Genome",
		"email" :"johh@genome.org",
		"institution" : "Genome Center"
	}
  },
  "query": {
    "queryOperator": {
    	"ALL":["A","B"]
    }
    "components": [
    	 {
	 "component_id": "A",
	 "ontology":"ENSEMBL",
	 "id":"ENSG00000161217",
	 "description":"PCYT1A"
 	},
	{
	 "component_id": "B",
	 "ontology":"hgvs",
	 "id":"A320T",
	 "description":""
 	}
    ]
  }
}

```


A the simplest, with minimum required values, the `reply` would be, (assuming there was a patient, with a variant in the gene specified)

```
construction in progress..

{
  "reply": {
   "provenance":{},
   "disclaimer" : {
		"version": "v0.0.1"
		"text": "we hereby..",
		"terms" : "the terms are such..",
 	 },
   "results":{
   	"exists": "true",
	"count": 1,
	"records": []
   }
}

```
