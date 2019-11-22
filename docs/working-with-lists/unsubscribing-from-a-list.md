---
title: Unsubscribing from a list
---

You can add an unsubscribe link by [adding an `::unsubscribeUrl::` placeholder](https://docs.spatie.be/laravel-mailcoach/v1/working-with-campaigns/creating-a-campaign/#setting-the-content-and-using-placeholders) to the HTML of your campaign.

When a subscriber visits the actual unsubscribe URL, a simple message will be displayed to confirming that they have  successfully unsubscribed.

To customize this confirmation message, publish the views.

```php
php artisan vendor:publish --provider="Spatie\Mailcoach\MailcoachServiceProvider" --tag="views"
```

Now we modify the following views in the `/resources/views/vendor/mailcoach/unsubscribe/` directory:

- `unsubscribed.blade.php`
- `notFound.blade.php`

## Unsubscribing manually

You can also unsubscribe someone manually like this.

```php
$emailList->unsubscribe('john@example.com');
```

Alternatively, you can call unsubscribe on a subscriber

```php
Subscriber::findForEmail('john@example.com')->unsubscribe();
```

## Unsubscribing using an email client

Emails sent have the ```List-Unsubscribe``` header included. This allows for users to unsubscribe from their email client as per [RFC2369](https://www.ietf.org/rfc/rfc2369.txt).

## Permanently deleting a subscriber

Behind the scenes, the subscriber and the subscription will not be deleted. Instead, the status of the subscription will be updated to `unsubscribed`.
If you want to delete a subscription outright, you can call `delete` on it.

```php
$emailList->getSubscription('john@example.com')->delete();
```

If you want to delete a subscriber entirely, you can call `delete` on it.

```php
Subscriber::findForEmail('john@example.com')->delete();
```

The code above will also delete all related subscriptions.
