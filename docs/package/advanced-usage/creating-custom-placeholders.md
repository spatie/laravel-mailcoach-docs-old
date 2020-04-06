---
title: Creating custom placeholders
---

By default Mailcoach offers a couple of placeholders you can use in the subject or content of your campaign, such as `webviewUrl` and `unsubscribeUrl`.

## Creating a replacer

Custom placeholders can be created. Do this you must create a class and let it implement `Spatie\Mailcoach\Support\Replacers\Replacer` interface.

This interface contains two methods. In `replace` you must do the actual replacement. In `helpText` you must return the helptext that will be visible on the campaign content screen.

Here is the code of the `WebviewReplacer` that ships with Mailcoach.

```php
namespace Spatie\Mailcoach\Support\Replacers;

use Spatie\Mailcoach\Models\Campaign;

class WebviewReplacer implements Replacer
{
    public function helpText(): array
    {
        return [
            'webviewUrl' =>  'This url will display the html of the campaign',
        ];
    }

    public function replace(string $html, Campaign $campaign): string
    {
        $webviewUrl = $campaign->webviewUrl();

        return str_ireplace('::webviewUrl::', $webviewUrl, $campaign->email_html);
    }
}
```

After creating a replacer you must register it in the `replacers` config key of the `mailcoach` config file.

## Creating a personalized replacer

A regular replace will do it's job when the campaign mail is being prepared. This will only happen once when sending a campaign. There's also a second kind of replacer: `Spatie\Mailcoach\Support\Replacers\Replacer\PersonalizedReplacer`. These replacer will get executed for each mail that is being sent out in a campaign. 
`PersonalizedReplacer`s have access to subscriber they are sent to via the `Send` object given in the `replace` method.

Here is the code of the `UnsubscribeUrlReplacer` that ships with Mailcoach.

```php
namespace Spatie\Mailcoach\Support\Replacers;

use Spatie\Mailcoach\Models\Send;

class UnsubscribeUrlReplacer implements PersonalizedReplacer
{
    public function helpText(): array
    {
        return [
            'unsubscribeUrl' => 'The url where users can unsubscribe',
        ];
    }

    public function replace(string $html, Send $pendingSend): string
    {
        $unsubscribeUrl = $pendingSend->subscriber->unsubscribeUrl($pendingSend);

        return str_ireplace('::unsubscribeUrl::', $unsubscribeUrl, $html);
    }
}
```