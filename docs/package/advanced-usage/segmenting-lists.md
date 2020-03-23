---
title: Segmenting lists
---

If you wish to send a campaign to only a part of an email list you can use a segment when sending your campaign. A segment is a class that is responsible for selecting subscribers on an email list. It should always extend `Spatie\Mailcoach\Support\Segments\Segment`

## A first example

Here's a silly segment that will only select subscriber whose email address begin with an 'a'

```php
class OnlyEmailAddressesStartingWithA extends Segment
{
    public function shouldSend(Subscriber $subscriber): bool
    {
        return Str::startsWith($subscriber->email, 'a');
    }
}
```

When sending a campaign this is how the segment can be used:

```php
Campaign::create()
   ->content($yourHtml)
   ->segment(OnlyEmailAddressesStartingWithA::class)
   ->sendTo($emailList);
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

    public function shouldSend(Subscriber $subscriber): bool
    {
        return Str::startsWith($subscriber->email, $this->character);
    }
}
```

When sending a campaign this is how the segment can be used:

```php
Campaign::create()
   ->content($yourHtml)
   ->segment(new OnlyEmailAddressesStartingWith('b'))
   ->sendTo($emailList);
```

The object will be serialized when saved to the campaign, and unserialized when used for segmenting.

## Using a query

If you have a very large list, it might be better to use a query to select the subscribers of your segment. This can be done with the `subscribersQuery` method on a segment.

Here's an example:

```php
class OnlyEmailAddressesStartingWithA extends Segment
{
    public function subscribersQuery(Builder $subscribersQuery): void
    {
        $subscribersQuery->where('email','like', 'a%');
    }
}
```

No matter what you do in `subscribersQuery`, the package will never mail people that haven't subscribed to the email list you're sending the campaign to.

## Segment description

`Spatie\Mailcoach\Support\Segments\Segment` allows us to give our custom segment a unique name. This is required by the interface and can be done very simple:

```php
    public function description(): string
    {
        return 'My cool segment';
    }
```

## Accessing the Campaign model

If you need to get any `campaign` details somewhere in your segment logic, you can use `$this->campaign` to access the model object of the campaign that is being sent.
