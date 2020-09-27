---
title: Upgrading
---

## Upgrading to v2

### Laravel 8

Mailcoach v3 requires Laravel 8, make sure to [upgrade your project](https://laravel.com/docs/8.x/upgrade#upgrade-8.0) first.

Mailcoach uses job batching under the hood. Make sure you add the required database table, as [mentioned in the Laravel docs on Job batching](https://laravel.com/docs/8.x/queues#job-batching).

## Upgrading the database schema

In your database you should add a few columns:

#### mailcoach_campaigns

- `reply_to_email`: string, nullable
- `reply_to_name`: string, nullable

#### mailcoach_subscribers

- `imported_via_import_uuid`: uuid, nullable

#### mailcoach_subscriber_imports

- `subscribe_unsubscribed` : boolean, default: false
- `unsubscribe_others`: boolean, default false,

#### mailcoach_email_lists

- `default_reply_to_email`: string, nullable
- `default_reply_to_name`: string, nullable
- `allowed_form_extra_attributes`: text, nullable,

#### mailcoach_sends

- add an index on `uuid`

## Upgrading database content

- `open_rate`, `click_rate`, `bounce_rate`, `unsubscribe_rate`: v3 of mailcoach now assumes that the two last numbers are the digits. For campaigns that were sent using v2 you should add two zeroes, so `31` should become `3100`

## Updating the config file

The `middleware` option now contains an array with `web` and `api`. This is the new default:

```php
    /*
     *  These middleware will be assigned to every Mailcoach routes, giving you the chance
     *  to add your own middleware to this stack or override any of the existing middleware.
     */
    'middleware' => [
        'web' => [
            'web',
            Spatie\Mailcoach\Http\App\Middleware\Authenticate::class,
            Spatie\Mailcoach\Http\App\Middleware\Authorize::class,
            Spatie\Mailcoach\Http\App\Middleware\SetMailcoachDefaults::class,
        ],
        'api' => [
            'api',
            'auth:api',
        ],
    ],
```

## Horizon configuration

We now suggest a new horizon configuration for balancing the queue that Mailcoach uses, make sure `mailcoach-general` and `mailcoach-heavy` are present in your production and local Horizon environments:

```php
// config/horizon.php
'environments' => [
    'production' => [
        'supervisor-1' => [
            'connection' => 'redis',
            'queue' => ['default'],
            'balance' => 'simple',
            'processes' => 10,
            'tries' => 2,
            'timeout' => 60 * 60,
        ],
        'mailcoach-general' => [
            'connection' => 'mailcoach-redis',
            'queue' => ['mailcoach', 'mailcoach-feedback', 'send-mail'],
            'balance' => 'auto',
            'processes' => 10,
            'tries' => 2,
            'timeout' => 60 * 60,
        ],
        'mailcoach-heavy' => [
            'connection' => 'mailcoach-redis',
            'queue' => ['send-campaign'],
            'balance' => 'auto',
            'processes' => 3,
            'tries' => 1,
            'timeout' => 60 * 60,
        ],
    ],

    'local' => [
        'supervisor-1' => [
            'connection' => 'redis',
            'queue' => ['default'],
            'balance' => 'simple',
            'processes' => 10,
            'tries' => 2,
            'timeout' => 60 * 60,
        ],
        'mailcoach-general' => [
            'connection' => 'mailcoach-redis',
            'queue' => ['mailcoach', 'mailcoach-feedback', 'send-mail'],
            'balance' => 'auto',
            'processes' => 10,
            'tries' => 2,
            'timeout' => 60 * 60,
        ],
        'mailcoach-heavy' => [
            'connection' => 'mailcoach-redis',
            'queue' => ['send-campaign'],
            'balance' => 'auto',
            'processes' => 3,
            'tries' => 1,
            'timeout' => 60 * 60,
        ],
    ],
],
```