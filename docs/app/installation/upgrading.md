---
title: Upgrading from v2 to v3
---

If you installed Mailcoach as a package inside an existing Laravel app, you need to follow [these instructions](https://mailcoach.app/docs/v3/package/general/upgrading).

An in-place upgrade of the full Mailcoach app is hard. Instead, we recommend creating a new fresh Mailcoach 2 app and updating your current database and settings to the new v2 format.

You can create a new Mailcoach app with this command

```bash
composer create-project spatie/Mailcoach
```

## Upgrading the database schema

In your database you should add a few columns:

#### mailcoach_subscribers

- `imported_via_import_uuid`: uuid, nullable

#### mailcoach_subscriber_imports

- `subscribe_unsubscribed` : boolean, default: false
- `unsubscribe_others`: boolean, default false,

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

## New command for cleanup

We've added a new command for cleanup of processed feedback in the `webhook_calls` table, make sure to add this to your `\App\Console\Kernel` schedule. 

```php
$schedule->command('mailcoach:cleanup-processed-feedback')->hourly();
```
