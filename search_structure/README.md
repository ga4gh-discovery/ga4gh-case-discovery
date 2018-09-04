# Search Request

This document describes a search request made by a client to a server.


# Format

A search request is an HTTP POST request from a client to a server.

An `HTTP POST` request to `<base_remote_url>/search`, with an `application/json` body with the following format.


## Minimal exampel JSON body - A simple query


```javascript
{
    "meta": {
        "request": {
            "components": {
                "search": {
                    "gene": "1.0.0"
                }
            }
        },
        "apiVersion": "1.0.0"
    },
    "query": {
        "components": {
            "gene": [
                {
                    "ensemblID": "ENSG00000139618"
                }
            ]
        }
    }
}
```

This is a simple search request in the search API format.


The JSON instance (or payload) is an object, and is required to have a `meta` and `query` object.

### Meta object

The `meta` object allows the client to tell the server information about the request it's making.

The `apiVersion` property defines a string which represents the Search API version format being used.

The `request` object contains a `components` object. This is further explained in the more advanced query below.

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
                "search": {
                    "gene": "1.0.0",
                    "subjectVariant": "1.0.0",
                    "phenotype": "1.0.0"
                },
                "requestMeta": {
                  "queryIdentification": "1.0.0"
                }
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
                    "assemblyID": "GRCh37"
                }],
            "phenotype": [{
                    "id": "HP:0000765",
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

The `request` object in the `meta` object provides version information about the components used in the request.
Each key is one of the component types found in a request (`search` or `requestMeta`).
The values of these keys are objecets where the keys are component names and the values are the component data.

### Requires object

The `requires` object allows the client to specify what components it requires in response for the response to be useful.
As the expectation is that this API will be used in a federated method, making the same request to multiple servers without discression, it follows that not all servers will contain or be able to share the data the client needs to process the results, or what the clients users will expect.

In this example, the client is requesting that the `exists` and `count` components exist. This is quite minimal, as these are both Collection components, and it's possible that no record data might actually be included in the servers response.

A client must also specify a version for the required components, in the format of an [X-Range](https://docs.npmjs.com/misc/semver#x-ranges-12x-1x-12-) string.
If no specific version is required, simply "x" or "\*" represents "any version".

### Logic object

The `logic` object allows the client to specify more complex boolean query logic.

Omitting the `logic` key is the same as defining "AND" for all components in the query.

Support for boolean logic oporators is recommended. If a server chooses not to implement boolean logic, the server must respond to any request containing boolean logic with HTTP status code `422 Unprocessable Entity`, and may include a message body to indicate the type of error.

The value of `logic` must be an object, which must contain either the boolean logic operator key "-AND" or "-OR".
The value of a boolean logic key must be an array, where the items in the array must be either
    - a JSON Pointer string, or
    - an object, which contains single boolean logic operator key, which follows the same rules as above.

This is a recursive structure to allow for creating complex queries using boolean logic.

A JSON Pointer according to [RFC6901](https://tools.ietf.org/html/rfc6901) is a string which identifies a specific value within a JSON document.
While JSON Pointer allows for complex pointers, this specification only requires the use of basic pointers, which must reference a query component.

An example:

```javascript
{
  "-AND": [                             // The boolan operator "AND"
    "/query/components/gene/0",         // References the first gene component
    {
      "-OR": [                          // The boolean operator "OR"
        "/query/components/gene/1",     // References the second gene component
        "/query/components/gene/2"      // References the third gene component
      ]
    }
  ]
}
```

In the above example, the query logic is `"gene/0 AND (gene/1 OR gene/2)"`.

There is no defined limit to the complexity of the query that be constructed.
As such, implementers may want to create recursive functions, which should be able define and detect a recursion depth limit.
Should a client make a request which triggers a recursion limit defined by the server, the server must return with HTTP status code `422 Unprocessable Entity`, and may include a message body to indicate the type of error.


The user interface required to construct any query, including complex boolean logic, is outside the scope of this specification.

# Acknowledgments

Ideas/inspiration from:
 - [@Relequestual's](https://github.com/Relequestual) [GA4GH Search API Proposal - Components](https://gist.github.com/Relequestual/65c0446944519a66f8562d02b3cb4c86) 
 - [@Buske's](https://github.com/Buske) [mockup of MME v2](https://github.com/ga4gh/mme-apis/blob/version2-mock/version2/overview.md)

