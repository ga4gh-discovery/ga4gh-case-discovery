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
* Added a bearer token and token issuer section to allow us to use Cafe Variome access controls
* The response is returned as an array of datasets
* Added codes to indicate a user's access level per dataset
* Added components for EAV and similarity matching


   [Elasticsearch]: <https://elastic.co>
   [Neo4j]: <https://neo4j.com>
   [bcftools]: <https://https://samtools.github.io/bcftools>
   [Café Variome]: <https://www.cafevariome.org>

