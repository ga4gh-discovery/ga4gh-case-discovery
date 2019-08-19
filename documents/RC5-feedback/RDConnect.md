# GA4GH Case Discovery implementation within RD-Connect/Solve RD

### Implementation
Currently, in our implementation only the subjectVariant component is supported. If the gene and/or phenotype components are included in the request the server will not process it. Following the specification,
the request/response JSONs are contructed appropriately. The logic object is supported, however for the moment a nested logic object is not supported. An appropriate message is provided in this case. The response includes the counts of the matching records without returning metadata for the records.

We have implemented a RESTful API that queries our local ElasticSearch instance in order to get the subjectVariant-related information. Python framework Flask was used along with with a python Elasticseach client.
Access to the resource is contolled by API keys. 

Future development will support gene-related queries (also to ElasticSearch) and phenotype-related queries (to our local phenotypic database).

### Issues
We didn't face any issues withe the specification.

### Extensions
So far, no extensions have been made to the defined format.
