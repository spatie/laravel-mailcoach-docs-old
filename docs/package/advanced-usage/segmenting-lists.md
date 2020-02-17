---
title: Segmenting lists
---

If you wish to send a campaign to only a part of an email list you can use a segment when sending your campaign. A segment is a class that is responsible for selecting subscribers on an email list. It should always extend `Spatie\Mailcoach\Support\Segments\Segment`

## A first example

Here's a silly segment that will only select subscriber whose email address begin with an 'a'

```php
class OnlyEmailAddressesStartingWithA extends Segment
{
    public function shouldSend(Subscription $subscription, Campaign $campaign): bool
    {
        return Str::startsWith($subscription->email, 'a');
    }
}
```

When sending a campaign this is how the segment can be used:

```php
Campaign::create()
   ->html($yourHtml)
   ->useSegment(OnlyEmailAddressesStartingWithA::class)
   ->sentTo($emailList);
```

## Using an instantiated Segment object

> **Since 1.7.1**
> make sure the `segment_class` field in your `mailcoach_campaigns` table is a `text` field and not a  `varchar`

Here's the same segment that will only select subscriber whose email address begin with a configurable character 'b'

```php
class OnlyEmailAddressesStartingWith extends Segment
{
    public string $character;

    public function __construct(string $character) {
        $this->character = $character;
    }

    public function shouldSend(Subscription $subscription, Campaign $campaign): bool
    {
        return Str::startsWith($subscription->email, $this->character);
    }
}
```

When sending a campaign this is how the segment can be used:

```php
Campaign::create()
   ->html($yourHtml)
   ->useSegment(new OnlyEmailAddressesStartingWith('b'))
   ->sentTo($emailList);
```

The object will be serialized when saved to the campaign, and unserialized when used for segmenting.

## Using a query

If you have a very large list, it might be better to use a query to select the subscribers of your segment. This can be done with the `getSubscriptionsQuery` method on a segment.

Here's an example:

```php
class OnlyEmailAddressesStartingWithA extends Segment
{
    public function getSubscriptionsQuery(Subscription $subscription, Campaign $campaign): bool
    {
        return Subscription::query()
            ->where('status', SubscriptionStatus::SUBSCRIBED)
            ->whereHas('subscriber', function(Builder $query) {
                $query->where('email','like', 'a%');
            })
            ->where('email_list_id', $campaign->emailList->id);
    }
}
```

No matter what you do in `getSubscriptionsQuery`, the package will never mail people that haven't subscribed to the email list you're sending the campaign to.
