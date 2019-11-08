---
title: SES
weight: 2
---

`laravel-mailcoach-ses-feedback` can handle bounce feedback coming from SES. All e-mail address that permanently bounce will be unsubscribed from all lists.

You can install the add-on package via composer:

```bash
composer require spatie/laravel-mailcoach-ses-feedback
```

Under the hood this package uses [spatie/laravel-webhook-client](https://github.com/spatie/laravel-mailcoach) to process handle webhooks. You are required to publish its migration to create the `webhook_calls` table. You can skip this step if your project already uses the `laravel-webhook-client` package directly.

```php
php artisan vendor:publish --provider="Spatie\WebhookClient\WebhookClientServiceProvider" --tag="migrations"
```

After the migration has been published, you can create the `webhook_calls` table by running the migrations:

```php
php artisan migrate
```

### Route

You must use this route macro somewhere in your routes file. Replace `'ses-feeback'` with the url you specified at SES when setting up the webhook there.

```php
Route::MailCoachSesFeedback('ses-feedback');
```

### Simple Email Service
1. Create a configuration set in your AWS SES console if you haven't already
2. Add a SNS destination in the Event Destinations
    - Make sure to check the **bounce** type
3. Create a new topic for this destination

### Simple Notification Service
1. Create a subscription for the topic you just created, use `HTTPS` as the Protocol
2. Use the endpoint you just created the route for
3. Do **not** check "Enable raw message delivery", otherwise signature validation won't work
4. Your subscription should be automatically confirmed if the endpoint was reachable 