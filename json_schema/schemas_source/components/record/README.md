# Record Components

This contains the yaml files for the record components.

## gene.yaml

```javascript
{
  "gene": [
    {
      "ensemblID": "ENSG00000139618",
      "hgncName": "BRCA2"
    }
  ]
}
```

The `gene` component contains a gene identifier.

The `ensemblID` is an Ensembl Gene ID, and is required.

The `hgncName` can be added for readability.

## phenotype.yaml

```javascript
{
  "phenotype": [
    {
      "id": "HP:0000765",
      "observation": "yes",
      "label": "Abnormality of the thorax"
    }
  ]
}
```

The `phenotype` component contain phenotypic assertions. 

The `id` is an HPO identifier, and is required (see [HPO](https://hpo.jax.org)).

The `observation` can be set to "yes", "no" and "unknown", it defaults to "yes" if not specified.

The `label` is the phenotype definition and can be added for readability. 

## subjectVariant.yaml

```javascript
{
  "subjectVariant": [
    {
      "referenceName": "17",
      "start": 42929130,
      "referenceBases": "G",
      "alternateBases": "A",
      "assemblyID": "GRCh37"
    }
  ],
}
```

The `subjectVariant` component contains a subject variant.

The `referenceName` is the chromosome, 1-22, "X", "Y", "MT", and is required.

The `start` is the genomic coordinate, and is required.

The `referenceBases` and `alternateBases` are the alleles.

The `assemblyID` is the NCBI assembly, "NCBI34", "NCBI35", "NCBI36", "GRCh37", "GRCh38", and is required.


