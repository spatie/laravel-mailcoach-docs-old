---
title: API Authentication
---

For authentication, the Mailcoach API requires a logged in user through the `auth:api` guard. This can be configured by changing the `mailcoach.middleware.api` configuration.

Other than this, Mailcoach does nothing special regarding authentication and leaves all authentication up to your implementation needs.

> Our documentation examples assume an API endpoint that is using a Bearer token for authentication

## User endpoint

Mailcoach ships with a user endpoint to get the details of the currently logged in user.

```shell script
$ MAILCOACH_TOKEN="your API token"
$ curl https://example.app/mailcoach/api/user \
    -H "Authorization: Bearer $MAILCOACH_TOKEN" \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json'
```

As a result, you get the details of the user that this token belongs to

```json
{
  "data": {
    "id": 1,
    "email": "john@doe.com",
    "created_at": "2020-08-06T12:08:25.000000Z",
    "updated_at": "2020-08-06T12:08:25.000000Z"
  }
}
```
