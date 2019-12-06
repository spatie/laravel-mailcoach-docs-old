---
title: Manually handling feedback
---

For every mail sent you must store a transport id to a `Send` model. Here's an example listener: 

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

Here are the things you can perform on a `Send`:

```php
$send->registerOpen();
$send->registerClick($url);
$send->registerBounce();
$send->registerComplaint();
```

When calling `registerBounce` or `registerComplaint`, the subscriber will get unsubscribed from the email list.
