---
title: Creating a list
---

An email list is used to group a collection of subscribers.

You can create a new email list by newing up an `EmailList`. The only required field is `name`.

```php
$emailList = EmailList::create(['name' => 'my email list name']);
```

### Setting a default sender

You can set a default email address and from name.

```php
$emailList->update([
    'default_from_email' => 'john@example.com',
    'default_from_name' => 'John Doe',
]);
```

When sending a campaign to this email list these defaults will be used if you send a campaign that doesn't have a sender of its own.

### Specifing the mailers to be used

By default, Mailcoach sends all mails using the default mailer of your Laravel app. In the `mailcoach.php` config file, [you can override this](https://mailcoach.app/docs/v2/package/general/installation-and-setup#configure-an-email-sending-service).

At the time of creating your EmailList, the `transactional_mailer` and `campaign_mailer` attributes will be set based on your configuration settings. Any future change in your configuration settings will not automatically apply to the EmailList.

You can also override the mailer to be used on the list level by updating the `campaign_mailer` attribute with the mailer to be used. Confirmation and welcome mails will be sent using the mailer specified in `transactional_mailer`.

```php
$emailList->update([
    'campaign_mailer' => $nameOfMailer,
    'transactional_mailer' => $nameOfAnotherMailer,
]);
``` 
