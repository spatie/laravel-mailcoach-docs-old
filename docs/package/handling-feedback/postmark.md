---
title: Postmark
---

The `spatie/laravel-mailcoach-postmark-feedback` package can handle bounce feedback coming from Postmark. All e-mail addresses that permanently bounce will be unsubscribed from all lists.

You can install the add-on package via composer:

```bash
composer require spatie/laravel-mailcoach-postmark-feedback:^2.0
```

## Adding the webhooks route

You must use this route macro in your route service provider. Do **NOT** apply the `web` group middleware to this route as that would cause an unnecessary session to be started for each webhook call.

You can replace `postmark-feedback` with any url you'd like.


```php
Route::postmarkFeedback('postmark-feedback');
```

## Configuring webhooks

At Postmark you must [configure a new webhook](https://postmarkapp.com/support/article/1067-how-do-i-enable-delivery-webhooks).

At the webhooks settings screen on Postmark, you must specify the webhook URL. That url should start with the domain you installed mailcoach on, followed by `/postmark-feedback`.

You should add a custom header named `mailcoach-signature`, and you can choose a value that you should keep secret.
 
 you must add the `Open`, `Bounce`, `Spam Complaint` and `Link Click` webhooks and point them to the route you configured. 

![](https://mailcoach.app/images/docs/v3/package/postmark/postmark-webhooks.png)

On the settings screen in Postmark, you should also enable open and link tracking

![](https://mailcoach.app/images/docs/v3/package/postmark/postmark-tracking.png)


In the `mailcoach` config file you must add this section.

```php
// in config/mailcoach.php

    'postmark_feedback' => [
        'signing_secret' => env('POSTMARK_SIGNING_SECRET'),
   ],
```

In your `.env` you must add a key `POSTMARK_SIGNING_SECRET` the value should be set to the value you specified in the `mailcoach-signature` on the Postmark webhook settings screen:

```
POSTMARK_SIGNING_SECRET=
```

## Setting the message stream in your Laravel app

This package automatically adds the correct `X-PM-Message-Stream` header for Postmark Broadcast support. Make sure the name of your configuration set is available under the `mailcoach.postmark.message_stream` configuration key.

Here's an example for a configuration set that is named `mailcoach`:

```php
// in config/mailcoach.php

'postmark' => [
    'message_stream' => 'newsletters',
]

```

## Using the correct mail driver

If you haven't done so already, you must configure Laravel to use the Postmark driver. Follow the instruction in [the mail section of the Laravel docs](https://laravel.com/docs/7.x/mail#driver-prerequisites).

## Informing Postmark 

Before start sending campaigns via Postmark we highly recommend getting in touch with their support and let them know you want to start sending bulk emails.
