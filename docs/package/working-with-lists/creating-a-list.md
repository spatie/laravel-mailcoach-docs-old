---
title: Creating a list
---

An email list is used to group a collection of subscribers.

You can create a new email list by newing up an `EmailList`. The only required field is `name`.

```php
$emailList = EmailList::create(['name' => 'my email list name']);
```

### Setting a default senders

You can set a default email address and from name.

```php
$emailList->default_from_email = 'john@example.com';
$emailList->default_from_name = 'John Doe';

$emailList->save();
```

When sending a campaign to this email list these defaults will be used if you send a campaign that doesn't have a sender of its own.
