---
title: Upgrading
---

## Upgrading to v2

### Laravel 7

Mailcoach v2 requires Laravel 7, make sure to [upgrade your project](https://laravel.com/docs/7.x/upgrade#upgrade-7.0) first.

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

### Database changes

Make sure to add the following changed columns to your database table.

#### mailcoach_email_lists
Added:
```
$table->string('campaign_mailer');
$table->string('transactional_mailer');
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
