# Record Meta Components

This contains the yaml for the record meta components.

## contact.yaml

```javascript
{
  "contact": [
    {
      "id": "12345",
      "role": "Clinician",
      "fullName": "John Doe",
      "email": "foo@bar.com",
      "institution": "Awesome Institute",
      "url": "http://awesome_institute.com/people/john_doe"
    }
  ]
}
```

The `id` component contains a unique identifier for the person in the database.

The `role` is label for the person's role.

The `fullName` is the full name for the person, and is required.

The `email` is the email for the person.

The `institution` is the institution for the person.

The `url` is the url for the person.

## links.yaml

```javascript
{
  "links": [
    {
      "url": "http://institute.com/record_database/record/123",
      "name": "Subject record"
    }
  ]
}
```

The `links` component contain links to resources that make up the record.

The `url` is a valid URL, and it required.

The `name` is a human readable name for the URL, and it required.

