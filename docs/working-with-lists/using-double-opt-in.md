---
title: Using double opt-in
---

To ensure that all subscribers of your email list really wanted to subscribe, you can enable the double opt-in requirement.

```php
EmailList::create([
    'name' => 'My list'
    'requires_double_opt_in' => true,
]);
```

When calling `subscribe` on a list where `requires_double_opt_in` is enabled, a subscription will be created with a `status` set to `pending`. An email will be sent to the email address you're subscribing. The email contains a link that, when clicked, will confirm the subscription. When a subscription is confirmed, its status will be set to `subscribed`.

When sending a campaign to an email list, only subscribers that have a subscription with status `subscribed` will receive the campaign.

To immediately subscribe someone, and skip sending a confirmation email, you can call `subscribeNow` on a list.

## Customizing the subscription confirmation response

When a person clicks the email confirmation link, a simple message explaining the result of confirmation is displayed. You can customize that response by publishing the views.

```php
php artisan vendor:publish --provider="Spatie\Mailcoach\MailcoachServiceProvider" --tag="views"
```

The responses of a confirmation can now be modified in the view inside the `/resources/views/vendor/mailcoach/confirmSubscription/` directory. It will contain these blade files:

- `confirmed.blade.php`
- `couldNotFindSubscription.blade.php`
- `wasAlreadyConfirmed.blade.php`

## Customizing the double opt in mail

You can customize the content of the double opt-in mail.

First, you must publish the views.

```bash
php artisan vendor:publish --provider="Spatie\Mailcoach\MailcoachServiceProvider" --tag="views"
```

After that, the content of the double opt-in mail can be modified in the `/resources/views/vendor/mailcoach/mails/confirmSubscription.blade.php` view.
