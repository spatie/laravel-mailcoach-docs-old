---
title: Subscribers
---

## Get all subscribers from an email list

The `/mailcoach/api/email-lists/<id>/subscribers` endpoint lists all subscribers of a specific email list.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl https://example.app/mailcoach/api/email-lists/99/subscribers \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json'
```

Searching on `email`, `first_name`, `last_name` and `tags` is possible on this endpoint using `?filter[search]=searchterm` for searching.

Filtering on subscriber status is possible using `?filter[status]=unconfirmed`, possible values are `unconfirmed`, `subscribed` and `unsubscribed`

Sorting is possible on this endpoint on `created_at`, `unsubscribed_at`, `email`, `first_name` and `last_name`. For example `?sort=-created_at` to sort descending on `created_at`

As a result, you get the details of all the email list's subscribers:

```json
{
    "data": [
        {
            "id": 3,
            "email_list_id": 1,
            "email": "john@doe.com",
            "first_name": null,
            "last_name": null,
            "extra_attributes": [],
            "tags": [],
            "uuid": "2a0c72d9-f91a-4fd2-88a7-589dfd75f464",
            "subscribed_at": "2020-08-06T13:24:31.000000Z",
            "unsubscribed_at": null,
            "created_at": "2020-08-06T13:24:31.000000Z",
            "updated_at": "2020-08-06T13:24:31.000000Z"
        },
        ...
    ],
    "links": {
        "first": "https://example.app/mailcoach/api/email-lists/1/subscribers?page=1",
        "last": "https://example.app/mailcoach/api/email-lists/1/subscribers?page=1",
        "prev": null,
        "next": null
    },
    "meta": {
        "current_page": 1,
        "from": 1,
        "last_page": 1,
        "path": "https://example.app/mailcoach/api/email-lists/1/subscribers",
        "per_page": 15,
        "to": 3,
        "total": 3
    }
}
```

## Get a specific subscriber

If you want to get the details of a specific subscriber, you can send a `GET` request to the `/mailcoach/api/subscribers/<id>` endpoint.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl https://example.app/mailcoach/api/subscribers/99 \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json'
```

Response:

```json
{
    "data": {
        "id": 99,
        "email_list_id": 1,
        "email": "john@doe.com",
        "first_name": null,
        "last_name": null,
        "extra_attributes": [],
        "tags": [],
        "uuid": "2a0c72d9-f91a-4fd2-88a7-589dfd75f464",
        "subscribed_at": "2020-08-06T13:24:31.000000Z",
        "unsubscribed_at": null,
        "created_at": "2020-08-06T13:24:31.000000Z",
        "updated_at": "2020-08-06T13:24:31.000000Z"
    }
}
```

## Subscribing to an email list

To subscribe an email address to an email list, send a `POST` request to the `/mailcoach/api/email-lists/<id>/subscribers` endpoint.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl -x POST https://example.app/mailcoach/api/email-lists/99/subscribers \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{"email":"john@doe.com", "first_name":"John", "last_name":"Doe"}'
```

The only required field is `email` and should be a valid email address that is not already subscribed (or unsubscribed) in the list.

If the API call succeeded, you'll be given a response with the subscriber's details:

You can pass the following fields while creating a subscriber:

- `email`: string, required
- `first_name`: nullable
- `last_name`: nullable
- `extra_attributes`: nullable, array
- `tags`: nullable, array

When passing tags, the tags will be synced to the subscriber.

```json
{
  "data": {
    "id": 3,
    "email_list_id": 1,
    "email": "john@doe.com",
    "first_name": null,
    "last_name": null,
    "extra_attributes": [],
    "tags": [],
    "uuid": "2a0c72d9-f91a-4fd2-88a7-589dfd75f464",
    "subscribed_at": "2020-08-06T13:24:31.000000Z",
    "unsubscribed_at": null,
    "created_at": "2020-08-06T13:24:31.000000Z",
    "updated_at": "2020-08-06T13:24:31.000000Z"
  }
}
```

## Delete a subscriber

To delete a subscriber, and erase all knowledge of it existing, you can send a `DELETE` request to the `/mailcoach/api/subscribers/<id>` endpoint.

> This means the email address can be subscribed again on a later date.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl -x DELETE https://example.app/mailcoach/api/subscribers/99 \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json'
```

If the API call succeeded, you'll be given an empty response with a `204` status code.

## Confirm a subscriber

To confirm a subscriber's subscription to an email list, you can send a `POST` request to the `/mailcoach/api/subscribers/<id>/confirm` endpoint

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl -x POST https://example.app/mailcoach/api/subscribers/99/confirm \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json'
```

If the API call succeeded, you'll be given an empty response with a `204` status code.

## Unsubscribe a subscriber

To unsubscribe a subscriber's subscription to an email list, you can send a `POST` request to the `/mailcoach/api/subscribers/<id>/unsubscribe` endpoint

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl -x POST https://example.app/mailcoach/api/subscribers/99/unsubscribe \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json'
```

If the API call succeeded, you'll be given an empty response with a `204` status code.

## Resend confirmation of a subscriber

To resend the confirmation email a subscriber received, you can send a `POST` request to the `/mailcoach/api/subscribers/<id>/resend-confirmation` endpoint

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl -x POST https://example.app/mailcoach/api/subscribers/99/resend-confirmation \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json'
```

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
