---
title: Creating a list
weight: 1
---

An email list is used to group a collection of subscribers.

You can create a new email list by newing up an `EmailList`. The only required field is `name`.

```php
$emailList = EmailList::create(['name' => 'my email list name']);
```
