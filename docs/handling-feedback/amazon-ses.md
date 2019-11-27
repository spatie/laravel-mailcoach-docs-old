---
title: Amazon SES
---

`laravel-mailcoach-ses-feedback` can handle bounce feedback coming from SES. All e-mail address that permanently bounce will be unsubscribed from all lists.

You can install the add-on package via composer:

```bash
composer require spatie/laravel-mailcoach-ses-feedback
```

## Migrating the database

Under the hood this package uses [spatie/laravel-webhook-client](https://github.com/spatie/laravel-mailcoach) to process handle webhooks. You are required to publish its migration to create the `webhook_calls` table. You can skip this step if your project already uses the `laravel-webhook-client` package directly.

```php
php artisan vendor:publish --provider="Spatie\WebhookClient\WebhookClientServiceProvider" --tag="migrations"
```

After the migration has been published, you can create the `webhook_calls` table by running the migrations:

```php
php artisan migrate
```

## Adding the webhooks route

You must use this route macro in your route service provider. We recommend to not apply the `web` group middleware to this route as that would cause an unnecessary session to be started for each webhook call.

You can replace `ses-feedback` with any url you'd like.

```php
Route::sesFeedback('ses-feedback');
```

## Configuring webhooks at Amazon SES


### Simple Email Service
1. Create a configuration set in your AWS SES console if you haven't already
2. Add a SNS destination in the Event Destinations
    - Make sure to check the **bounce** and **complaint** types
3. Create a new topic for this destination

### Simple Notification Service
1. Create a subscription for the topic you just created, use `HTTPS` as the Protocol
2. Use the endpoint you just created the route for
3. Do **not** check "Enable raw message delivery", otherwise signature validation won't work
4. Your subscription should be automatically confirmed if the endpoint was reachable 

## Using the correct mail driver

If you haven't done so already, you must configure Laravel to use the Amazon SES driver. Follow the instruction in [the mail section of the Laravel docs](https://laravel.com/docs/master/mail#driver-prerequisites).
