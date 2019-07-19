# CaseLog implementation and extension to Case Discovery API 1.0.0-RC5


CaseLog aims to provide a genomics search engine to jointly query genotypic and phenotypic data from human genomes. On top of the proprietary CaseLog data store, a layer of RESTful APIs provides fast, read-only access to search for individual variants, genes, and samples and compute statistics such as allele frequencies, case-versus-control tables, and genetic burden tests. All queries that return summary statistics can be federated across participating studies to potentially increase statistical power.

CaseLog’s main use cases include queries such as finding all patients with a particular disease or phenotype, and carrying a specific variant, or one of a set of variants. Patients can be described in terms of disease(s)/phenotype(s), age, sex, and other clinical attributes. Variants can be described in terms of genomic, coding sequence, or protein change; genotype; and mode of inheritance. Another use case for CaseLog is to find candidate genes for a given disease or phenotype. Those candidate genes are identified by a high mutational burden in the local or distributed cohort, pathogenic or de novo variants.



## GA4GH Case Discovery API
We have implemented a translational layer that maps the GA4GH Case Discovery API to pre-existing, internal RESTful CaseLog APIs. We control access to the external-facing Case Discovery using API keys that will be distributed to clients upon request. For the most part, this adaption necessitated only a straight-forward mapping/splitting of JSON field names and the restructuring of input/output JSON. The nature of CaseLog’s overall aim to compute summary statistics and allow for meta-analysis, required the addition of private extensions explaining some metadata about each cohort being queried, including total counts and counts of non-matching cases.

## Private extensions
### Metadata:
-	for each query, include the total number of samples; or count of non-matching samples;
-	return the counts of matching/non-matching samples grouped by phenotype

### Data on matches:
-	zygosity
-	de novo or inherited variant?
-	variant considered (likely) pathogenic, benign, unknown?

### Extension for the MME use case:
-	requestor information (e-mail, institution)
-	data owner information (e-mail, institution, study/cohort)


Currently implemented private extensions to identify requestors/data owners, as implemented first by Variant Matcher and then by CaseLog:
```json
"meta": {
    "request": {
      "components": {
        "search": {
          "subjectVariant": "1.0.0",
          "phenotype": "1.0.0",
          "gene": "1.0.0"
        },
        "requestMeta": {
          "_querycontact": "1.0.0",
          "queryIdentification": "1.0.0"
        }
      }
    },
    "apiVersion": "1.0.0",
    "components": {
      "_queryContact": {
        "_email": "requestor@requestor.com",
        "_fullName": "Bob Requestor",
        "_id": "1",
        "_url": "http://www.requestor.com/mystudy",
        "_role": "Researcher",
        "_institution": "Request, Inc."
      },
      "queryIdentification": {
        "queryID": " MyQueryID_12345",
        "queryLabel": "MyQueryID 12345 2019-07-15:1 10:55:55 UTC"
      }
    }
  }, ...
```
