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

When you are adding subscribers from an external form, you can also add extra attributes by using a specific input name pattern `attributes['name-of-your-new-attribute']`. This is what that looks like for storing a user's job.

```html
<input type="text" name="attributes[job]" value="developer">
```

To make this work, you also have to make sure `job` is defined as an allowed extra field on your email list's field `allowed_form_extra_attributes`.

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
