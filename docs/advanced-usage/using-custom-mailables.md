---
title: Using custom mailables
weight: 7
---

You can use your own [mailable](https://laravel.com/docs/master/mail#writing-mailables) to be used when sending a campaign. Any mailable that extends `Spatie\MailCoach\Mails\CampaignMailable` is valid.

Here's an example mailable;

```php
use Spatie\MailCoach\Mails\CampaignMailable;

class MyCustomMailable extends CampaignMailable
{
    public function build()
    {
        return $this->view('emails.your-custom-view');
    }
}
```

You can use it in a campaign like this:

```php
Campaign::create()
    ->useMailable(MyCustomMailable::class)
    ->sendTo($emailList);
```
