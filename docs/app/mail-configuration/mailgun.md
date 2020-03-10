---
title: Mailgun
---

## Configuring Mailgun

First off, make sure you have created an account with Mailgun and that you have verified your domain with them. Check [Mailgun's documentation](https://documentation.mailgun.com/en/latest/user_manual.html#verifying-your-domain) on how to do this.

Go to your _Domain settings_ in Mailgun and make sure the _Click tracking_ and _Open tracking_ options are enabled:

![](https://mailcoach.app/images/docs/v2/mailgun-domain-settings.png)

Mailgun needs to know where to send your campaigns' statistics. Go to your Mailcoach _Mail config_ page (in the user menu on the top right), select _Mailgun_ as your driver, and copy the webhook that is mentioned in your Mail configuration:

![](https://mailcoach.app/images/docs/v2/app/mail-configuration/mailgun-copy-webhook.png)

Then, go to the _Webhooks_ submenu in Mailgun, and, with the webhook URL that you copied in the last step, add new webhooks for the following event types: _Clicks_, _Opens_, _Permanent Failure_, and _Spam Complaints_:

![](https://mailcoach.app/images/docs/v2/mailgun-webhooks.png)

For Mailcoach to know the statistics are trustworthy, it needs to know your Mailgun HTTP webhook signing key, which you can find on the same page:

![](https://mailcoach.app/images/docs/v2/app/mail-configuration/mailgun-copy-webhook-signing-key.png)

Copy it, and paste it in the _Webhook signing secret_ field in your mail configuration:

![](https://mailcoach.app/images/docs/v2/app/mail-configuration/mailgun-copy-webhook-signing-key.png)

## Configuring Mailcoach

Mailcoach still needs to know where to send its emails, so go to _Overview_ page in your Mailgun dashboard, select the _API_ option, then _cURL_ and take note of the _API key_ and _API base URL_ that are now visible:

![](https://mailcoach.app/images/docs/v2/app/mail-configuration/mailgun-api-key.png)

Go to your _Mail configuration_ page in Mailcoach, and fill in the fields:

![](https://mailcoach.app/images/docs/v2/app/mail-configuration/mailgun-setup-mail-config.png)

- Mails per second: this will be different according to your Mailgun plan, you can find an appropriate value for this in your Mailgun dashboard
- Domain: the same domain you did the setup for in Mailgun
- Secret: the _API key_ you copied in the last step
- Endpoint: the _API base URL_ that you copied in the last step, but without the `https://` and everything after the top-level domain (`.net` in my case)
