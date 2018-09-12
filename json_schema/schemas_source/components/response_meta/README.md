# Response Meta Components

This contains the yaml for the response meta components.

## acknowledgments.yaml

```javascript
{
  "components": {
    "acknowledgments": {
      "terms": "Honesty is the best policy. If I lose mine honor, I lose myself.",
      "disclaimer": "Though this be madness, yet there is method in it.",
      "reference": "It was Greek to me.",
      "id": "42"
    }
  }, 
}
```

The `acknowledgments` component contains terms and conditions of viewing and or using the data.

The `terms` is the terms and conditions.

The `disclaimer` is the disclaimer associated with the data.

The `disclaimer` is the disclaimer associated with the data.

The `reference` is the reference associated with the data.

The `id` is the ID given to this set of acknowledgments.

Multiple `acknowledgments` are represented as an array:

```javascript
{
  "components": {
    "acknowledgments": [
      {
        "terms": "Honesty is the best policy. If I lose mine honor, I lose myself.",
        "disclaimer": "Though this be madness, yet there is method in it.",
        "reference": "It was Greek to me.",
        "id": "1"
      },
      {
        "terms": "Time is the justice that examines all offenders.",
        "disclaimer": "By medicine life may be prolonged, yet death will seize the doctor too.",
        "reference": "We have seen better days.",
        "id": "2"
      },
    ],
  }, 
}
```


