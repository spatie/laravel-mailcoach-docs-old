---
title: Installation & setup
weight: 4
---

Use composer for installing the package:

```bash
composer require "spatie/laravel-email-campaigns:^1.0.0"
```

## Choosing a mail driver

Configure Laravel to use one of [the many available mail drivers](https://laravel.com/docs/master/mail#driver-prerequisites). All emails sent via the package will use that default driver.

## Prepare the database

You need to publish and run the migration:

```bash
php artisan vendor:publish --provider="Spatie\MailCoach\MailCoachServiceProvider" --tag="migrations"
php artisan migrate
```

## Add the route macro

You must register the routes needed to handle subscription confirmations, open, and click tracking. You can do that by adding this macro to your routes file.

```php
Route::mailCoach('mailcoach');
```

## Schedule the calculate statistics command

In the console kernel, you should schedule the `email-campaigns:calculate-statistics` to run every minute.

```php
// in app/Console/Kernel.php
protected function schedule(Schedule $schedule)
{
    // ...
    $schedule->command('email-campaigns:calculate-statistics')->everyMinute();
}
```

## Publish the config file

You must publish the config file with this command.

```bash
php artisan vendor:publish --provider="Spatie\MailCoach\MailCoachServiceProvider" --tag="config"
```

Below is the default content of the config file:

```php
return [

    /*
     * Here you can specify which jobs should run on which queues.
     * Use an empty string to use the default queue.
     */
    'perform_on_queue' => [
        'calculate_statistics_job' => '',
        'register_click_job' => '',
        'register_open_job' => '',
        'send_campaign_job' => '',
        'send_mail_job' => '',
        'send_test_mail_job' => '',
    ],

    /*
     * By default only 5 mails per second will be sent to avoid overwhelming your
     * e-mail sending service. To use this feature you must have Redis installed.
     */
    'throttling' => [
        'enabled' => false,
        'redis_connection_name' => 'default',
        'redis_key' => 'laravel-email-campaigns',
        'allowed_number_of_jobs_in_timespan' => 5,
        'timespan_in_seconds' => 1,
        'release_in_seconds' => 5,
    ],

    /*
       * You can customize some of the behavior of this package by using our own custom action.
       * Your custom action should always extend the one of the default ones.
       */
    'actions' => [
        'personalize_html_action' => \Spatie\MailCoach\Actions\PersonalizeHtmlAction::class,
        'prepare_email_html_action' => \Spatie\MailCoach\Actions\PrepareEmailHtmlAction::class,
        'prepare_webview_html_action' => \Spatie\MailCoach\Actions\PrepareWebviewHtmlAction::class,
        'subscribe_action' => \Spatie\MailCoach\Actions\SubscribeAction::class,
        'confirm_subscription_action' => \Spatie\MailCoach\Actions\ConfirmSubscriptionAction::class,
    ],
];
```

## Install and configure redis

It's common for e-mail providers to limit the number of e-mails you can send within a given amount of time. The package uses Redis to throttle e-mails, so make sure it's available on your system. You must specify a valid Redis connection name in the `throttling.redis_connection_name` key.

By default, we set this value to the default Laravel connection name, `default`.

## Prepare the queues

The package queues many tasks it performs. Because of this, use [a different queue driver](https://laravel.com/docs/master/queues#driver-prerequisites) than `sync`.

You're able to run different jobs in different queues.  Specify this using the `perform_on_queue` key in the `email-campaigns` config file. The `register_click_job`, `register_open_job`, and `send_mail_job` jobs could receive a large number of jobs. Using only one queue potentially results in a long wait for other jobs. So we recommend using a separate queue for the `register_click_job`, `register_open_job`, and `send_mail_job` jobs.

## Schedule commands

To send out scheduled campaigns, you must add this command to your scheduler

```php
// in app\Console\Kernel.php

    // ...

    protected function schedule(Schedule $schedule)
    {
        $schedule->command('email-campaigns:send-scheduled-campaigns')->everyMinute();
    }
``` 