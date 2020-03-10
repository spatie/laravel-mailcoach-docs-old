---
title: Creating a campaign
---

To send an email to all subscribers of your list, you must create a campaign.

A campaign can be created like this:

```php
Campaign::create()
    ->from('sender@example.com')
    ->subject('My newsletter #1')
    ->content($html)
    ->trackOpens()
    ->trackClicks()
    ->to($emailList);
```

The `trackOpens` and `trackClicks` calls are optional.

Alternatively, you could manually set the attributes on a `Campaign` model.

```php
Campaign::create([
   'from_email' => 'sender@example.com',
   'subject' => 'My newsletter #1',
   'content' => $html,
   'track_opens' => true,
   'track_clicks' => true,
   'email_list_id' => $emailList->id,
]);
```

## Setting the content and using placeholders

You can set the content of a campaign by setting it's `HTML` attribute.

```php
$campaign->html = $yourHtml;
$campaign->save();
```

In that HTML you can use these placeholders which will be replaced when sending out the campaign:

- `::unsubscribeUrl::`: this string will be replaced with the URL that, when hit, will immediately unsubscribe the person that clicked it
- `::webviewUrl`: this string will be replaced with the URL that will display the content of your campaign. Learn more about this in [the docs on webviews](/docs/v2/package/advanced-usage/displaying-webviews).

If there is no way for a subscriber to unsubscribe, it will result in a lot of frustration on the part of the subscriber. We recommend always to use the `::unsubscribeUrl::` in the HTML of each campaign you send.

## Setting a from name

To set a from name, just pass the name as a second parameter to `from`

```php
Campaign::create()->from('sender@example.com', 'Sender name')
```
