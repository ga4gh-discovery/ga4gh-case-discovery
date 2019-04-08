# GA4GH Search API Proposal - Components

## Executive Summary

The Matchmaker Exchange API is a successful live operating system which has resulted in real world life altering discoveries. Work on the Matchmaker Exchange API has shown that, while meeting most of the limited MME use cases, a more generalised discovery based API needs to be flexible, extensible, and planned for long term use.

A new discovery search based API should be expected to be in use for a minimum two decades once operational.

Keeping the barrier to entry low while not compromising on utility is essential. Each implementation of the API should be able to update to newer versions of the API at their own pace, without compromising interoperability. Each data repository exposing its data must be able to use as much or as little data for searching and responding to requests, is key.

By using a component based system, the GA4GH Search API can be extended as new use cases present themselves without causing issues. Example component types may include, `gene`, `variant`, `phenotype`, `contact`, and `annotation`.

Specifying components structure and expectations separately from a single API definition, allows components to be developed to meet new requirements as they arise. This method of defining and expanding the API format, avoids a number of issues in adoption of the specification, and it's further development, based on the experienced gained developing the MME API and other specification centric projects.

Providing a great amount of flexibility is not without the risk of creating a disparate non-interoperable network! However given our experience from MME and other similar ventures, and the speed of development, a component based architecture and will significantly reduce this risk, while not preventing the APIs evolution.

## Overview
This is not a fully fleshed out API, and is merely a proposal as a foundation for iteration and further work.

### Purpose of the Search API

While the Matchmaker Exchange API and the Beacon API provide a way to access data at some level from siloed databases, MME is limited by it's use case of required reciprocal sharing and candidate genes, and the clinical utility of Beacon is not fully realised without some method to access associated patient data.

A new Search API from the Discovery Workstream of the Global Alliance for Genomic Health, must continue to enable the expansion of data sharing while also considering the practical utility to end users.

### Requirements

*A standard workflow, method, and format, which can facilitate genomic data discoverability and sharing on a federated global network scale.*

Because of the heterogeneity of the data involved, from patient phenotypes to variants, the API needs to be dynamic in nature, being able to cater for various types of data in any combination. The challenge this presents has been felt while developing the MME API, and the initial draft for a second version of MME is the foundation for this proposal.

A few key requirements, but not a complete set:

* Discoverability of patient data
* Discoverability of variant data
* Many possible search constraints
* Many possible response data types
* Flexibility on data type support
* Low barrier to entry
* Low cost to maintain

## Components

In order to facilitate broad flexibility of use while keeping the barrier to entry low, the suggestion is to use a series of modular components, which can be used in any combination based on requirements or support, to harmonise data models in the request and response.

All components, at whatever level, are optional, however returning no components at any level is useless. The more components a service is able to cater for and respond with, the greater the power provided to the querying service, and their users reviewing results.

Thinking of the basic beacon request and response, the request query includes a genomic location, and the response is an assertion of existence.


### Request and Response

A simple Search API request representing a Beacon request could look like the following.

```json
// Request:

{
  "query": {
    "components": {
      "variant": [{
        "referenceName": 13,
        "start": 32936732,
        "referenceBases": "G",
        "alternateBases": "C",
        "assemblyId": "GRCh37"
      }]
    }
  }
  ...
}

// Response:

{
  "collectionComponents": {
    "exists": {
      "assertion": true
    }
  }
  ...
}
```

The request defines it is searching for a specific variant, and the response asserts a positive assertion on existence of that variant.

The components used are `variant` and `exists`, where `variant` is a record level component and `exists` is an collection level component. These two levels of components are required when you want to express different aspects of the response.

The response in this instance only includes a collection level component, `exists`, with an assertion of `true`, which denotes the existence of that variant within the queried system.

A more advanced request and response may look like the following. I'll explain the idea behind each section in reference to the API requirements.

```json
// Request:

{
  "query": {
    "components": {
      "variant": [{
        "referenceName": 13,
        "start": 32936732,
        "referenceBases": "G",
        "alternateBases": "C",
        "assemblyId": "GRCh37"
      }]
    }
  },
  "requires": {
    "response": {
      "components": {
          "exists": "^1"
      }
    }
  }
  ...
}

// Response:

{
  "meta": {
    "response": {
      "collectionComponents": {
        "exists": "1.0.0",
        "count": "1.0.0"
      },
      "components": {
        "variant": "1.0.0",
        "phenotype": "1.0.0",
        "contact": "1.0.0",
        "link": "1.0.0"
      }
    },
    "request": {
      "componentsUsed": [ "variant" ]
    },
    "provenance": {
      ...
    }
  },
  "collectionComponents": {
    "exists": {
      "assertion": true
    },
    "count": 23
  },
  "records": [{
    "meta": {
      "provenance" : {
        ...
      }
    },
    "components": {
      "variant": [{
        ...
      }],
      "phenotype": [{
        ...
      }],
      "contact": [{
        ...
      }],
      "link": [{
        ...
      }]
    }
  }]
  ...
}
```
### Request Detail


The query object contains a `components` object, where each value of the `components` object is a collection of components of the type defined by the  key. In this case, one variant component is given.

Any number of components could be designed and specified for use in the set of components. This makes it possible to discover a variety of data types, including, but not limited to, patient and variant level data.

Using this component structure, new components can be added as new use cases are discovered. This allows for many possible search constraints and response data types.

```json
// Request:

{
  "query": {
    "components": {
      "variant": [{
        "referenceName": 13,
        "start": 32936732,
        "referenceBases": "G",
        "alternateBases": "C",
        "assemblyId": "GRCh37"
      }]
    }
  }
  ...
}
```

The structure for the query payload remains mostly the same, however the requestor has specified they wish to only get responses from the service if they support responding with the `exists` component at version 1 or above. The syntax used to specify version is that defined by semantic versioning, and is well defined for package management tools like npm.

```json
//Request
{
  "query": {
    ...
  },
  "requires": {
    "response": {
      "components": {
          "exists": "^1"
      }
    }
  }
  ...
}
```


### Response Detail

The response should indicate which components and their versions it has included. This allows for flexible data type support, as the responding system can include as many data types as they like beyond those required.

```json
// Response:

{
  "meta": {
    "response": {
      "collectionComponents": {
        "exists": "1.0.0",
        "count": "1.0.0"
      },
      "components": {
        "variant": "1.0.0",
        "phenotype": "1.0.0",
        "contact": "1.0.0",
        "link": "1.0.0"
      }
    },
```

While it may seem overkill to define a version for a component which only defines existence, it's not impossible that how this is defined might want to change. A new requirement could result in a new version of a component, or a new but similar component.

(It's possible we could define a means for a requestor to specify they want responses to ONLY include specific components).

The response could indicate which components have been used when searching. It may be expected that not all components will be used (for example, if an OR condition is used).

```json
{
  "meta": {
    ...
    "request": {
      "componentsUsed": [ "variant" ]
    },
    ...
  },
```

Metadata may contain any sort of information about the actual data. This might include a URI which has further service description information encoded in JSON, but that's out of the scope or this proposal.

If initially a number of categories of provenance data can be agreed on, these can be formalised, however if not, we could simply allow an open text field to see what information databases are willing to share. Provenance data may not need to be computable till it's better defined.

```json
{
  "meta": {
    ...
    "provenance": {
      ...
    }
  }
```

In the more fully featured response, a `count` collection component has been added. This represents the number of records returned. This wasn't requested or required by the request body, and may not seem that useful, however may become useful if the response doesn't include all the search results in the response, and provides some mechanism for seeing the rest (like pagination or an external link of some kind). We might need to define a means to specify the results have been truncated.

```json
  "collectionComponents": {
    "exists": {
      "assertion": true
    },
    "count": 23
  },
```

One of the objects in the response `records` collection includes a number of components, which were specified in the top level `meta` object.


```json
"records": [{
    ...
    "components": {
      "variant": [{
        ...
      }],
      "phenotype": [{
        ...
      }],
      "contact": [{
        ...
      }],
      "link": [{
        ...
      }]
    }
  }]
```

The records collection object may also include a meta key. This is required to allow overriding of provenance data (or any other meta data) for nodes that re-issue the request in a federated fashion to other private child nodes. This is possibly not required to begin with, but justifies positioning record data components within a `components` key for the records collection object.

```json
  "records": [{
    "meta": {
      "provenance" : {
        ...
      }
    },
    ...
```

# Query Operators

By default, it should be assumed that all components should be additive to a search request, resulting in tighter a constrained search.

Borrowing an existing well used concept of operators in structure from ORMs, it's possible to express `AND` and `OR` operators for components.


```json
...
  "components": {
    "-or": [
      {
        "-and": {
          "gene": [
            {
              "hgnc_name": "A"
            },
            {
              "hgnc_name": "B"
            }
          ]
        }
        "gene": [
          {
            "hgnc_name": "C"
          }
        ]
      }
    ]
  }
...
```
The above request JSON represents the query structure `(A AND B) OR C`.

By allowing these operations to be part of the component system, the API allows for dynamic support of operators, keeping the barrier to entry low.

This structure allows nesting of any level, without the need to parse strings and re-order parts of query structure to compose their internal query structure. (Remember, this API structure is intended to be parsed by code; the interface presented to the user to construct a query is determined by each system.)

It's expected that such operators are not initially supported. When nodes or clients are ready to support operator components, queries should specify their support is required in the request, otherwise they may be ignored, and results might not be very useful.

Similar to the request meta object previously shown...

```json
//Request
{
  "query": {
    ...
  },
  "requires": {
    "request": {
      "components": {
          "-and": "^1",
          "-or": "^1"
      }
    }
  }
  ...
}
```
(Although a version is supplied, as all components must be versioned, they will probably not be incremented beyond 1.0 for operators)


# Summary

Any number of components at any level can be defined and supported to meet use cases as they are discovered. By providing support for components as each systems is able to, and requesting the use or return of components as required or is useful, the API framework proposed has a low barrier to entry and low cost to maintain, while providing a flexible and robust platform to exchange critical data.

The componentisation of data allows for focused development or requirements for groups of interested parties to be discussed and improved on, with fewer cross cutting concerns, and significantly reduced issues of breaking backwards compatibility of the API as a whole.


# Considerations

## Use Existing Standards

We should use existing standard where possible to avoid reinventing the wheel. 

For example using HTTP status codes to indicate possible response states, rather than creating a new set of response codes and embedding them in JSON responses.

If there's an expectation that a search request could be applicable to VCF files, it's reasonable to assume the response may not be attainable within a HTTP request timeframe. As such, we may propose a method for returning results later by the requestor providing a callback URL or some such, OR, respond with a URL and time the URL may be ready to provide the required response. An appropriate HTTP response from such a request could be HTTP status 202, accepted. (There are multiple possible ways this could be handled.)

We should define the API structure and the components individually. This is easy to do with JSON Schema (and by extension OAI for full API specification). 

## Using New Standards

We should strive to use standard created by the GA4GH where possible and appropriate to avoid duplicating work, specifically, model definitions. Our main expectation for this is to use parts of the Phenopackets specification (when stable), defining segments as individual versioned components.

We should not however hold back from using initial versions of components due to lack of a GA4GH standard, as this would be likely to slow the development of the Search API and any associated network.

## API Specification Document

The API specification document, should as a requirement include the JSON Schema for each component, and the associated Request and Response.

Using JSON Schema removes a level of possible ambiguity that may otherwise cause implementation issues, which have been observed during the development and implementation of the MME API network.

It's fairly trivial to split up the components into their own JSON Schema documents, and reference them, while conforming to JSON Schema.

A full API specification may be built using the Open API Initiative specification, which is widely used as the defacto API definition language for new JSON based web APIs (which uses JSON Schema for payload format).

## Service Definition and Indexes

Given the API has aspects of varying levels of support in terms of different components, it would be extremely useful if services provided information about the support they are able to offer. This would detail the components they use for searching, and those they may return, with the potential to detail more about the service as is useful to do so. This must be machine readable.

It's expected that it will be possible to construct an index of services with their component support. From discussions during GA4GH meetings, I believe this is the intent of the "Discovery Networks API" group, to index services, including those that support GA4GH Search. I believe this may take on the form of a distributed network of indexes. This falls outside the remit of this document, however defining the Search API Service definition JSON may fall within the scope of the Search API development.


## Aggregator / Federated Response Nodes


It is expected that a response would use components where all components of the same type would be the same version. However with aggregator or federated nodes which forward queries on to multiple child nodes, it may legitimately return an array of results which contain components of different versions.

This document is canonicalised at https://gist.github.com/Relequestual/65c0446944519a66f8562d02b3cb4c86
