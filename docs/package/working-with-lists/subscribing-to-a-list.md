---
title: Subscribing to a list
---

This is the easiest way to subscribe to a list:

```php
$emailList->subscribe('john@example.com');
```

Alternatively you can create a subscriber via the `Subscriber` model.

```
Subscriber::createWithEmail('john@example.com')->subscribeTo($emailList);
```

## Specifying first name and last name

There are attributes named `first_name` and `last_name` on the `Subscriber` model itself. When subscribing you can fill them by passing a second argument to `subscribe`.

```php
$subscriber = $emailList->subscribe('john@example.com', [
    'first_name' => 'John', 
    'last_name' => 'Doe'
]);
```

Alternatively you can create a subscriber with attributes via the `Subscriber` model.

```
Subscriber::createWithEmail('john@example.com')
   ->withAttributes([
       'first_name' => 'John', 
       'last_name' => 'Doe'
   ])
   ->subscribeTo($emailList);
```

## Adding regular attributes

If you need more attributes on a subscriber you can create [a migration](https://laravel.com/docs/master/migrations) that adds a field.

```php
Schema::table('mailcoach_subscribers', function (Blueprint $table) {
    $table->string('job_title')->nullable();
});
```

To fill the field just add a key to the second array passed to `subscribe`.

```php
$subscriber = $emailList->subscribe('john@example.com', ['job_title' => 'Developer']);

$subscriber->job_title; // returns 'Developer'
```

## Adding extra attributes

The `email_list_subscribers` table has an json field called `extra_attributes`. You can use this field to add unstructured data to a subscriber.

When subscribing pass unstructured data as the value of the `extra_attributes` key . This data will be saved in `extra_attributes`.

```php
$subscriber = $emailList->subscribe('john@example.com', [
    'extra_attributes' => [
        'key 1' => 'value 1',
        'key 2' => 'value 2',
    ],
]);

$subscriber->extra_attributes->get('key 1'); // returns 'value 1';
$subscriber->extra_attributes->get('key 2'); // returns 'value 2';
```

You can read more on extra attributes in [this section of the docs](/docs/package/advanced-usage/working-with-extra-attributes-on-subscribers).

## Checking if someone is subscribed

You can check if a given email is subscribed to an email list.

```php
$emailList->isSubscribed('john@example.com'); // returns a boolean
```

You can use a subscriber to check to if it is subscriber to a list.

```php
$subscriber->isSubscribedTo($emailList) // returns a boolean;
```

## Getting the status of a subscriber

To get the status of a subscriber

```php
$subscriber->status;
```

This property can contain three possible values:
- `unconfirmed`: when the list uses [double opt in](/docs/package/working-with-lists/using-double-opt-in) and the confirmation link wasn't clicked yet
- `subscribed`: when the subscriber is subscribed.
- `unsubscribed`: when the subscriber was unsubscribed.

## Getting all list subscribers

To get all subscribers of an email list you can use the `emailList` you can call `subscribers` on an email list.

```php
$subscribers = $emailList->subscribers; // returns all subscribers
```

To get the email address of a subscriber call `email` on a subscriber.

```php
$email = $subscribers->first()->email;
```

Calling `subscribers` on an email list will only return subscribers that have a subscription with a `subscribed` status. Subscribers that have unsubscribed or are still unconfirmed (when using [double opt in](/docs/package/working-with-lists/using-double-opt-in)) will not be returned.

To return all subscribers, including all unconfirmed and unsubscribed ones, use `allSubscribers`.

```php
$allSubscribers = $emailList->allSubscribers;
```

## Skipping opt in when subscribing

If [double opt-in](/docs/package/working-with-lists/using-double-opt-in) is enabled on a list, then `subscribeTo` won't result in an immediate subscription. Instead, the user must first confirm, by clicking a link in a mail, before their subscription to the new list is completed.

To immediately confirm someone, and skipping sending the confirmation mail, use `subscribeSkippingConfirmation`:

```php
$emailList->subscribeskippingConfirmation('john@example.com');
```

Alternatively you can use this syntax:

```php
Subscriber::createWithEmail('john@example.com')
   ->skipConfirmation()
   ->subscribeTo($emailList);
```

## Skipping sending a welcome mail

If your list is configured to send a welcome mail, you can skip sending a welcome mail for a particular subscriber.

```php
Subscriber::createWithEmail('john@example.com')
   ->doNotSendWelcomeMail()
   ->subscribeTo($emailList);
```