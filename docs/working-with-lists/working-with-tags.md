---
title: Working with tags
---

A mailinglist can have one or more tags. These tags can be applied onto the subscribers of that list.

## Creating and attaching tags

Here's how you can create a tag.

```
$subscriber->addTag('tagA');
``` 

The tag named `tagA` will be created an associated with the email list of the subscriber and with the subscriber itself.

Tags can be created and associated in one go:

```php
$subscriber->addTags(['tagB', 'tagC']);
```

## Detaching tags

Tags can be detached.

```php
$subscriber->removeTag('tagA');
$subscriber->removeTags(['tagA', 'tagB']);
```

Detaching a tags will only remove it from a subscriber. It will not be removed from the email list.

## Deleting tags

To delete a certain tag from an email list, just call `delete` on it.

```php
$emailList->tags()->where('name', 'tagA')->first()->delete();
```

## Syncing tags

You can pass multiple tags names to `syncTags`. Tags that do not exist yet will be created. Tags that are already attached but not present in the array passed to `syncTags` will be detached from the subscriber.

```php
$subscriber->syncTags(['tagA', 'tagC']);
```