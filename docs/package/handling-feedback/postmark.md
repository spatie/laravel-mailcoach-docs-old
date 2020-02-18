---
title: Postmark
---

The `spatie/laravel-mailcoach-postmark-feedback` package can handle bounce feedback coming from Postmark. All e-mail address that permanently bounce will be unsubscribed from all lists.

You can install the add-on package via composer:

```bash
composer require spatie/laravel-mailcoach-postmark-feedback
```

## Migrating the database

Under the hood this package uses [spatie/laravel-webhook-client](https://github.com/spatie/laravel-webhook-client) to process handle webhooks. You are required to publish its migration to create the `webhook_calls` table. You can skip this step if your project already uses the `laravel-webhook-client` package directly.

```php
php artisan vendor:publish --provider="Spatie\WebhookClient\WebhookClientServiceProvider" --tag="migrations"
```

After the migration has been published, you can create the `webhook_calls` table by running the migrations:

```php
php artisan migrate
```

## Adding the webhooks route

You must use this route macro in your route service provider. We recommend to not apply the `web` group middleware to this route as that would cause an unnecessary session to be started for each webhook call.

You can replace `postmark-feedback` with any url you'd like.


```php
Route::postmark('postmark-feedback');
```

## Configuring webhooks

At Post you must [configure a new webhook](https://postmarkapp.com/support/article/1067-how-do-i-enable-delivery-webhooks).

At the webhooks settings screen, on Postmark, you must add the `Open`, `Bounce`, `Spam Complaint` and `Link Click` webhooks and point them to the route you configured. 

In the `mailcoach` config file you must add this section.

```php
// in config/mailcoach.php

    'post_feedback' => [
        'signing_secret' => env('POSTMARK_SIGNING_SECRET'),
   ],
```

In your `.env` you must add a key `POSTMARK_SIGNING_SECRET` the value should be set in the format: `<username>:<password>`. Here's an example:

```
POSTMARK_SIGNING_SECRET=your-user:your-password
```

You must use this route macro somewhere in your routes file. Replace `'postmark-feeback'` with the url you specified at Postmark when setting up the webhook there.

## Using the correct mail driver

If you haven't done so already, you must configure Laravel to use the Postmark driver. Follow the instruction in [the mail section of the Laravel docs](https://laravel.com/docs/master/mail#driver-prerequisites).

## Informing Postmark 

Before start sending campaigns via Postmark we highly recommend getting in touch with their support and let them know you want to start sending bulk emails.
