---
title: Installation & setup
---

Mailcoach can be installed as a package inside an existing Laravel application. This version of mailcoach does not come with any user management, instead your can [use a gate check to determine who can access Mailcoach](https://mailcoach.app/docs/package/using-the-ui/authorizing-users).

In order to install Mailcoach you'll need to [get a license](/docs/app/general/getting-a-license) first.
 
 First, add the `satis.mailcoach.app` repository in your `composer.json`.

```php
"repositories": [
    {
        "type": "composer",
        "url": "https://satis.mailcoach.app"
    }
],
```

Next, you need to create a file called `auth.json` and place it either next to the `composer.json` file in your project, or in the composer home directory. You can determine the composer home directory on *nix machines by using this command.

```bash
composer config --list --global | grep home
```

This is the content you should put in `auth.json`:

```php
{
    "http-basic": {
        "satis.mailcoach.app": {
            "username": "<YOUR mailcoach.app USERNAME HERE>",
            "password": "<YOUR-LICENSE-KEY-HERE>"
        }
    }
}
```

With the configuration above in place you'll be able to install the package into your project using this command:

```bash
composer require "spatie/laravel-mailcoach:^1.0.0"
```

## Configure an email sending service

Mailcoach can send out mail via various email sending services and can handle the feedback (opens, clicks, bounces, complaints) those services give.

Follow the instruction on the dedicated docs page of each supported service.

- [Amazon SES](/docs/package/handling-feedback/amazon-ses)
- [Mailgun](/docs/package/handling-feedback/mailgun)
- [Sendgrid](/docs/package/handling-feedback/sendgrid)

## Prepare the database

You need to publish and run the migration:

```bash
php artisan vendor:publish --provider="Spatie\Mailcoach\MailcoachServiceProvider" --tag="mailcoach-migrations"
php artisan migrate
```

## Add the route macro

You must register the routes needed to handle subscription confirmations, open, and click tracking. We recommend that you don't put this in your routes file, but in the `map` method of `RouteServiceProvider`

```php
Route::mailcoach('mailcoach');
```

## Schedule the commands

In the console kernel, you should schedule these commands.

```php
// in app/Console/Kernel.php
protected function schedule(Schedule $schedule)
{
    // ...
    $schedule->command('mailcoach:calculate-statistics')->everyMinute();
    $schedule->command('mailcoach:send-scheduled-campaigns')->everyMinute();
    $schedule->command('mailcoach:send-campaign-summary-mail')->hourly();
    $schedule->command('mailcoach:send-email-list-summary-mail')->mondays()->at('9:00');
    $schedule->command('mailcoach:delete-old-unconfirmed-subscribers')->daily();
}
```

## Publish the assets

You must publish the JavaScript and CSS assets using this command:

```bash
php artisan vendor:publish --tag mailcoach-assets --force
```

To ensure that these assets get republished each time Mailcoach is updated, we highly recommend you add the following command to the `post-update-cmd` of the `scripts` section of your `composer.json`.

```php
    "scripts": {
        "post-update-cmd": [
            "@php artisan vendor:publish --tag mailcoach-assets --force"
        ]
    }
```

## Publish the config file

Optionally, You can publish the config file with this command.

```bash
php artisan vendor:publish --provider="Spatie\Mailcoach\MailcoachServiceProvider" --tag="mailcoach-config"
```

Below is the default content of the config file:

```php
return [
    /*
     * The date format used on all screens of the UI
     */
    'date_format' => 'Y-m-d H:i',

    /*
     * Replacers are classes that can make replacements in the html of a campaign.
     *
     * You can use a replacer to create placeholders.
     */
    'replacers' => [
        \Spatie\Mailcoach\Support\Replacers\WebviewReplacer::class,
        \Spatie\Mailcoach\Support\Replacers\SubscriberReplacer::class,
        \Spatie\Mailcoach\Support\Replacers\EmailListReplacer::class,
        \Spatie\Mailcoach\Support\Replacers\UnsubscribeUrlReplacer::class,
    ],

    /*
     * Here you can specify which jobs should run on which queues.
     * Use an empty string to use the default queue.
     */
    'perform_on_queue' => [
        'calculate_statistics_job' => 'mailcoach',
        'send_campaign_job' => 'send-campaign',
        'send_mail_job' => 'send-mail',
        'send_test_mail_job' => 'mailcoach',
        'process_feedback_job' => 'mailcoach-feedback'
    ],

    /*
     * By default only 10 mails per second will be sent to avoid overwhelming your
     * e-mail sending service. To use this feature you must have Redis installed.
     */
    'throttling' => [
        'enabled' => true,
        'redis_connection_name' => 'default',
        'redis_key' => 'laravel-mailcoach',
        'allowed_number_of_jobs_in_timespan' => 10,
        'timespan_in_seconds' => 1,
        'release_in_seconds' => 5,
    ],

      /*
       * You can customize some of the behavior of this package by using our own custom action.
       * Your custom action should always extend the one of the default ones.
       */
    'actions' => [
        /*
         * Actions concerning campaigns
         */
        'calculate_statistics' => \Spatie\Mailcoach\Actions\Campaigns\CalculateStatisticsAction::class,
        'convert_html_to_text' => \Spatie\Mailcoach\Actions\Campaigns\ConvertHtmlToTextAction::class,
        'personalize_html' => \Spatie\Mailcoach\Actions\Campaigns\PersonalizeHtmlAction::class,
        'prepare_email_html' => \Spatie\Mailcoach\Actions\Campaigns\PrepareEmailHtmlAction::class,
        'prepare_webview_html' => \Spatie\Mailcoach\Actions\Campaigns\PrepareWebviewHtmlAction::class,
        'retry_sending_failed_sends' => \Spatie\Mailcoach\Actions\Campaigns\RetrySendingFailedSendsAction::class,
        'send_campaign' => \Spatie\Mailcoach\Actions\Campaigns\SendCampaignAction::class,
        'send_mail' => \Spatie\Mailcoach\Actions\Campaigns\SendMailAction::class,
        'send_test_mail' => \Spatie\Mailcoach\Actions\Campaigns\SendTestMailAction::class,

        /*
         * Actions concerning subscribers
         */
        'confirm_subscriber' => \Spatie\Mailcoach\Actions\Subscribers\ConfirmSubscriberAction::class,
        'create_subscriber' => \Spatie\Mailcoach\Actions\Subscribers\CreateSubscriberAction::class,
        'import_subscribers' => \Spatie\Mailcoach\Actions\Subscribers\ImportSubscribersAction::class,
        'send_confirm_subscriber_mail' => \Spatie\Mailcoach\Actions\Subscribers\SendConfirmSubscriberMailAction::class,
        'send_welcome_mail' => \Spatie\Mailcoach\Actions\Subscribers\SendWelcomeMailAction::class,
        'update_subscriber' => \Spatie\Mailcoach\Actions\Subscribers\UpdateSubscriberAction::class,
    ],

    /*
     * Unauthorized users will get redirected to this route.
     */
    'redirect_unauthorized_users_to_route' => 'login',

    /*
     *  This configuration option defines the authentication guard that will
     *  be used to protect your the Mailcoach UI. This option should match one
     *  of the authentication guards defined in the "auth" config file.
     */
    'guard' => env('MAILCOACH_GUARD', null),

    /*
     *  These middleware will be assigned to every Mailcoach UI route, giving you the chance
     *  to add your own middleware to this stack or override any of the existing middleware.
     */
    'middleware' => [
        'web',
        Spatie\Mailcoach\Http\App\Middleware\Authenticate::class,
        Spatie\Mailcoach\Http\App\Middleware\Authorize::class,
        Spatie\Mailcoach\Http\App\Middleware\SetMailcoachDefaults::class,
    ]
];
```

## Install and configure redis

It's common for e-mail providers to limit the number of e-mails you can send within a given amount of time. The package uses Redis to throttle e-mails, so make sure it's available on your system. You must specify a valid Redis connection name in the `throttling.redis_connection_name` key.

By default, we set this value to the default Laravel connection name, `default`.

## Install Horizon

This package handles various tasks in a queued way via [Laravel Horizon](https://laravel.com/docs/master/horizon). If your application doesn't have Horizon installed yet, follow [their installation instructions](https://laravel.com/docs/master/horizon#installation).

After Horizon is installed, don't forget to set `QUEUE_CONNECTION` in your `.env` file to `redis`.

`config/horizon.php` should have been created in your project. In this config file you must add a block named `mailcoach` to both the `production` and `local` environment.

```php
'environments' => [
    'production' => [
        'supervisor-1' => [
            'connection' => 'redis',
            'queue' => ['default'],
            'balance' => 'simple',
            'processes' => 10,
            'tries' => 2,
        ],
        'mailcoach' => [
            'connection' => 'mailcoach-redis',
            'queue' => ['send-campaign', 'send-mail', 'mailcoach-feedback', 'mailcoach'],
            'balance' => 'auto',
            'processes' => 3,
            'tries' => 1,
            'timeout' => 60 * 10,
        ],
    ],

    'local' => [
        'supervisor-1' => [
            'connection' => 'redis',
            'queue' => ['default'],
            'balance' => 'simple',
            'processes' => 10,
            'tries' => 2,
        ],

        'mailcoach' => [
            'connection' => 'mailcoach-redis',
            'queue' => ['send-campaign', 'send-mail', 'mailcoach-feedback', 'mailcoach'],
            'balance' => 'auto',
            'processes' => 3,
            'tries' => 1,
            'timeout' => 60 * 10,
        ],
    ],
],
```

In the `config/queue.php` file you must add the `mailcoach-redis` connection:

```php
'connections' => [

    // ...

    'mailcoach-redis' => [
        'driver' => 'redis',
        'connection' => 'default',
        'queue' => env('REDIS_QUEUE', 'default'),
        'retry_after' => 11 * 60,
        'block_for' => null,
    ],
```

## Add authorization to Mailcoach UI

By default you can only use the Mailcoach on a local environment. To use it in other environment you need to register an gate check. Head over to [the section on authorizing users](/docs/package/using-the-ui/authorizing-users) to learn more.


## Specifying a timezone

By default all dates in Mailcoach are in UTC. If you want to use another timezone, you can change the `timezone` setting in the `config/app.php` config file.

## Visit the UI

After performing all these steps, you should be able to visit the Mailcoach UI at `/mailcoach`.

Before sending out a real campaign, we highly recommend creating a small email list with a couple of test email addresses and send a campaign to it. This way, you can verify that sending mails, and the open & click tracking are all working correctly.
