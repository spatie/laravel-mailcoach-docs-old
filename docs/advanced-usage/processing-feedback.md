---
title: Processing feedback
weight: 3
---

After a mail is sent, most email providers send feedback on the result.

You can handle that feedback for a certain provider by installing one of the add-on package we provide:
- [Mailgun](https://github.com/spatie/laravel-email-campaigns-mailgun-feedback)
- [SES](https://github.com/spatie/laravel-email-campaigns-ses-feedback)

## Manually handling feedback

You can also manually handle feedback.

First you must add a transport id to a `CampaignSend` model. Here's an example listener: 

```php
class StoreTransportMessageId
{
    public function handle(MessageSent $event)
    {
        if (! isset($event->data['campaignSend'])) {
            return;
        }

        /** @var \Spatie\EmailCampaigns\Models\CampaignSend $campaignSend */
        $campaignSend = $event->data['campaignSend'];

        $transportMessageId = $event->message->getHeaders()->get('header-name-used-by-your-email-provider')->getFieldBody();

        $campaignSend->storeTransportMessageId($transportMessageId);
    }
}
```

You must register that listener. A typical place would be in a service provider.

```php
Event::listen(Illuminate\Mail\Events\MessageSent::class, StoreTransportMessageId::class);
```

Next, in the code that handles feedback you can get to the `MailSend` like this:

```php
$campaignSend = CampaignSend::findByTransportMessageId($messageId);
```

You can mark a campaignSend as bounced.

```php
$campaignSend->markAsBounced(CampaignSendBounceSeverity::PERMANENT);
```

When a `CampaignSend` is marked as permanently bounced, the subscriber will get unsubscribed from the email list.
