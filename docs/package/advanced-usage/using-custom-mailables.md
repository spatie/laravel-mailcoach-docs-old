---
title: Using custom mailables
---

You can use your own [mailable](https://laravel.com/docs/7.x/mail#writing-mailables) to be used when sending a campaign. Any mailable that extends `Spatie\Mailcoach\Mails\CampaignMail` is valid.

Here's an example mailable;

```php
use Spatie\Mailcoach\Mails\CampaignMail;

class MyCustomMailable extends CampaignMail
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
