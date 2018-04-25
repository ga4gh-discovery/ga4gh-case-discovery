# GA4GH Discovery Search API Usage Examples

These examples were gathered from the Discovery WS driver project community and can be found [here.](https://docs.google.com/spreadsheets/d/1Vxfo7hssqsMtJeLUWTj5HRTAkE0q8bcl10V_N-SjwpE/edit#gid=0)

These are still in progress..more to come!

## Use case example 1: Trio Analysis	
```

	Query:
		Filter variants by AF (population and in-house databases) based on inheritance models (de novo dominant AF<0.001, autosomal recessive AF<002: compound heterozygous [het in each parent or in one parent and de novo on second allele], homozygous; possibly heterozygous AF<0.001*), high and medium impact variants, variant present in known human disease gene		
	
	Answer format:
		Subset of variants for phenotype-genotype assessment	
	
	Context:
		Private VCF	
	
	Submitter: 
		Lisa Ewans (via Marcel Dinger @ Garvan)		
																					
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
	"queryLabel" : "search by allele frequencies",
	"maximumNumberOfResultsRequested" : 10,
	"queryResultLevel" :  "Records",
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
    "queryOperator": "AND",
    "components": [ "genomicFeature" ,
    				"genomicFeature" ,
    			 .. ]
  }
}

```
