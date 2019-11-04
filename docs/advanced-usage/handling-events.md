---
title: Handling events
weight: 5
---

This package fires several events. You can listen for them firing to perform extra logic.

## `Spatie\EmailCampaigns\Events\Subscribed`

This event will be fired as soon as someone subscribes. If [double opt-in](https://docs.spatie.be/laravel-email-campaigns/v1/working-with-lists/using-double-opt-in/) is enabled on the email list someone is in the process of subscribing to, this event will be fired when the subscription is confirmed.

The event has one public property: `$subscription` which is an instance of `Spatie\EmailCampaigns\Models\Subscription`.

You can get the email address of the subscriber like this:

```php
$email = $event->subscription->subcriber->email;
```

This is how to get the name of the email list:

```php
$nameOfEmailList = $event->subscription->emailList->name;
```

## `Spatie\EmailCampaigns\Events\Unsubscribed`

This event will be fired as soon as someone unsubscribes. When somebody unsubscribes, the subscription won't be deleted, but its `status` attribute will be set to `unsubscribed`.

The event has two public properties:
- `$subscription` which is an instance of `Spatie\EmailCampaigns\Models\Subscription`.
- `$campaignSend` an instance of `Spatie\EmailCampaigns\Models\CampaignSend`. You can use this to determine in which campaign the unsubscribe occurred: `$campaignSend->campaign`. This properties can also be `null`.

You can get the email address of the subscriber like this:

```php
$email = $event->subscription->subcriber->email;
```

This is how to get the name of the email list:

```php
$nameOfEmailList = $event->subscription->emailList->name;
```

## `Spatie\EmailCampaigns\Events\CampaignSent`

This event will be fired after you've sent a campaign, and all mails are queued to be sent. Keep in mind that not all your subscribers will have gotten your mail when this event is fired.

The event has one public property: `$campaign`, which is an instance of the `\Spatie\EmailCampaigns\Models\Campaign` model.

## `Spatie\EmailCampaigns\Events\CampaignMailSent`

This event will be fired when a mail has actually been sent to a single subscriber.

The event has one public property: `$campaignSend` which is an instance of the `Spatie\EmailCampaigns\Models\CampaignSend` model.

You can get the email the mail was sent to like this:

```php
$email = $event->campaignSend->subscription->subscriber->email;
```

You can also retrieve the name of the campaing that this mail was part of:

```php
$campaignName = $event->campaignSend->campaign->name;
```

## `Spatie\EmailCampaigns\Events\CampaignOpened`

This event will be fired when somebody opens an email. Be aware that this event could be fired many times after sending a campaign to a email list with a large number of subscribers. This event will only be fired for campaigns that have [open tracking](https://docs.spatie.be/laravel-email-campaigns/v1/working-with-campaigns/tracking-opens/) enabled.

It has one public property: `$campaignOpen`, which is an instance of the `Spatie\EmailCampaigns\Models\CampaignOpen` model.

You can get the email address that opened your email like this:

```php
$email = $event->campaignOpen->subscriber->email;
```

This is how you can get to the name of the campaign:

```php
$name = $event->campaignOpen->campaign->name;
```

## `Spatie\EmailCampaigns\Events\CampaignLinkClicked`

This event will be fired when somebody clicks a link in a mail. This event will only be fired for campaigns that have [click tracking](https://docs.spatie.be/laravel-email-campaigns/v1/working-with-campaigns/tracking-clicks/) enabled.

It has one public property `$campaignClick`, which is an instance of the `Spatie\EmailCampaigns\Models\CampaignClick` model.

You can get to the url of the link clicked like this:

```php
$url = $event->campaignClick->link->original_link;
```

The email address of the subscriber who clicked the link can be retrieved like this:

```php
$email = $event->campaignClick->subscriber->email;
```

## `Spatie\EmailCampaigns\Events\CampaignStatisticsCalculated`

After a campaign has been sent, statistics will [be calculated according to a schedule](https://docs.spatie.be/laravel-email-campaigns/v1/working-with-campaigns/viewing-statistics-of-a-sent-campaign/).

Each time the statistics are calculated this event will be fired. It has one public property `$campaign`, which is an instance of the `Spatie\EmailCampaigns\Models\Campaign` model.
