---
title: Troubleshooting errors during sending
---

When sending a campaign, the package will create `Send` models for each mail to be sent. A `Send` has a property `sent_at` that stores the date time of when an email was actually sent. If that attribute is `null` the email has not yet been sent.

If you experience problems while sending, and the state of your queues is been lost, you should dispatch a `SendMailJob` for each `Send` that has `sent_at` set to `null`.

```php
Send::whereNull('sent_at')->each(function(Send $send) {
   dispatch(new SendMailJob($campaigSend);
}
```

You can run the above code by executing this command:

```bash
php artisan email-campaigns:retry-pending-sends
```

Should, for any reason, two jobs for the same `Send` be scheduled, it is highly likely that only one mail will be sent. After a `SendMailJob` has sent an email it will update `sent_at` with the current timestamp. The job will not send a mail for a `SendMail` whose `sent_at` is not set to `null`.
