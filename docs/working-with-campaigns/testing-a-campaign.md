---
title: Testing a campaign
weight: 2
---

Before sending a campaign to an entire list, you can send a test to a given email address.

```php
// to a single email address
$campaign->sendTestMail('john@example.com');

// to multiple email addresses at once
$campaign->sentTestMail(['john@example.com', 'paul@example.com'])
```

In the sent mail [the placeholders](https://docs.spatie.be/laravel-email-campaigns/v1/working-with-campaigns/creating-a-campaign/#setting-the-content-and-using-placeholders) won't be replaced. Rest assured that when you send the mail to your entire list, they will be replaced.
