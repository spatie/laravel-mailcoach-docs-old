---
title: Upgrading
---

## Upgrading to v2

### Laravel 7

Mailcoach v2 requires Laravel 7, make sure to [upgrade your project](https://laravel.com/docs/v2/7.x/upgrade#upgrade-7.0) first.

### Media library v8

Medialibrary has been upgraded to v8, please read the [upgrade guide](https://github.com/spatie/laravel-medialibrary/blob/master/UPGRADING.md#from-v7-to-v8) to make any changes necessary to your project.

If you were not using media library in any way in your own code, you only need to create and run a migration to add this column to the `media` table.

```php
$table->string('conversions_disk')->nullable();
$table->uuid('uuid')->nullable();
```

### View changes
All the views have changed, if you have published views that you've modified, make sure to republish them and port your changes.

### Config changes
Add the following new configuration keys to your `mailcoach.php` config file and set them to your preferred mailers. You can also keep these `null` and Mailcoach will use the default app mailer.

```php
/*
 * The mailer used by Mailcoach for password resets and summary emails.
 * Mailcoach will use the default Laravel mailer if this is not set.
 */
'mailer' => null,

/*
 * The default mailer used by Mailcoach for campaign sends. This can
 * be overridden on a List level.
 */
'campaign_mailer' => null,

/*
 * The default mailer used by Mailcoach for double opt-in and confirmation mails.
 * This can be overridden on a List level.
 */
'transactional_mailer' => null,

/**
 * Here you can configure which template editor Mailcoach uses.
 * By default this is a text editor that highlights HTML.
 */
'editor' => \Spatie\Mailcoach\Support\Editor\TextEditor::class,
```

### Update the feed back package.

If you're tracking opens and links you need to update the feedback package you are using to v2.

Update `^1.0` for `^2.0` for the `spatie/laravel-mailcoach-<your email provider>-feedback` you have installed

### Database changes

Create and run a migration to add the following changed columns to your database table.

#### mailcoach_email_lists
Added:
```
$table->string('campaign_mailer')->nullable();
$table->string('transactional_mailer')->nullable();
$table->integer('welcome_mail_delay_in_minutes')->default(0);
```

#### mailcoach_campaigns
Added:
```
$table->longText('structured_html')->nullable();
```

#### mailcoach_templates
Added:
```
$table->longText('structured_html')->nullable();
```
