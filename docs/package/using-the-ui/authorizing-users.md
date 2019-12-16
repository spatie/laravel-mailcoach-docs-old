---
title: Authorizing users
---

You can determine which users of your application are allowed to view the mailcoach UI by defining a gate check called `viewMailcoach`.

A common place to register this check is in a service provider. 

Here's an example where we assume that administrators of your application have a `admin` attribute that is set to `true`. This gate definition will only allow administrators to use the Mailcoach UI.

```php
// in a service provider

public function boot()
{
   Illuminate\Support\Facades\Gate::define('viewMailcoach', function ($user = null) {
       return optional($user)->admin;
   });
}
```
