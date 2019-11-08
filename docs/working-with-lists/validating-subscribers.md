---
title: Validating subscribers
weight: 45
---

When integrating this package into your app you will likely build a UI where people can subscribe to your list. This package provides a validation rule that verifies that given email address isn't already on the given email list. You can use it like this:

```php
// in a form request

public function rules() {
   $emailList = EmailList::first();

   return [
      'email' => ['email', new Spatie\MailCoach\Rules\EmailListSubscriptionRule($emailList)]
   ];
}
```

You can customize the validation error message publishing the lang files.

```php
php artisan vendor:publish --provider="Spatie\MailCoach\MailCoachServiceProvider" --tag="lang"
```

You'll find the message in `resources/lang/en/messages.php`.
