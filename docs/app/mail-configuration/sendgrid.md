---
title: SendGrid
---

To get started, make sure your SendGrid account is completely set up and your domain has been verified. The verification steps are outside of the scope of this tutorial, so we refer you to [SendGrid's docs](https://sendgrid.com/docs/ui/account-and-settings/how-to-set-up-domain-authentication/) to get these steps done. Sending out emails without a verified domain is possible, but not recommended, as the chances of your emails being flagged as spam are very high.

## Email configuration

Setting up your email configuration with SendGrid is very easy. Open your SendGrid dashboard on the [API keys page](https://app.sendgrid.com/settings/api_keys) and press the _Create API Key_ button in the top right corner:

![](https://mailcoach.app/images/docs/app/mail-configuration/sendgrid-api-key-create.png)

Choose a name for your API key, and select the _Resticted Access_ permission. Mailcoach only needs the _Mail Send_ and _Tracking_ permissions:

![](https://mailcoach.app/images/docs/app/mail-configuration/sendgrid-api-key-permissions.png)

Create the API key, and click the text field on the next screen to copy the newly created key. Make sure to save this in a safe spot, like a secure password manager.

![](https://mailcoach.app/images/docs/app/mail-configuration/sendgrid-api-key-copy.png)

Go to your Mailcoach Mail configuration page (under your user menu), make sure you have selected _SendGrid_ as your driver, and paste the API Key we just created there:

![](https://mailcoach.app/images/docs/app/mail-configuration/sendgrid-api-key-in-mailcoach.png)

## Tracking events

If you are not there already, go to your Mailcoach Mail configuration page (under your user menu), make sure your driver is set to _SendGrid_, and choose a value for your _Webhook signing secret_. This should be an unguessable string, but don't use any passwords that you're already using. This secret will be used by Mailcoach to verify incoming events from SendGrid:

![](https://mailcoach.app/images/docs/app/mail-configuration/sendgrid-webhook-signing-key.png)

Also, copy the webhook URL that is mentioned on the same page and save your configuration.

Go to your SendGrid dashboard, open the _Settings_ menu and go to your [_Mail Settings_](https://app.sendgrid.com/settings/mail_settings). Open and enable the _Event Notification_ setting. In the _HTTP POST URL_ field, paste the webhook URL that you just copied, and replace _YOUR-WEBHOOK-SIGNING-SECRET_ by the webhook signing secret that you chose in the last step.

At the bottom, enable these actions to be reported back to Mailcoach: _Dropped_, _Bounced_, _Opened_, and _Clicked_. Finally, press the blue checkmark in the top right corner to save your configuration:

![](https://mailcoach.app/images/docs/app/mail-configuration/sendgrid-event-notifications.png)

Your SendGrid configuration should now be complete, and you can go ahead and try sending a test mail. It may go to your spam if you have not set up your domain settings.
