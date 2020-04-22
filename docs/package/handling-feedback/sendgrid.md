---
title: Sendgrid
---

The `spatie/laravel-mailcoach-sendgrid-feedback` package can handle bounce feedback coming from Sendgrid. All e-mail address that permanently bounce will be unsubscribed from all lists.

You can install the add-on package via composer:

```bash
composer require spatie/laravel-mailcoach-sendgrid-feedback:^2.0
```

## Migrating the database

Under the hood this package uses [spatie/laravel-webhook-client](https://github.com/spatie/laravel-webhook-client)to process handle webhooks. You are required to publish its migration to create the `webhook_calls` table. You can skip this step if your project already uses the `laravel-webhook-client` package directly.

```php
php artisan vendor:publish --provider="Spatie\WebhookClient\WebhookClientServiceProvider" --tag="migrations"
```

After the migration has been published, you can create the `webhook_calls` table by running the migrations:

```php
php artisan migrate
```

## Adding the webhooks route

You must use this route macro in your route service provider. We recommend to not apply the `web` group middleware to this route as that would cause an unnecessary session to be started for each webhook call.

You can replace `sendgrid-feedback` with any url you'd like.


```php
Route::sendgridFeedback('sendgrid-feedback');
```

## Configuring webhooks

At Sendgrid you must [configure a new webhook](https://sendgrid.com/docs/v2/for-developers/tracking-events/getting-started-event-webhook/).

At the webhooks settings screen at sendgrid you must add the `Bounced`, `Opened`, `Clicked` and `Mark as Spam` webhooks and point them to the route your configured. In the screenshot below we configured the webhooks using a `ngrok.io` domain with a `?secret=yolo-no-real-signature` appended to the webhook url.

![Sendgrid webhooks](https://mailcoach.app/images/docs/v2/sendgrid-webhooks.png)

At the Tracking settings you must enable click and open tracking

![Mailgun webhooks](https://mailcoach.app/images/docs/v2/sendgrid-tracking-settings.png)

In the `mailcoach` config file you must add this section.

```php
// in config/mailcoach.php

    'sendgrid_feedback' => [
        'signing_secret' => env('SENDGRID_SIGNING_SECRET'),
   ],
```

In your `.env` you must add a key `SENDGRID_SIGNING_SECRET` with the Sendgrid signing secret you defined when setting up the webhook. 

## Using the correct mail driver

If you haven't done so already, you must configure Laravel to use the Sendgrid driver. Follow the instruction in [the mail section of the Laravel docs](https://laravel.com/docs/7.x/mail#driver-prerequisites).

## Informing Sendgrid 

Before start sending campaigns via Sendgrid we highly recommend getting in touch with their support and let them now the amount of mails your email list contains. Usually they will adjust the sending limits of your account so it's not a problem to send a large volumes in a short amount of time.
