# Results Response

This document describes a `results` response by a `server` in response to a `search` request by a `client`.

# Format

The `results` response to a valid `search` request which the `server` is able to process, must include a `application/json` body with the following format.

## Basic Example - Simple results

```javascript
{
  "meta": {
    "response": {
      "apiVersion": "1.0.0",
      "components": {
        "collection": {
        "exists": "1.0.0",
        "count": "1.0.0"
        }
      }
    },
    "request": {
      "apiVersion": {
        "given": "1.0.0",
        "usedAs": "1.0.0"
      },
      "componentsUsed": [
        "gene"
      ]
    },
    "components": {
      "acknowledgments": {
        "terms": "There is no darkness, but ignorance."
      }
    }
  },
  "collectionComponents": {
    "exists": {
      "assertion": true
    },
    "count": 10
  },
  "records": []
}
```

This is a simple `results` response in the search API format.

Required objects in a `results` response are `meta` and `records` which are both specified in the basic example above, `collectionComponents` is optional and not specified.

### Meta object

The `meta` object allows the `server` to tell the `client` information about the `results` response it is giving, and how it determined these results. The `meta` object is required in the `results` response.

The `meta` object contains a `response`, `request`, and `components` objects.

#### Response object

The `response` object must include an `apiVersion` string, which specifies the API version used in the `results` response.

The `response` object provides version information about the components used in the `results` response.
Each key is one of the component types found in a `results` response (`record`, `recordMeta`, `collection`, or `responseMeta`).
The values of these keys are objects where the keys are component names and the values are the component data.

The `response` object is required.

#### Request object

The `request` object contains an `apiVersion` and `componentsUsed` property.

The `apiVersion` value is an object that allows the `server` to communicate to the `client` that it was provided with one specific API version, and used it as a different version.

The `componentsUsed` value is an array which contains the list of component types the `server` has used to find the `results`.

This includes components in the `query` and any `meta` components used, if any.

#### Components object

The `components` object (within the the `meta` object) must only contain response meta components.

The `acknowledgments` response meta component has only one required property which is `terms`. The remaining properties are defined in the JSON Schema file for this component.

### CollectionComponents object

The `collectionComponents` object contains summary or statistical information about the results.
Collection components may include information about results which are not returned as `records` in the `results` response.

In the above example, there are no individual `records` returned, however the collection components used communicate that records exist for the search, along with the number of records. This is similar to the Beacon response.

The `collectionComponents` object must only contain collection components.

### Records object

The `records` property is an array of objects, where each object represents an individual record, made up of record components and optional result meta components. 

In the above example, there are no records returned for the search.
Below is an example of a `records` array with some components.


```javascript
{
  ...
  "records:": [
    {
      "meta": {
        "links": [
          {
            "url": "https://database.com/patients/12345",
            "name": "Patient Record"
          }
        ],
        "contact": [
          {
            "fullName": "Foo Bar",
            "role": "Clinician",
            "email": "foo@institute.com",
            "instition": "Institute",
            "url": "https://institute.com/people/foobar"
          }
        ]
      },
      "gene": [
        {
          "ensemblID": "ENSG00000139618",
          "hgncName": "BRCA2"
        }
      ],
      "subjectVariant": [
        {
          "referenceName": "13",
          "start": 32936732,
          "end": 32936822,
          "assemblyID": "GRCh37"
        }
      ],
      "phenotype": [
        {
          "id": "HP:0000765",
          "observation": "yes",
          "label": "Abnormality of the thorax"
        }
      ]
    }
  ]
  ...
}
```

Three components are used in the above example, `gene`, `subjectVariant`, and `phenotype`.

The details in this document on specific components is not exhaustive. The JSON Schema YAML files should be referenced for the full component specification.

#### Gene object

The `gene` component uses `ensemblID` as the primary identifier, which must be an Ensembl Gene ID.

#### SubjectVariant object

The `subjectVariant` component contains a variant that has been seen in an individual subject.

Required properties are `assemblyID`, `referenceName`, and `start`.

#### Phenotype object

The `phenotype` component contains phenotypic assertions.

The only required property for a `phenotype` component is `id`, which is an HPO identifier string.

Omitting `observation` is the same as setting it to `"yes"`, so in this case it is redundant.

The `label` property is a string which contains a human readable name of the phenotype.

# Acknowledgments

Ideas/inspiration from:
 - [@Relequestual's](https://github.com/Relequestual) [GA4GH Search API Proposal - Components](https://gist.github.com/Relequestual/65c0446944519a66f8562d02b3cb4c86) 
 - [@Buske's](https://github.com/Buske) [mockup of MME v2](https://github.com/ga4gh/mme-apis/blob/version2-mock/version2/overview.md)
