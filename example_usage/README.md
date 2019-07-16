# GA4GH Discovery Search API Usage Examples

These examples were gathered from the Discovery WS driver project community and can be found [here.](https://docs.google.com/spreadsheets/d/1Vxfo7hssqsMtJeLUWTj5HRTAkE0q8bcl10V_N-SjwpE/edit#gid=0)

These are still in progress..more to come!

## Example 1: Query if data exists with a variant in the given gene
```
  Query:
    Do you have an entry with this gene?

  Answer format:
    yes | no
```

The `search` can be represented by,

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


With minimum required values, the `results` would be,

```javascript
{
    "meta": {
        "components": {
            "acknowledgments": {
                "terms": "Own more than thou showest, speak less than thou knowest."
            }
        }, 
        "request": {
            "apiVersion": {
                "given": "1.0.0", 
                "usedAs": "1.0.0"
            }
        }, 
        "response": {
            "apiVersion": "1.0.0", 
            "components": {
                "responseMeta": {
                    "acknowledgments": "1.0.0"
                }
            }
        }
    }, 
    "records": []
}
```


## Example 2: Query on variant and gene
```
  Query:
    Do you have an entry on BRCA2 with 17:42929130 C > A, and without HP:0000765		

  Answer format:
    yes | no
```

The `search` can be represented by,

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
      "queryIdentification": {
        "queryID": "abc123",
        "queryLabel": "Search from client X for user on [date]"
      }
    }
  },
  "requires": {
    "response": {
      "components": {
        "responseMeta": {
		  "acknowledgments": "1"
        },
        "collection": {
          "exists": "1",
          "count": "1"
        }
      }
    }
  },
  "query": {
    "components": {
      "gene": [
        {
          "ensemblID": "ENSG00000139618",
          "hgncName": "BRCA2"
        }
      ],
      "subjectVariant": [
        {
          "referenceName": "17",
          "start": 42929130,
          "referenceBases": "G",
          "alternateBases": "A",
          "assemblyID": "GRCh37"
        }
      ],
      "phenotype": [
        {
          "id": "HP:0000765",
          "observation": "no",
          "label": "Abnormality of the thorax"
        }
      ]
    }
  }
}
```


The `results` would be,

```javascript
{
    "meta": {
        "components": {
            "acknowledgments": {
                "terms": "All the world's a stage, and all the men and women merely players: they have their exits and their entrances; and one man in his time plays many parts."
            }
        }, 
        "request": {
            "apiVersion": {
                "given": "1.0.0", 
                "usedAs": "1.0.0"
            }, 
            "componentsUsed": [
                "subjectVariant"
            ]
        }, 
        "response": {
            "apiVersion": "1.0.0", 
            "components": {
                "collection": {
                    "count": "1", 
                    "exists": "1"
                }, 
                "responseMeta": {
                    "acknowledgments": "1"
                }
            }
        }
    }, 
    "collectionComponents": {
        "count": 1, 
        "exists": {
            "assertion": true
        }
    }, 
    "records": []
}
```


## Example 3: Query on variant and gene
```
  Query:
    Do you have an entry on BRCA2 with 17:42929130 C > A, and without HP:0000765		

  Answer format:
    records
```

The `search` can be represented by,

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
      "queryIdentification": {
        "queryID": "abc123",
        "queryLabel": "Search from client X for user on [date]"
      }
    }
  },
  "requires": {
    "response": {
      "components": {
        "responseMeta": {
		  "acknowledgments": "1"
        },
        "collection": {
          "exists": "1",
          "count": "1"
        },
        "record": {
          "gene": "1",
          "subjectVariant": "1",
          "phenotype": "1"
        },
        "recordMeta": {
          "links": "1",
          "contact": "1"
        }
      }
    }
  },
  "query": {
    "components": {
      "gene": [
        {
          "ensemblID": "ENSG00000139618",
          "hgncName": "BRCA2"
        }
      ],
      "subjectVariant": [
        {
          "referenceName": "17",
          "start": 42929130,
          "referenceBases": "G",
          "alternateBases": "A",
          "assemblyID": "GRCh37"
        }
      ],
      "phenotype": [
        {
          "id": "HP:0000765",
          "observation": "no",
          "label": "Abnormality of the thorax"
        }
      ]
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


The `results` would be,

```javascript
{
    "meta": {
        "components": {
            "acknowledgments": {
                "terms": "We are such stuff as dreams are made on, and our little life, is rounded with a sleep."
            }
        }, 
        "request": {
            "apiVersion": {
                "given": "1.0.0", 
                "usedAs": "1.0.0"
            }, 
            "componentsUsed": [
                "subjectVariant"
            ]
        }, 
        "response": {
            "apiVersion": "1.0.0", 
            "components": {
                "collection": {
                    "count": "1.0.0", 
                    "exists": "1.0.0"
                }, 
                "record": {
                    "phenotype": "1.0.0", 
                    "subjectVariant": "1.0.0"
                }, 
                "recordMeta": {
                    "contact": "1.0.0", 
                    "links": "1.0.0"
                }, 
                "responseMeta": {
                    "acknowledgments": "1.0.0"
                }
            }
        }
    }, 
    "collectionComponents": {
        "count": 1, 
        "exists": {
            "assertion": true
        }
    }, 
    "records": [
        {
            "components": {
                "phenotype": [
                    {
                        "id": "HP:0000453", 
                        "label": "Choanal atresia", 
                        "observation": "yes"
                    }, 
                    {
                        "id": "HP:0000405", 
                        "label": "Conductive hearing impairment", 
                        "observation": "yes"
                    }, 
                    {
                        "id": "HP:0011451", 
                        "label": "Congenital microcephaly", 
                        "observation": "yes"
                    }, 
                    {
                        "id": "HP:0011451", 
                        "label": "Congenital microcephaly", 
                        "observation": "yes"
                    }, 
                    {
                        "id": "HP:0010880", 
                        "label": "Increased nuchal translucency", 
                        "observation": "yes"
                    }, 
                    {
                        "id": "HP:0000272", 
                        "label": "Malar flattening", 
                        "observation": "yes"
                    }, 
                    {
                        "id": "HP:0000272", 
                        "label": "Malar flattening", 
                        "observation": "yes"
                    }, 
                    {
                        "id": "HP:0000347", 
                        "label": "Micrognathia", 
                        "observation": "yes"
                    }, 
                    {
                        "id": "HP:0000347", 
                        "label": "Micrognathia", 
                        "observation": "yes"
                    }, 
                    {
                        "id": "HP:0008551", 
                        "label": "Microtia", 
                        "observation": "yes"
                    }, 
                    {
                        "id": "HP:0011342", 
                        "label": "Mild global developmental delay", 
                        "observation": "yes"
                    }, 
                    {
                        "id": "HP:0000545", 
                        "label": "Myopia", 
                        "observation": "yes"
                    }, 
                    {
                        "id": "HP:0000384", 
                        "label": "Preauricular skin tag", 
                        "observation": "yes"
                    }, 
                    {
                        "id": "HP:0001622", 
                        "label": "Premature birth", 
                        "observation": "unknown"
                    }, 
                    {
                        "id": "HP:0009623", 
                        "label": "Proximal placement of thumb", 
                        "observation": "yes"
                    }
                ], 
                "subjectVariant": [
                    {
                        "alternateBases": "A", 
                        "assemblyID": "GRCh37", 
                        "referenceBases": "G", 
                        "referenceName": "17", 
                        "start": 42929130
                    }
                ]
            }, 
            "meta": {
                "contact": [
                    {
                        "email": "submitter@institution.edu", 
                        "fullName": "A submitter", 
                        "institution": "An institution", 
                        "role": "Submitter"
                    }
                ], 
                "links": [
                    {
                        "name": "Submission: 1, Identifier: 'HG000042'", 
                        "url": "http://phenodbsearch.local:8080/submission/1"
                    }
                ]
            }
        }
    ]
}
```

## Example 4: Query on variants
```
  Query:
    Do you have an entry with ( 5:118860953 T > C OR 17:42929130 G > A ) AND 5:118792051 C > T

  Answer format:
    yes | no
```

The `search` can be represented by,

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
      "queryIdentification": {
        "queryID": "abc123",
        "queryLabel": "Search from client X for user on [date]"
      }
    }
  },
  "requires": {
    "response": {
      "components": {
        "responseMeta": {
          "acknowledgments": "1"
        },
        "collection": {
          "exists": "1",
          "count": "1"
        }
      }
    }
  },
  "query": {
    "components": {
      "subjectVariant": [
        {
          "referenceName": "5",
          "start": 118860953,
          "referenceBases": "T",
          "alternateBases": "C",
          "assemblyID": "GRCh37"
        },
        {
          "referenceName": "17",
          "start": 42929130,
          "referenceBases": "G",
          "alternateBases": "A",
          "assemblyID": "GRCh37"
        },
        {
          "referenceName": "5",
          "start": 118792051,
          "referenceBases": "C",
          "alternateBases": "T",
          "assemblyID": "GRCh37"
        }
      ]
    }
  },
  "logic": {
    "-AND": [
      "/query/components/subjectVariant/0",
      {
        "-OR": [
          "/query/components/subjectVariant/1",
          "/query/components/subjectVariant/2"
        ]
      }
    ]
  }
}
```


The `results` would be,

```javascript
{
    "meta": {
        "components": {
            "acknowledgments": {
                "terms": "There is no darkness, but ignorance."
            }
        }, 
        "request": {
            "apiVersion": {
                "given": "1.0.0", 
                "usedAs": "1.0.0"
            }, 
            "componentsUsed": [
                "subjectVariant"
            ]
        }, 
        "response": {
            "apiVersion": "1.0.0", 
            "components": {
                "collection": {
                    "count": "1.0.0", 
                    "exists": "1.0.0"
                }, 
                "responseMeta": {
                    "acknowledgments": "1.0.0"
                }
            }
        }
    }, 
    "collectionComponents": {
        "count": 1, 
        "exists": {
            "assertion": true
        }
    }, 
    "records": []
}
```







