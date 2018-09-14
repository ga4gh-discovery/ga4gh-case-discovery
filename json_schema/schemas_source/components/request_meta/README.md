# Request Meta Components

This contains the yaml file for the request meta components.

## query_identification.yaml

```javascript
{
  "components": {
    "queryIdentification": {
      "queryID": "abc123",
      "queryLabel": "Search from client X for user on [date]"
    }
  }
}
```

The `queryIdentification` component contains a unique identifier for the query.

The `queryID` is the unique identifier, and is required.

The `queryLabel` can be added for readability.

