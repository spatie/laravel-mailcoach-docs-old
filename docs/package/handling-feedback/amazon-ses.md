---
title: Amazon SES
---

`laravel-mailcoach-ses-feedback` can handle bounce feedback coming from SES. All e-mail addresses that permanently bounce will be unsubscribed from all lists.

You can install the add-on package via composer:

```bash
composer require spatie/laravel-mailcoach-ses-feedback
```

## Migrating the database

Under the hood this package uses [spatie/laravel-webhook-client](https://github.com/spatie/laravel-webhook-client) to process handle webhooks. You are required to publish its migration to create the `webhook_calls` table. You can skip this step if your project already uses the `laravel-webhook-client` package directly.

```bash
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

### Simple Email Service Setup
1. In your AWS Management Console, create a configuration set if you haven't already

![](/images/docs/v1/package/ses-feedback/1.create-configuration-set.png)

2. Add a SNS destination in the Event Destinations and make sure to check the event types you would like to receive

![](/images/docs/v1/package/ses-feedback/2-1-add-destination.png)

![](/images/docs/v1/package/ses-feedback/2-2-add-destination.png)

3. Create a new topic for this destination

![](/images/docs/v1/package/ses-feedback/3-create-new-topic.png)

### Simple Notification Service Setup

> First, make sure your endpoint is accessible, if you're installing this locally you'll need to share your local environment using `valet share` or a service like `ngrok`

![](/images/docs/v1/package/ses-feedback/4-1-create-subscription.png)

![](/images/docs/v1/package/ses-feedback/4-2-create-subscription.png)

1. Create a subscription for the topic you just created, use `HTTPS` as the Protocol
2. Enter the endpoint you just created the route for
3. Do **not** check "Enable raw message delivery", otherwise signature validation won't work
4. In **Delivery retry policy (HTTP/S)** make sure to set a limit in the **Maximum receive rate** setting, `5` / second is a good default as that is the default php-fpm pool size.
5. You can leave all other settings on their defaults
6. Your subscription should be automatically confirmed if the endpoint was reachable

![](/images/docs/v1/package/ses-feedback/5-subscription-confirmed.png)

## Setting the configuration name in your Laravel app

This package automatically adds the correct `X-Configuration-Set` header for Amazon SES to process feedback. Make sure the name of your configuration set is available under the `mailcoach.ses_feedback.configuration_set` configuration key.

Here's an example for a configuration set that is named `mailcoach`:

```php
// in config/mailcoach.php

'ses_feedback' => [
    'configuration_set' => 'mailcoach',
]

```

## Using the correct mail driver

If you haven't done so already, you must configure Laravel to use the Amazon SES driver. Follow the instruction in [the mail section of the Laravel docs](https://laravel.com/docs/v1/master/mail#driver-prerequisites).

