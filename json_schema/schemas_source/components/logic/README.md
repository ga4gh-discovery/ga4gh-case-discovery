# Boolean Logic

This contains the yaml file for the boolean `logic` object.

## boolean_logic.yaml

Below is a simple example that "AND"s three components:

```javascript
{
  "logic": {
    "-AND": [
      "/query/components/gene/0",
      "/query/components/subjectVariant/0",
      "/query/components/phenotype/0"
    ]
  }
}
```

This is a more complex example that combines "AND" and "OR":

```javascript
{
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
