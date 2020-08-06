---
title: Introduction
---

The Mailcoach lets you perform multiple actions through a simple, structured Application Programming Interface (API).

Let's help get you started.

## API Endpoints

All endpoints are registered at /mailcoach/api using the default configuration, you can change this when registering the route macro. From there on, you will find a logical structure that follows to the REST standard.

Here's a quick summary of the API methods.

- `GET`: all GET requests are for data retrieval only and will never modify any data.
- `POST`: a POST method will add new items to Mailcoach
- `DELETE`: the DELETE method is used to delete items.
- `PUT`: this method is used to update information on existing items.

In general, GET requests can be performed as many times as you'd like (they are idempotent), all other methods actually transform data in your account and will cause changes to take effect.

## Response data

All responses from the Mailcoach API will be formatted as JSON.

Here's an example payload of the /api/templates endpoint, that lists all templates.

```json
{
  "data": [
    {
      "id": 1,
      "name": "Newsletter template",
      ...
    },
    {
      "id": 2,
      "name": "A second template",
      ...
    }
  ]
}
```
