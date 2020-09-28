---
title: Mailgun
---

The `spatie/laravel-mailcoach-mailgun-feedback` package can handle bounce feedback coming from Mailgun. All e-mail address that permanently bounce will be unsubscribed from all lists.

You can install the add-on package via composer:

```bash
composer require spatie/laravel-mailcoach-mailgun-feedback:^2.0
```

## Adding the webhooks route

You must use this route macro in your route service provider. We recommend to not apply the `web` group middleware to this route as that would cause an unnecessary session to be started for each webhook call.

You can replace `mailgun-feedback` with any url you'd like.


```php
Route::mailgunFeedback('mailgun-feedback');
```

## Configuring webhooks

At Mailgun you must [configure a new webhook](https://www.mailgun.com/blog/a-guide-to-using-mailguns-webhooks/).

At the webhooks settings screen, on mailgun, you must add the `clicked`, `complained`, `opened` and `permanent_fail` webhooks and point them to the route you configured. In the screenshot below we configured the webhooks using a `ngrok.io` domain.

![Mailgun webhooks](https://mailcoach.app/images/docs/v3/mailgun-webhooks.png)

At the domain settings you must enable click and open tracking

![Mailgun webhooks](https://mailcoach.app/images/docs/v3/mailgun-domain-settings.png)


In the `mailcoach` config file you must add this section.

```php
// in config/mailcoach.php

    'mailgun_feedback' => [
        'signing_secret' => env('MAILGUN_SIGNING_SECRET'),
   ],
```

In your `.env` you must add a key `MAILGUN_SIGNING_SECRET` with the Mailgun signing secret you'll find at the Mailgun dashboard as its value. 

## Using the correct mail driver

If you haven't done so already, you must configure Laravel to use the Mailgun driver. Follow the instruction in [the mail section of the Laravel docs](https://laravel.com/docs/7.x/mail#driver-prerequisites).

## Informing Mailgun 

Before start sending campaigns via Mailgun we highly recommend getting in touch with their support and let them know the amount of mails your email list contains. Usually they will adjust the sending limits of your account so it's not a problem to send a large volumes in a short amount of time.
