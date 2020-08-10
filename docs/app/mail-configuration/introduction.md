---
title: Introduction
---

Mailcoach sends out mail using a third party email delivery platform.

- [Amazon SES](/docs/v3/app/mail-configuration/amazon-ses)
- [SendGrid](/docs/v3/app/mail-configuration/sendgrid)
- [Mailgun](/docs/v3/app/mail-configuration/mailgun)
- [Postmark](/docs/v3/app/mail-configuration/postmark)

We also support sending via SMTP. When you select this driver, open and click tracking will not be available in your campaigns.

## Sending test mails

To make sure Mailcoach is happy with your mail configuration, send a test mail to your own email address. It should arrive in your mailbox not long after. If this test mail ends up in your spam, there may be something wrong with your domain or DNS settings.

![screenshot](https://mailcoach.app/images/docs/v3/app/mail-configuration/successful-test-mail.png)

### Sending confirmation and welcome mails with a different account

Some email providers have very strict rules on sending mails. They require to keep a low bounce rate at all times. Confirmation mails have a higher chance of bouncing because they are sent to unverified email addresses.

To keep your primary email service happy, you can opt to use a different account to send out confirmation and welcome mails. To do so, configure a transactional mailer in the "Transaction mail" section of the settings.

![screenshot](https://mailcoach.app/images/docs/v3/app/mail-configuration/transactional.png)
