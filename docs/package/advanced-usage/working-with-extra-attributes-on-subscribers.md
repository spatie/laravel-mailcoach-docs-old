---
title: Working with extra attributes on subscribers
---

When subscribing you can add extra attributes. Here's an example where `first_name` and `last_name` are added.

```php
$subscription = $emailList->subscribe('john@example.com', [
    'first_name' => 'John',
    'last_name' => 'Doe'
]);
```

Here are examples of methods to work with those extra attributes.

```php
$subscriber = $subscription->subscriber;

// get all extra attributes
$subscriber->extra_attributes->all(); // return an array with all extra attributes;

// updating an extra attibute
$subscriber->extra_attributes->first_name = 'Other name';
$subscriber->save();

// replacing all extra attributes in one go
$subscriber->extra_attributes = ['first_name' => 'Jane', 'last_name' => 'Dane'];
$subscriber->save();

// returns all models with a first_name set to John
$subscriber->withExtraAttributes(['first_name' => 'John'])->get();
```
