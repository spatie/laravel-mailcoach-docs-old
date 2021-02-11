---
title: Displaying webviews
---

Whenever you send a campaign, a webview is also created. A webview is a hard to guess URL that people who didn't subscribe can visit to read the content of your campaign.

You can get to the URL of the webview of a campaign:

```php
$campaign->webViewUrl();
```

## Customizing the webview

You can customize the webview. To do this, you must publish all views:

```bash
php artisan vendor:publish --provider="Spatie\Mailcoach\MailcoachServiceProvider" --tag="views"
```

After that, you can customize the `webview.blade.php` view in the `resources/views/vendor/mailcoach/campaign` directory.
