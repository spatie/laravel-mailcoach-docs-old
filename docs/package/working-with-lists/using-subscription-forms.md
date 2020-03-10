---
title: Using subscription forms
---

You can accept email list subscriptions coming from external sites by adding a subscription form to that site.

In order to accept incoming form subscriptions you must set  `allow_form_subscriptions` to true.

```php
$emailList->update(['allow_form_subscriptions' => true]);
```

Here's an example form you can embed on an external site to accept subscriptions.

```html
<form method="POST" action="https://domain-where-mailcoach-runs/mailcoach/subscribe/<uuid-of-emaillist>">
    <div>
        <label for="email">Email</label>
        <input name="email">
    </div>
   <!-- optionally you can include the first_name and last_name field
    <div>
        <label for="first_name">First name</label>
        <input name="first_name">
    </div>
    <div>
        <label for="last_name">Last name</label>
        <input name="last_name">
    </div>
    -->
    <input type="hidden" name="redirect_after_subscribed" value="https://your-site/subscribed"  />
    <input type="hidden" name="redirect_after_already_subscribed" value="https://your-site/already-subscribed"  />
   <!-- only required if your list has double opt-in enabled
    <input type="hidden" name="redirect_after_subscription_pending" value="https://your-site/redirect-after-pending"  />
    -->
    <div>
       <button type="submit">Subscribe</button>    
    </div>
</form>
```

## Attaching tags

You can specify one or more tags in an input field named `tags` that should be attached to the subscriber when it gets created.

```html
<!-- somewhere in your form -->
<input type="hidden" name="tags" value="tagA;tagB">
```

Only tags that were added on the `allowedFormSubscriptionTags` relation of the email list you're subscribing someone to will be attached. Here's you can attach tags to that relation.

```php
$tagA = $emailList->tags()->where('name', 'tagA')->first();
$tagB = $emailList->tags()->where('name', 'tagB')->first();

$emailList->allowedFormSubscriptionTags([$tagA->id, $tagB->id]);
```

## A word to the wise

We highly recommend that you turn on [confirmation](/docs/v1/package/working-with-lists/using-double-opt-in) for email lists that allow form subscriptions. This will keep your list healthy. Also consider adding a honeypot to the form to avoid bots from trying to subscribe. When using Laravel, you could opt to use [the spatie/laravel-honeypot package](https://github.com/spatie/laravel-honeypot).
