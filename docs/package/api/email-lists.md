---
title: Email Lists
---

## Get all email lists

The `/mailcoach/api/email-lists` endpoint lists all your email lists.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl https://example.app/mailcoach/api/email-lists \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json'
```

Searching on name is possible on this endpoint using `?filter[search]=searchterm` for searching.

Sorting is possible on this endpoint on `name`, `created_at` and `active_subscribers_count`. For example `?sort=-updated_at` to sort descending on `updated_at`

As a result, you get the details of all your email lists:

```json
{
    "data": [
        {
            "id": 1,
            "uuid": "a69075c6-ff5c-4b6a-b217-12026cb72e4f",
            "name": "debitis",
            "active_subscribers_count": 0,
            "campaigns_feed_enabled": true,
            "default_from_email": "hertha.tremblay@rau.com",
            "default_from_name": "Davin Daniel",
            "allow_form_subscriptions": false,
            "redirect_after_subscribed": null,
            "redirect_after_already_subscribed": null,
            "redirect_after_subscription_pending": null,
            "redirect_after_unsubscribed": null,
            "requires_confirmation": false,
            "confirmation_mail_subject": null,
            "confirmation_mail_content": null,
            "confirmation_mailable_class": null,
            "campaign_mailer": "log",
            "transactional_mailer": "log",
            "send_welcome_mail": false,
            "welcome_mail_subject": null,
            "welcome_mail_content": null,
            "welcome_mailable_class": null,
            "welcome_mail_delay_in_minutes": 0,
            "report_recipients": null,
            "report_campaign_sent": false,
            "report_campaign_summary": false,
            "report_email_list_summary": false,
            "email_list_summary_sent_at": null,
            "created_at": "2020-08-06T13:12:08.000000Z",
            "updated_at": "2020-08-06T13:12:08.000000Z"
        },
        ...
    ],
    "links": {
        "first": "https://example.app/mailcoach/api/email-lists?page=1",
        "last": "https://example.app/mailcoach/api/email-lists?page=1",
        "prev": null,
        "next": null
    },
    "meta": {
        "current_page": 1,
        "from": 1,
        "last_page": 1,
        "path": "https://example.app/mailcoach/api/email-lists",
        "per_page": 15,
        "to": 3,
        "total": 3
    }
}
```

## Get a specific email list

If you don't want to retrieve all email-lists, you can get a specific email list if you know its ID. The example below will get the details of email list ID 99.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl https://example.app/mailcoach/api/email-lists/99 \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json'
```

Response:

```json
{
    "data": {
        "id": 99,
        "uuid": "a69075c6-ff5c-4b6a-b217-12026cb72e4f",
        "name": "debitis",
        "active_subscribers_count": 0,
        "campaigns_feed_enabled": true,
        "default_from_email": "hertha.tremblay@rau.com",
        "default_from_name": "Davin Daniel",
        "allow_form_subscriptions": false,
        "redirect_after_subscribed": null,
        "redirect_after_already_subscribed": null,
        "redirect_after_subscription_pending": null,
        "redirect_after_unsubscribed": null,
        "requires_confirmation": false,
        "confirmation_mail_subject": null,
        "confirmation_mail_content": null,
        "confirmation_mailable_class": null,
        "campaign_mailer": "log",
        "transactional_mailer": "log",
        "send_welcome_mail": false,
        "welcome_mail_subject": null,
        "welcome_mail_content": null,
        "welcome_mailable_class": null,
        "welcome_mail_delay_in_minutes": 0,
        "report_recipients": null,
        "report_campaign_sent": false,
        "report_campaign_summary": false,
        "report_email_list_summary": false,
        "email_list_summary_sent_at": null,
        "created_at": "2020-08-06T13:12:08.000000Z",
        "updated_at": "2020-08-06T13:12:08.000000Z"
    }
}
```

## Add an email list

To add an email list, create a `POST` call to the `/mailcoach/api/email-lists/` endpoint.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl -X POST https://example.app/mailcoach/api/email-lists \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{"name":"My subscribers", "default_from_email":"john@doe.com"}'
```

The only required field is the email list's `name` and `default_from_email`.

These are all the available fields with their validation rules:

- `name` => required
- `default_from_email` => required | valid email
- `default_from_name` => string
- `campaign_mailer` => valid mailer (defined in laravel config)
- `transactional_mailer` => valid mailer (defined in laravel config)
- `campaigns_feed_enabled` => boolean
- `report_recipients` => comma delimited emails
- `report_campaign_sent` => boolean
- `report_campaign_summary` => boolean
- `report_email_list_summary` => boolean
- `allow_form_subscriptions` => boolean
- `requires_confirmation` => boolean
- `allowed_form_subscription_tags` => array
- `redirect_after_subscribed` => string
- `redirect_after_already_subscribed` => string
- `redirect_after_subscription_pending` => string
- `redirect_after_unsubscribed` => string
- `welcome_mail` => 'do_not_send_welcome_mail' or 'send_default_welcome_mail' or 'send_custom_welcome_mail'
- `welcome_mail_subject` => required if `welcome_mail` = 'send_custom_welcome_mail'
- `welcome_mail_content` => required if `welcome_mail` = 'send_custom_welcome_mail'
- `welcome_mail_delay_in_minutes` => number
- `confirmation_mail` => 'send_default_confirmation_mail' or 'send_custom_confirmation_mail'
- `confirmation_mail_subject` => required if `custom_confirmation_mail` = 'send_custom_confirmation_mail'
- `confirmation_mail_content` => required if `custom_confirmation_mail` = 'send_custom_confirmation_mail'

If the API call succeeded, you'll be given output like this.

```json
{
    "data": {
        "id": 99,
        "uuid": "a69075c6-ff5c-4b6a-b217-12026cb72e4f",
        "name": "My subscribers",
        ...
    }
}
```

## Update an email list

To update an email list, create a `PUT` call to the `/mailcoach/api/email-lists/<id>` endpoint. In the example below we're updating the email list with ID 99. When update you should pass all fields mentioned in the payload above.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl -X PUT https://example.app/mailcoach/api/email-lists/99 \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{"name":"Updated name", "default_from_email":"john@doe.com"}'
```

Required fields and validation are the same as the create endpoint.

If the API call succeeded, you'll be given output like this.

```json
{
    "data": {
        "id": 99,
        "uuid": "a69075c6-ff5c-4b6a-b217-12026cb72e4f",
        "name": "Updated name",
        ...
    }
}
```

## Delete an email list

To delete an email list, create a `DELETE` call to the `/mailcoach/api/email-lists/<id>` endpoint. In the example below we're deleting the email list with ID 99.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl -X DELETE https://example.app/mailcoach/api/email-lists/99 \
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
