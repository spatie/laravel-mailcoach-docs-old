---
title: Postmark
---

## Configuring Postmark

First off, make sure you have created an account with [Postmark](https://postmarkapp.com). 

At this moment, sending bulk email with Postmark is in beta. Before using Mailcoach to send out mails with Postmark, contact their support to enable this on your account.

## Configuring Postmark

At Postmark you must [configure a new webhook](https://postmarkapp.com/support/article/1067-how-do-i-enable-delivery-webhooks).

The webhook should be sent to `https://<your-domain>/postmark-feedback`

You should add a custom header named `mailcoach-signature`, and you can choose a value that you should keep secret. You must turn on the `Open`, `Bounce`, `Spam Complaint` and `Link Click`.

![](https://mailcoach.app/images/docs/v3/package/postmark/postmark-webhooks.png)

On the settings screen in Postmark, you should also enable open and link tracking.

![](https://mailcoach.app/images/docs/v3/package/postmark/postmark-tracking.png)

## Configuring Mailcoach

On the Mail configuration settings screen in Mailcoach, you'll have to have to fill in these settings.

- Mails per second: to not overwhelm Postmark with mail requests, send this to a sensible value. In many cases `10` will be sufficient
- Server token: you can get the right value on the [Postmark account details screen](https://account.postmarkapp.com/account/edit)
- Signing secret: this should be set to the value you specified for the `mailcoach-signature` header.
- Message stream (optional): the [Postmark Broadcast](https://postmarkapp.com/message-streams) message stream

## Informing Postmark 

Using Postmark for bulk mail is currently in beta. Before start sending campaigns via Postmark we highly recommend getting in touch with their support and let them know you want to start sending bulk emails.

