---
title: Sending a campaign
---

Before sending a campaign, ensure that the `subject`, `HTML` and `email_list_id` attributes are set.

A campaign can be sent with the `send` method.

```php
$campaign->send();
```

Alternatively you can set the email list and send the campaign in one go:

```php
$campaign->sendTo($emailList);
```

If you don't want to send email to your entire list, but only to a subset of subscribers, you can [use a segment](https://docs.spatie.be/laravel-mailcoach/v1/advanced-usage/segmenting-lists/).

## What happens when a campaign is being sent

When you send a campaign, a job called `SendCampaign` job will be dispatched. This job will create a `MailSend` model for each of the subscribers of the list you're sending the campaign to. A `MailSend` represents a mail that should be sent to one subscriber.

For each created `SendMail` model, a `SendMailJob` will be started. That job will send that actual mail. After the job has sent the mail, it will mark the `SendMail` as sent, by filling `sent_at` with the current timestamp.

 You can customize on which queue the `SendCampaignJob` and `SendMailJob` jobs are dispatched in the `perform_on_queue` in the `email-campaigns` config file. We recommend the `SendMailJob` having its own queue because it could contain many pending jobs if you are sending a campaign to a large list of subscribers.

The `SendMailJob` is throttled by default so that it doesn't overwhelm your email service with a large number of calls. Only 5 of those jobs will be handled per second. To learn more about this, read [the docs on throttling sends](https://docs.spatie.be/laravel-mailcoach/v1/advanced-usage/throttling-sends/).
