# Search Request

This document describes a search request made by a client to a server.


# Format

A search request is an HTTP POST request from a client to a server.

An `HTTP POST` request to `<base_remote_url>/search`, with an `application/json` body with the following format:


## Minimal exampel JSON body - Simplest query


```javascript
{
    "meta": {
        "request": {
            "components": {
                "gene": "1.0.0",
            }
        },
        "apiVersion": "1.0.0"
    },

    "query": {
        "components": {
            "gene": [{
                    "ensemblID": "ENSG00000139618",
                }],
        }
    }
}
```

This is possibly the most simple search request the search API is capable of expressing.

The request payload will be refurred to as "the instance".


The JSON instance is an object, and is required to have a `meta` and `query` object.

### Meta object

The `meta` object allows the client to tell the server information about the request it's making.

The `apiVersion` property defines a string which represents the Search API version format being used.

The `request` object contains a `components` object, where each key represents a component name, where the value is the version of the component used in the request.
This includes components in the `query` and any `meta` components used, if any.

The `meta` object is required.

### Query object

The `query` object specifies the question the client is asking.

It contains a `components` object, where the keys are the component name, and the values are an array of objects which represent that component type.
In the basic example, the `gene` component has one object, which represents BRCA2 in the form of an Ensembl Gene ID.

The `query` object is required.

## More advanced query


```javascript
{
    "meta": {
        "request": {
            "components": {
                "gene": "1.0.0",
                "subjectVariant": "1.0.0",
                "phenotype": "1.0.0",
                "queryIdentification": "1.0.0"
            }
        },
        "apiVersion": "1.0.0",
        "components": {
            "queryIdentification" : {
                "queryID" : "abc123",
                "queryLabel" : "Search from client X for user on [date]"
            }
        }
    },

    "requires": {
        "response": {
            "components": {
                "exists": "1",
                "count": "1"
            }
        }
    },

    "query": {
        "components": {
            "gene": [{
                    "ensemblID": "ENSG00000139618",
                    "hgncName": "BRCA2"
                }],
            "subjectVariant": [{
                    "referenceName": "13",
                    "start": 32936732,
                    "referenceBases": "G",
                    "alternateBases": "C",
                    "assemblyId": "GRCh37"
                }],
            "phenotype": [{
                    "id": "HP_0000765",
                    "observation": "no",
                    "label": "Abnormality of the thorax"
                }]
        }
    },
    "logic": {
        "-AND": [
            "/query/components/gene/0",
            "/query/components/subjectVariant/0",
            "/query/components/phenotype/0"
        ]
    }
}
```

That's a bit more JSON than the minimal example, but we're asking a more specific question.

### Meta object

The `meta` object includes a `components` object, where the keys are component names and the values are the component data.
These are Request Meta components, and allow the client to express things to the server about the request.
In this example, the client has idenitifed this query by an ID which the server may reference.


### Requires object

The `requires` object allows the client to specify what components it requires in response for the response to be useful.
As the expectation is that this API will be used in a federated method, making the same request to multiple servers without discression, it follows that not all servers will contain or be able to share the data the client needs to process the results, or what the clients users will expect.

In this example, the client is requesting that the `exists` and `count` components exist. This is quite minimal, as these are both Collection components, and it's possible that no record data might actually be included in the servers response.

A client must also specify a version for the required components, in the format of an [X-Range](https://docs.npmjs.com/misc/semver#x-ranges-12x-1x-12-) string.
If no specific version is required, simply "x" or "\*" represents "any version".

### Logic object

The `logic` object allows the client to specify more complex boolean query logic.

---WIP---


# Acknowledgments

Ideas/inspiration from:
 - [@Relequestual's](https://github.com/Relequestual) [GA4GH Search API Proposal - Components](https://gist.github.com/Relequestual/65c0446944519a66f8562d02b3cb4c86) 
 - [@Buske's](https://github.com/Buske) [mockup of MME v2](https://github.com/ga4gh/mme-apis/blob/version2-mock/version2/overview.md)

