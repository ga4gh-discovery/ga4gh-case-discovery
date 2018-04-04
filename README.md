# Global Alliance for Genomics and Health Discovery Search API

A standard for a global federated data sharing network that allows the querying, and subsequent -optional- processing of the results on a cloud environment

This standard is work in progress, and built using existing work and contributions by:

* Global Alliance for Genomics and Health Discovery Search API team
* Anthony J. Brookes and his team at the Cafe Variome discovery platform
* Orion Buske (@buske, https://github.com/ga4gh/mme-apis/blob/version2-mock/version2/overview.md)
* The Matchmaker Exchange APIs  (http://www.matchmakerexchange.org, https://github.com/ga4gh/mme-apis/blob/master/search-api.md)


# OVERVIEW

**Submit patient search request:**
`HTTP POST` to remote server: `<base_remote_url>/<api-version>/search`
For example: `https://yournode.org/v0.1/search`


## Security

This specification is expected to support:
* Open data sources that does not require authentication 
* Protected data sources that would would expect authentication tokens in message headers
* The host can decide to support incoming search request

## API Versioning

Where `<api-version>` takes the form `vX.Y`, where `X` is a major version and `Y` is a minor version. Minor versions are cross-compatible. 

For example:

`https://yournode.org/v0.1/search`


## Content type

Content type is expected to be `application/json`

For example:

`Content-Type: application/json`

## Search Request

`HTTP POST` request to `<base_remote_url>/search`, with an `application/json` body with the following format:

### Specification for query

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
      "genome": {
        "inheritanceMode": {
          "terms": [<HPO terms that describe inheritance>,]
        }
        "filters": [ annotation | alleleFrequency | consequence | gene | phenotype ]
      }
    }
  }
}
```

