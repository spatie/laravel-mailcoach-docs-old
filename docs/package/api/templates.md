---
title: Templates
---

## Get all templates

The `/mailcoach/api/templates` endpoint lists all your templates.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl https://example.app/mailcoach/api/templates \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json'
```

Searching is possible on this endpoint using `?filter[search]=searchterm` for searching.

Sorting is possible on this endpoint on `name` and `updated_at`. For example `?sort=-updated_at` to sort descending on `updated_at`

As a result, you get the details of all your templates:

```json
{
    "data": [
        {
            "id": 1,
            "name": "alias",
            "html": "<html><head>...</html>\n",
            "structured_html": null,
            "created_at": "2020-08-06T12:11:26.000000Z",
            "updated_at": "2020-08-06T12:11:26.000000Z"
        },
        {
            "id": 2,
            "name": "consectetur",
            "html": "<html><head>...</html>\n",
            "structured_html": null,
            "created_at": "2020-08-06T12:11:26.000000Z",
            "updated_at": "2020-08-06T12:11:26.000000Z"
        },
        {
            "id": 3,
            "name": "velit",
            "html": "<html><head>...</html>\n",
            "structured_html": null,
            "created_at": "2020-08-06T12:11:26.000000Z",
            "updated_at": "2020-08-06T12:11:26.000000Z"
        }
    ],
    "links": {
        "first": "https://example.app/mailcoach/api/templates?page=1",
        "last": "https://example.app/mailcoach/api/templates?page=1",
        "prev": null,
        "next": null
    },
    "meta": {
        "current_page": 1,
        "from": 1,
        "last_page": 1,
        "path": "https://example.app/mailcoach/api/templates",
        "per_page": 15,
        "to": 3,
        "total": 3
    }
}
```

## Get a specific template

If you don't want to retrieve all templates, you can get a specific template if you know its ID. The example below will get the details of template ID 99.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl https://example.app/mailcoach/api/templates/99 \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json'
```

Response:

```json
{
    "data": {
        "id": 99,
        "name": "alias",
        "html": "<html><head>...</html>\n",
        "structured_html": null,
        "created_at": "2020-08-06T12:11:26.000000Z",
        "updated_at": "2020-08-06T12:11:26.000000Z"
    }
}
```

## Add a template

To add a template, create a `POST` call to the `/mailcoach/api/templates/` endpoint.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl -X POST https://example.app/mailcoach/api/templates \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{"name":"Template name", "html":"<html>...</html>"}'
```

Your request payload should also be valid JSON. The actual payload, when formatted, looks like this:

```json
{
  "name": "Template name",
  "html": "<html>...</html>"
}
```

The only required field is the template's `name`. Other fields are `html` and `structured_html`

If the API call succeeded, you'll be given output like this.

```json
{
    "data": {
        "id": 1,
        "name": "Template name",
        "html": "<html>...</html>\n",
        "structured_html": null,
        "created_at": "2020-08-06T12:11:26.000000Z",
        "updated_at": "2020-08-06T12:11:26.000000Z"
    }
}
```

## Update a template

To update a template, create a `PUT` call to the `/mailcoach/api/templates/<id>` endpoint. In the example below we're updating the template with ID 99.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl -X PUT https://example.app/mailcoach/api/templates/99 \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{"name":"Updated name", "html":"<html>...</html>"}'
```

The only required field is the template's `name`. Other fields are `html` and `structured_html`

If the API call succeeded, you'll be given output like this.

```json
{
    "data": {
        "id": 99,
        "name": "Updated name",
        "html": "<html>...</html>\n",
        "structured_html": null,
        "created_at": "2020-08-06T12:11:26.000000Z",
        "updated_at": "2020-08-06T12:11:26.000000Z"
    }
}
```

## Delete a template

To delete a template, create a `DELETE` call to the `/mailcoach/api/templates/<id>` endpoint. In the example below we're deleting the template with ID 99.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl -X DELETE https://example.app/mailcoach/api/templates/99 \
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
