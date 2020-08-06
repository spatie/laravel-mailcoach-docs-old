---
title: Campaigns
---

## Get all campaigns

The `/mailcoach/api/campaigns` endpoint lists all your campaigns.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl https://example.app/mailcoach/api/campaigns \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json'
```

Searching is possible on this endpoint using `?filter[search]=searchterm` for searching.

Filtering is possible on the status of the campaigns using `?filter[status]=sent`, possible values are `sent`, `scheduled` and `draft`

Sorting is possible on this endpoint. For example `?sort=-name` to sort descending on `name`.
Allowed sorts:
- `name`
- `unique_open_count`
- `unique_click_count`
- `unsubscribe_rate`
- `sent_to_number_of_subscribers`
- `sent`

As a result, you get the details of all your campaigns:

```json
{
    "data": [
        {
            "id": 1,
            "name": null,
            "uuid": "11ce123b-57c4-429b-9840-c79f8047d8aa",
            "email_list_id": 1,
            "from_email": "joh@doe.com",
            "from_name": "John Doe",
            "status": "draft",
            "html": "<html>...</html>\n",
            "structured_html": null,
            "email_html": null,
            "webview_html": null,
            "mailable_class": null,
            "track_opens": false,
            "track_clicks": true,
            "sent_to_number_of_subscribers": "0",
            "segment_class": null,
            "segment_description": "0",
            "open_count": "0",
            "unique_open_count": "0",
            "open_rate": 0,
            "click_count": "0",
            "unique_click_count": "0",
            "click_rate": 0,
            "unsubscribe_count": "0",
            "unsubscribe_rate": "0",
            "bounce_count": "0",
            "bounce_rate": "0",
            "sent_at": null,
            "statistics_calculated_at": null,
            "scheduled_at": null,
            "last_modified_at": "2020-08-06T12:48:41.000000Z",
            "summary_mail_sent_at": null,
            "created_at": "2020-08-06T12:48:41.000000Z",
            "updated_at": "2020-08-06T12:48:41.000000Z"
        },
        ...
    ],
    "links": {
        "first": "https://example.app/mailcoach/api/campaigns?page=1",
        "last": "https://example.app/mailcoach/api/campaigns?page=1",
        "prev": null,
        "next": null
    },
    "meta": {
        "current_page": 1,
        "from": 1,
        "last_page": 1,
        "path": "https://example.app/mailcoach/api/campaigns",
        "per_page": 15,
        "to": 3,
        "total": 3
    }
}
```

## Get a specific campaign

If you don't want to retrieve all campaigns, you can get a specific campaign if you know its ID. The example below will get the details of campaign ID 99.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl https://example.app/mailcoach/api/campaigns/99 \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json'
```

Response:

```json
{
    "data": {
        "id": 99,
        "name": null,
        "uuid": "11ce123b-57c4-429b-9840-c79f8047d8aa",
        "email_list_id": 1,
        "from_email": "joh@doe.com",
        "from_name": "John Doe",
        "status": "draft",
        "html": "<html>...</html>\n",
        "structured_html": null,
        "email_html": null,
        "webview_html": null,
        "mailable_class": null,
        "track_opens": false,
        "track_clicks": true,
        "sent_to_number_of_subscribers": "0",
        "segment_class": null,
        "segment_description": "0",
        "open_count": "0",
        "unique_open_count": "0",
        "open_rate": 0,
        "click_count": "0",
        "unique_click_count": "0",
        "click_rate": 0,
        "unsubscribe_count": "0",
        "unsubscribe_rate": "0",
        "bounce_count": "0",
        "bounce_rate": "0",
        "sent_at": null,
        "statistics_calculated_at": null,
        "scheduled_at": null,
        "last_modified_at": "2020-08-06T12:48:41.000000Z",
        "summary_mail_sent_at": null,
        "created_at": "2020-08-06T12:48:41.000000Z",
        "updated_at": "2020-08-06T12:48:41.000000Z"
    }
}
```

## Add a campaign

To add a campaign, create a `POST` call to the `/mailcoach/api/campaigns/` endpoint.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl -X POST https://example.app/mailcoach/api/campaigns \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{"name":"Campaign name", "email_list_id": 1, "html": "<html>...</html>", "track_opens": true, "track_clicks": false}'
```

Your request payload should also be valid JSON. The actual payload, when formatted, looks like this:

```json
{
  "name": "Campaign name",
  "email_list_id": 1,
  "html": "html",
  "track_opens": true,
  "track_clicks": false
}
```

The only required fields are the campaign's `name` and `list_id`.

Fields:
- `name` => required | string
- `email_list_id` => required | integer
- `segment_id` => integer
- `html` => string
- `mailable_class` => string
- `track_opens` => boolean
- `track_clicks` => boolean
- `schedule_at` => date_format:Y-m-d H:i:s

If the API call succeeded, you'll be given output like this.

```json
{
    "data": {
        "id": 1,
        "name": "Campaign name",
        "html": "<html>...</html>\n",
        ...
    }
}
```

## Update a campaign

To update a campaign, create a `PUT` call to the `/mailcoach/api/campaigns/<id>` endpoint. In the example below we're updating the campaign with ID 99.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl -X PUT https://example.app/mailcoach/api/campaigns/99 \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{"name":"Updated name", "html":"<html>...</html>"}'
```

The only required fields are the campaign's `name` and `list_id`. Other fields are the same when creating a campaign.

If the API call succeeded, you'll be given output like this.

```json
{
    "data": {
        "id": 99,
        "name": "Updated name",
        "html": "<html>...</html>\n",
        ...
    }
}
```

## Delete a campaign

To delete a campaign, create a `DELETE` call to the `/mailcoach/api/campaigns/<id>` endpoint. In the example below we're deleting the campaign with ID 99.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl -X DELETE https://example.app/mailcoach/api/campaigns/99 \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json'
```

If the API call succeeded, you'll be given an empty response with a `204` status code.

## Sending a test

To send a campaign test, create a `POST` call to the `/mailcoach/api/campaigns/<id>/send-test` endpoint.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl -X POST https://example.app/mailcoach/api/campaigns/99/send-test \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{"email":"john@doe.com"}'
```

The only field is a required `email` field, which can be a `,` delimited field with a maximum of `10` email addresses.

If the API call succeeded, you'll be given an empty response with a `204` status code.

## Sending a campaign

To send a campaign, create a `POST` call to the `/mailcoach/api/campaigns/<id>/send` endpoint.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl -X POST https://example.app/mailcoach/api/campaigns/99/send \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json'
```

This endpoint doesn't require a body.

If the API call succeeded, you'll be given an empty response with a `204` status code.

## Getting a sent campaign's opens

To get the opens of a campaign, create a `GET` call to the `/mailcoach/api/campaigns/<id>/opens` endpoint.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl https://example.app/mailcoach/api/campaigns/99/opens \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json'
```

You can sort on `email`, `open_count` and `first_opened_at`.
Searching by email can be done by using `?filter[search]=john@doe.com`

If the API call succeeded, you'll get output like this

```json
{
    "data": [
        {
            "subscriber_id": 2,
            "subscriber_email": "john@doe.com",
            "open_count": 1,
            "first_opened_at": "2020-08-06T13:02:46.000000Z"
        }
    ],
    "links": {
        "first": "https://example.app/mailcoach/api/campaigns/2/opens?page=1",
        "last": "https://example.app/mailcoach/api/campaigns/2/opens?page=1",
        "prev": null,
        "next": null
    },
    "meta": {
        "current_page": 1,
        "from": 1,
        "last_page": 1,
        "path": "https://example.app/mailcoach/api/campaigns/2/opens",
        "per_page": 15,
        "to": 1,
        "total": 1
    }
}
```

## Getting a sent campaign's clicks

To get the clicks of a campaign, create a `GET` call to the `/mailcoach/api/campaigns/<id>/clicks` endpoint.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl https://example.app/mailcoach/api/campaigns/99/clicks \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json'
```

You can sort on `unique_click_count` and `click_count`.
Searching by url can be done by using `?filter[search]=example.com`


If the API call succeeded, you'll get output like this

```json
{
    "data": [
        {
            "url": "https//example.app",
            "unique_click_count": 1,
            "click_count": 2
        }
    ],
    "links": {
        "first": "https://example.app/mailcoach/api/campaigns/2/clicks?page=1",
        "last": "https://example.app/mailcoach/api/campaigns/2/clicks?page=1",
        "prev": null,
        "next": null
    },
    "meta": {
        "current_page": 1,
        "from": 1,
        "last_page": 1,
        "path": "https://example.app/mailcoach/api/campaigns/2/clicks",
        "per_page": 15,
        "to": 1,
        "total": 1
    }
}
```

## Getting a sent campaign's unsubscribes

To get the unsubscribes of a campaign, create a `GET` call to the `/mailcoach/api/campaigns/<id>/unsubscribes` endpoint.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl https://example.app/mailcoach/api/campaigns/99/unsubscribes \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json'
```

You can sort on `created_at`.
Searching by email, first_name or last_name can be done by using `?filter[search]=john`

If the API call succeeded, you'll get output like this

```json
{
    "data": [
        {
            "campaign_id": 1,
            "subscriber_id": 1,
            "subscriber_email": "john@doe.com"
        }
    ],
    "links": {
        "first": "https://example.app/mailcoach/api/campaigns/2/unsubscribes?page=1",
        "last": "https://example.app/mailcoach/api/campaigns/2/unsubscribes?page=1",
        "prev": null,
        "next": null
    },
    "meta": {
        "current_page": 1,
        "from": 1,
        "last_page": 1,
        "path": "https://example.app/mailcoach/api/campaigns/2/unsubscribes",
        "per_page": 15,
        "to": 1,
        "total": 1
    }
}
```

## Error handling

If an error occured, you'll be given a non-HTTP/200 response code. The resulting payload might look like this.

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
