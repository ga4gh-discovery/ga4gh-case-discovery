# Results Response

This document describes a `results` response by a `server` in response to a `search` request by a `client`.

# Conventions and Terminology

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL
NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and
"OPTIONAL" in this document are to be interpreted as described in
[RFC 2119](https://tools.ietf.org/html/rfc2119).

# Format

The `results` response to a valid `search` request which the `server` is able to process, MUST include a `application/json` body with the following format.

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

A `results` response MUST include `meta` and `records` keys, and are defined in the basic example above.

The `results` response MAY include a `collectionComponents` object.

### Meta object

The `meta` object allows the `server` to tell the `client` information about the `results` response it is giving, and how it determined these results. The `meta` object is REQUIRED in the `results` response.

The `meta` object is REQUIRED, which MUST contain a `response` object.

The `meta` object MAY contain a `request` and `components` object.


#### Response object

The `response` object MUST include an `apiVersion` string, which specifies the API version used in the `results` response.

The `response` object provides version information about the components used in the `results` response.
Each key is one of the component types found in a `results` response (`record`, `recordMeta`, `collection`, or `responseMeta`).
The values of these keys are objects where the keys are component names and the values are the component data.

The `response` object is REQUIRED.

#### Request object

The `request` object SHOULD contain an `apiVersion` and `componentsUsed` property.

The `apiVersion` value is an object that allows the `server` to communicate to the `client` that it was provided with one specific API version, and used it as a different version.

The `componentsUsed` value is an array which contains the list of component types the `server` has used to find the `results`.

This SHOULD includes components in the `query` and any `meta` components used, if any.

#### Components object

The `components` object (within the the `meta` object) MUST only contain response meta components.

The `acknowledgments` response meta component has only one REQUIRED property which is `terms`. The remaining properties are defined in the JSON Schema file for this component.

### CollectionComponents object

The `collectionComponents` object contains summary or statistical information about the results.
Collection components may include information about results which are not returned as `records` in the `results` response.

In the above example, there are no individual `records` returned, however the collection components used communicate that records exist for the search, along with the number of records. This is similar to the Beacon response.

The `collectionComponents` object MUST only contain collection components.

### Records object

The `records` property MUST be an array of objects, where each object represents an individual record, made up of record components and optional result meta components. 

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

#### Gene object

The `gene` component uses `ensemblID` as the primary identifier, which must be an Ensembl Gene ID.

#### SubjectVariant object

The `subjectVariant` component contains a variant that has been seen in an individual subject.

Required properties are `assemblyID`, `referenceName`, and `start`.

#### Phenotype object

The `phenotype` component contains phenotypic assertions.

The only required property for a `phenotype` component is `id`, which is an HPO identifier string.

The `observation` defaults to `yes` if omitted.

The `label` property is a string which contains a human readable name of the phenotype.

# Other components

The details in this document on specific components is not exhaustive. The JSON Schema YAML files should be referenced for the full component specification, and are provided as a normative reference.

# Acknowledgments

Ideas/inspiration from:
 - [@Relequestual's](https://github.com/Relequestual) [GA4GH Search API Proposal - Components](https://gist.github.com/Relequestual/65c0446944519a66f8562d02b3cb4c86) 
 - [@Buske's](https://github.com/Buske) [mockup of MME v2](https://github.com/ga4gh/mme-apis/blob/version2-mock/version2/overview.md)
