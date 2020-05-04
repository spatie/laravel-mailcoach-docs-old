---
title: Welcome
---

![](https://mailcoach.app/images/docs/v2/package/welcome.png)

This package allows you to send out email campaigns to a list of subscribers effortlessly. It also provides a UI to manage campaigns, email lists and subscribers.

Your currently looking at the documentation of v2 which requires Laravel 7. If you are on Laravel 6, take a look at [the docs of Mailcoach v1](/docs/v1/app/general/welcome).

Let’s take a quick look at how to use the package. First, create an email list:

```php
$emailList = EmailList::create(['name' => 'newsletter subscribers']);
```

Next, we subscribe people to a list. There's also support for [double opt-in subscriptions](/docs/v2/package/working-with-lists/using-double-opt-in).

```php
$emailList->subscribe('john@example.com');
$emailList->subscribe('paul@example.com');
```

You can send an email to all those subscribed to the list.

```php
Campaign::create()
    ->subject('test')
    ->content($html)
    ->trackOpens()
    ->trackClicks()
    ->sendTo($emailList);
```

After a campaign is sent, [interesting statistics](/docs/v2/package/working-with-campaigns/viewing-statistics-of-a-sent-campaign) are made available.

