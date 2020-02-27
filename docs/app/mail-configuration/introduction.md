---
title: Introduction
---

Mailcoach can't send out emails by itself. To do this, Mailcoach needs a mail configuration with a third party email delivery platform. We support these services:

- [Amazon SES](/docs/app/mail-configuration/amazon-ses)
- [SendGrid](/docs/app/mail-configuration/sendgrid)
- [Mailgun](/docs/app/mail-configuration/mailgun)
- [Postmark](/docs/app/mail-configuration/postmark)

We also support sending via SMTP. When you select this driver, open and click tracking will not be available in your campaigns.

## Sending test mails

To make sure Mailcoach is happy with your mail configuration, send a test mail to your own email address. It should arrive in your mailbox not long after. If this test mail ends up in your spam, there may be something wrong with your domain or DNS settings.

![](https://mailcoach.app/images/docs/app/mail-configuration/successful-test-mail.png)
