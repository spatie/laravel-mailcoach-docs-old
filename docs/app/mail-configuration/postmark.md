---
title: Mailgun
---

## Configuring Mailgun

First off, make sure you have created an account with [Postmark](https://postmarkapp.com). 

At this moment, sending bulk email with Postmark is in beta. Before using Mailcoach to send out mails with Postmark, contact their support to enable this on your account.

## Configuring Postmark

At Postmark you must [configure a new webhook](https://postmarkapp.com/support/article/1067-how-do-i-enable-delivery-webhooks).

At the webhooks settings screen, on Postmark, you must add the `Open`, `Bounce`, `Spam Complaint` and `Link Click` webhooks and point them to the route you configured. 

The webhook should be sent to `https://<your-domain>/postmark-feedback`

## Configuring Mailcoach

Mailcoach still needs to know where to send its emails, so go to _Overview_ page in your Mailgun dashboard, select the _API_ option, then _cURL_ and take note of the _API key_ and _API base URL_ that are now visible:

![](https://mailcoach.app/images/docs/app/mail-configuration/mailgun-api-key.png)

Go to your _Mail configuration_ page in Mailcoach, and fill in the fields:

![](https://mailcoach.app/images/docs/app/mail-configuration/mailgun-setup-mail-config.png)

- Mails per second: to not overwhelm Postmark with mail requests, send this to a sensible value. In many cases `10` will be sufficient
- Server token: you can get the right value on the [Postmark account details screen](https://account.postmarkapp.com/account/edit)
- SMTP user
- SMTP password
