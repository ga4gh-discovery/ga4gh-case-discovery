# GA4GH Discovery Search API Usage Examples

These examples were gathered from the Discovery WS driver project community and can be found [here.](https://docs.google.com/spreadsheets/d/1Vxfo7hssqsMtJeLUWTj5HRTAkE0q8bcl10V_N-SjwpE/edit#gid=0)

These are still in progress..more to come!

## Use case example 1: Query on if a case exists with a given gene
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
  
  "patientDescription" : {},
  
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
    "queryOperator": "ALL",
    "components": [
    	
    ]
  }
}

```
