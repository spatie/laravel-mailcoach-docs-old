---
title: Subscribing to a list
---

You can use a string to subscribe to an email list.

```php
$emailList->subscribe('john@example.com');
```

Behind the scenes, we'll create a `Subscriber` with email `john@example.com` and a `Subscription`, which is the relation between a subscriber and a list.

If you try to subscribe an email that already exists on a list it will be ignored.

You can also use a `Subscriber` to subscribe to an email list.

```php
$subscriber = Subscriber::findForEmail('john@example.com');

$subscriber->subscribeTo($anotherEmailList);
```

## Specifying first name and last name

There are attributes named `first_name` and `last_name` on the `Subscriber` model itself. When subscribing you can fill them by passing a second argument to `subscribe`.

```php
$subscription = $emailList->subscribe('john@example.com', ['first_name' => 'John', 'last_name' => 'Doe']);

$subscription->subscriber->first_name; // returns 'John'
$subscription->subscriber->last_name; // returns 'Doe'
```

## Adding regular attributes

If you need more attributes on a subscriber you can create [a migration](https://laravel.com/docs/master/migrations) that adds a field.

```php
Schema::table('email_list_subscribers', function (Blueprint $table) {
    $table->string('job_title')->nullable();
});
```

To fill the field just add a key to the second array passed to `subscribe`.

```php
$subscription = $emailList->subscribe('john@example.com', ['job_title' => 'Developer']);

$subscription->subscriber->job_title; // returns 'Developer'
```

## Adding extra attributes

The `email_list_subscribers` table has an json field called `extra_attributes`. You can use this field to add unstructured data to a subscriber.

When subscribing pass all the data to the third argument of `subscribe`. This data will be saved in `extra_attributes`.

```php
$emailList->subscribe('john@example.com', [],  [
    'key 1' => 'value 1',
    'key 2' => 'value 2'
]);

$subscriber = Subscriber::findForEmail('john@example.com');

$subscriber->extra_attributes->get('key 1'); // returns 'value 1';
$subscriber->extra_attributes->get('key 2'); // returns 'value 2';
```

You can read more on extra attributes in [this section of the docs](https://docs.spatie.be/laravel-email-campaigns/v1/advanced-usage/working-with-extra-attributes-on-subscribers/).

## Checking if someone is subscribed

You can check if a given email is subscribed to an email list.

```php
$emailList->isSubscribed('john@example.com'); // returns a boolean
```

You can use a subscriber to check to if it is subscriber to a list.

```php
$subscriber->isSubscribedTo($emailList) // returns a boolean;
```

## Getting all list subscribers

To get all subscribers of an email list you can use the `emailList` you can call `subscribers` on an email list.

```php
$subscribers = $emailList->subscribers; // returns all subscriber
```

To get the email address of a subscriber call `email` on a subscriber.

```php
$email = $subscribers->first()->email;
```

Calling `subscribers` on an email list will only return subscribers that have a subscription with a `subscribed` status. Subscribers that have unsubscribed or are still pending (when using [double opt in](https://docs.spatie.be/laravel-email-campaigns/v1/working-with-lists/using-double-opt-in/)) will not be returned.

To return all subscribers, including all pending and unsubscribed ones, use `allSubscribers`.

```php
$allSubscribers = $emailList->allSubscribers;
```

## Skipping opt in when subscribing

If [double opt-in](https://docs.spatie.be/laravel-email-campaigns/v1/working-with-lists/using-double-opt-in/) is enabled on a list, then `subscribeTo` won't result in an immediate subscription. Instead, the user must first confirm, by clicking a link in a mail, before their subscription to the new list is completed.

To immediately confirm someone, and skipping sending the confirmation mail, use `subscribeNow`:

```php
$emailList->subscribeNow('john@example.com');

// or using an existing subscriber
$subscriber->subscribeNowTo($anotherEmailList);
```
