---
title: Processing feedback
---

After a mail is sent, most email providers send feedback on the result.

You can handle that feedback for a certain provider by installing one of the add-on package we provide:
- [Amazon SES](https://github.com/spatie/laravel-mailcoach-ses-feedback)
- [Sendgrid](https://github.com/spatie/laravel-mailcoach-sendgrid-feedback)
- [Mailgun](https://github.com/spatie/laravel-mailcoach-mailgun-feedback)
- [Postmark](https://github.com/spatie/laravel-mailcoach-postmark-feedback)

## Manually handling feedback

You can also manually handle feedback.

First you must add a transport id to a `Send` model. Here's an example listener: 

```php
class StoreTransportMessageId
{
    public function handle(MessageSent $event)
    {
        if (! isset($event->data['send'])) {
            return;
        }

        /** @var \Spatie\Mailcoach\Models\Send $send */
        $send = $event->data['send'];

        $transportMessageId = $event->message->getHeaders()->get('header-name-used-by-your-email-provider')->getFieldBody();

        $send->storeTransportMessageId($transportMessageId);
    }
}
```

You must register that listener. A typical place would be in a service provider.

```php
Event::listen(Illuminate\Mail\Events\MessageSent::class, StoreTransportMessageId::class);
```

Next, in the code that handles feedback you can get to the `MailSend` like this:

```php
$send = Send::findByTransportMessageId($messageId);
```

You can mark a send as bounced.

```php
$send->markAsBounced(SendBounceSeverity::PERMANENT);
```

When a `Send` is marked as permanently bounced, the subscriber will get unsubscribed from the email list.
