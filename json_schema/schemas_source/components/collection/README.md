# Collection Components

This contains the yaml for the collection components.

## count.yaml

```javascript
{
    "collectionComponents": {
        "count": 1
    }
}
```

The `count` object contains the number of records retrieved in this search. This is distinct from the number of records that may be contained in the `records` object.

## exists.yaml

```javascript
{
    "collectionComponents": {
        "exists": {
            "assertion": true
        }
    }
}
```

The `exists` object contain an `assertion` object  that is set to "true" if the `servers` contains any data that matches the query.

