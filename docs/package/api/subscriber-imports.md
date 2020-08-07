---
title: Subscriber imports
---

## Get all subscriber imports

The `/mailcoach/api/subscriber-imports` endpoint lists all subscriber imports.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl https://example.app/mailcoach/api/subscriber-imports \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json'
```

As a result, you get the details of all the subscriber imports:

```json
{
    "data": [
        {
            "id": 1,
            "uuid": "2ca0e737-b907-468f-819f-5d16e6bd6852",
            "subscribers_csv": null,
            "status": "completed",
            "email_list_id": 2,
            "subscribe_unsubscribed": false,
            "unsubscribe_others": false,
            "imported_subscribers_count": 577,
            "error_count": 836
        },
        {
            "id": 2,
            "uuid": "363fb337-7030-4bfd-8f8a-716493bb6c24",
            "subscribers_csv": null,
            "status": "completed",
            "email_list_id": 3,
            "subscribe_unsubscribed": false,
            "unsubscribe_others": false,
            "imported_subscribers_count": 178,
            "error_count": 565
        },
        ...
    ],
    "links": {
        "first": "https://example.app/mailcoach/api/subscriber-imports?page=1",
        "last": "https://example.app/mailcoach/api/subscriber-imports?page=1",
        "prev": null,
        "next": null
    },
    "meta": {
        "current_page": 1,
        "from": 1,
        "last_page": 1,
        "path": "https://example.app/mailcoach/api/subscriber-imports",
        "per_page": 15,
        "to": 3,
        "total": 3
    }
}
```

## Get the details of a specific subscriber import

If you want to get the details of a specific subscriber import, you can send a `GET` request to the `/mailcoach/api/subscriber-imports/<id>` endpoint.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl https://example.app/mailcoach/api/subscriber-imports/99 \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json'
```

Response:

```json
{
    "data": {
        "id": 99,
        "uuid": "363fb337-7030-4bfd-8f8a-716493bb6c24",
        "subscribers_csv": null,
        "status": "completed",
        "email_list_id": 3,
        "subscribe_unsubscribed": false,
        "unsubscribe_others": false,
        "imported_subscribers_count": 178,
        "error_count": 565
    }
}
```

## Creating a subscriber import

To create an import, send a `POST` request to the `/mailcoach/api/subscriber-imports` endpoint.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl -x POST https://example.app/mailcoach/api/subscriber-imports \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{"subscribers_csv":"email\njohn@doe.com", "email_list_id": 1, "subscribe_unsubscribed": true, "unsubscribe_others": false}'
```

All fields are required:

- `subscribers_csv`: a csv string with the subscribers' information
- `email_list_id`: a valid email list id
- `subscribe_unsubscribed`: true/false whether previously unsubscribed emails should be subscribed again
- `unsubscribe_others`: true/false whether subscribers not in the csv should be unsubscribed

If the API call succeeded, you'll be given a response with the import's details:

```json
{
  "data": {
      "id": 1,
      "uuid": "363fb337-7030-4bfd-8f8a-716493bb6c24",
      "subscribers_csv": "email\njohn@doe.com",
      "status": "draft",
      "email_list_id": 1,
      "subscribe_unsubscribed": true,
      "unsubscribe_others": false,
      "imported_subscribers_count": null,
      "error_count": 0
  }
}
```

## Updating a subscriber import

To update an import, send a `PUT` request to the `/mailcoach/api/subscriber-imports/<id>` endpoint.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl -x PUT https://example.app/mailcoach/api/subscriber-imports/99 \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{"subscribers_csv":"email\njohn@doe.com", "email_list_id": 1, "subscribe_unsubscribed": true, "unsubscribe_others": false}'
```

Fields are the same as in the create endpoint. This endpoint only works if the import has a `draft` status.

If the API call succeeded, you'll be given a response with the import's details:

```json
{
  "data": {
      "id": 1,
      "uuid": "363fb337-7030-4bfd-8f8a-716493bb6c24",
      "subscribers_csv": "email\njohn@doe.com",
      "status": "draft",
      "email_list_id": 1,
      "subscribe_unsubscribed": true,
      "unsubscribe_others": false,
      "imported_subscribers_count": null,
      "error_count": 0
  }
}
```

## Delete a subscriber import

To delete a subscriber import, you can send a `DELETE` request to the `/mailcoach/api/subscriber-imports/<id>` endpoint.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl -x DELETE https://example.app/mailcoach/api/subscriber-imports/99 \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json'
```

If the API call succeeded, you'll be given an empty response with a `204` status code.

## Appending to a subscriber import

You can append to the `subscribers_csv` field by sending a `POST` request to the `/mailcoach/api/subscriber-imports/<id>/append` endpoint.

This is useful for creating imports with a large amount of subscribers.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl -x POST https://example.app/mailcoach/api/subscriber-imports/99/append \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json'
    -d '{"subscribers_csv":"john2@doe.com"}'
```

`subscribers_csv` is the only, required, field.

If the API call succeeded, you'll be given a response with the import's details:

```json
{
  "data": {
      "id": 1,
      "uuid": "363fb337-7030-4bfd-8f8a-716493bb6c24",
      "subscribers_csv": "email\njohn@doe.com\njohn2@doe.com",
      "status": "draft",
      "email_list_id": 1,
      "subscribe_unsubscribed": true,
      "unsubscribe_others": false,
      "imported_subscribers_count": null,
      "error_count": 0
  }
}
```

## Starting the import

Once you're ready to start your import, you can send a `POST` request to the `/mailcoach/api/subscriber-imports/<id>/start` endpoint.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl -x POST https://example.app/mailcoach/api/subscriber-imports/99/start \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json'
```

This endpoint does not expect a body. The subscriber import should have a `draft` status.

If the API call succeeded, you'll be given an empty response with a `204` status code.

## Error handling

If an error occurred, you'll be given a non-HTTP/200 response code. The resulting payload might look like this.

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "name": [
      "The name field is required."
    ]
  }
}
```
