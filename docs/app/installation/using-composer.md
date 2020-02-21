---
title: Installation using Composer
---

You can create a new Laravel application with Mailcoach preinstalled into using `Composer`. This application will also have authorization screens (login, password reset) and user management.

### Creating the application

You can create the application with Mailcoach pre-installed using this command

```bash
composer create-project spatie/Mailcoach
```

During the execution of this command Composer will ask for a user and a password. The user is the email address you used when registering at [mailcoach.app](https://mailcoach.app). The password is the key of [a Mailcoach license](/docs/app/general/getting-a-license).

### Creating the database

Next, you must update the values of the `DB_*` entries in `.env` so they match your db. After that run `php artisan migrate` to create all tables.

### Creating the first user

After that you can create an initial user by executing `php artisan make:user`. You can use the created user to login at `/mailcoach`. New user can be made on the users screen in mailcoach.

![Users screen](https://mailcoach.app/images/docs/app/getting-started/users.png)

### Configure the email sending service

Now that you are logged in you must configure the email sending service you'd like to use. Here are set up instructions for

- [Amazon SES](/docs/app/mail-configuration/amazon-ses)
- [SendGrid](/docs/app/mail-configuration/sendgrid)
- [Mailgun](/docs/app/mail-configuration/mailgun)

### Running Horizon

This package handles various tasks in a queued way via [Laravel Horizon](https://laravel.com/docs/master/horizon). The `horizon.php` config is already preconfigured. You only need to [make sure that Horizon runs](https://laravel.com/docs/master/horizon#running-horizon).

### Running The Scheduler
This package relies on the laravel scheduler, be sure to run `php artisan schedule:run` to send out any scheduled campaigns. 

### Making sure everything works

Before sending out a real campaign, we highly recommend creating a small email list with a couple of test email addresses and send a campaign to it. This way, you can verify that sending mails, and the open & click tracking are all working correctly.
