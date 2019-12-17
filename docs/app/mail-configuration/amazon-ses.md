---
title: Amazon Simple Email Service (SES)
---

To send mails with Amazon SES, you need to do some configuration in the AWS dashboard first. To get started, open your AWS dashboard, select your preferred region and go to Simple Email Service under _Services_.

Firstly, you need to verify your domain and the email address mails will come from (your _From email_ in Mailcoach lists). Both of these are out of the scope of this tutorial, so we refer you to [SES' documentation](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/setting-up-email.html). You can send campaigns from SES without verifying your domain, however, this might cause your mails to be flagged as spam by mail clients, and you will not be able to track any statistics from your campaigns.

### Key and Secret (sending emails)

Your SES key and secret, or _Access Key ID_ and _Secret Access Key_ as they are called in AWS, are the credentials Mailcoach needs to be able to send emails using SES. To know more about these keys, read about them in the [AWS documentation](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys).

To get your key and secret, you need to create a new user. Go to the _IAM_ service in AWS, and then to _Users_ under _Access management_ and press the _Add user_ button:

![](https://mailcoach.app/images/docs/app/mail-configuration/amazon-ses-iam.png)

Choose a name for your new user, and make sure to enable _Programmatic access_. This allows it to send requests to the AWS API, and is required to be able to send mails from a third party platform (like Mailcoach):

![](https://mailcoach.app/images/docs/app/mail-configuration/amazon-ses-programmatic-access.png)

Go to the next page, and set the permissions for the user. Since we won't be creating multiple users in this tutorial, we will simply use the _Attach existing policies directly_ option to add the _AmazonSESFullAccess_ permission to this user:

![](https://mailcoach.app/images/docs/app/mail-configuration/amazon-ses-permissions.png)

Go over to the next page. We are skipping tags for now and continuing to the _Review_ page. Make sure the details are correct, and verify creating the user.

![](https://mailcoach.app/images/docs/app/mail-configuration/amazon-ses-review.png)

If everything went OK, you should now be able to see your user's Access Key ID and Secret access key:

![](https://mailcoach.app/images/docs/app/mail-configuration/amazon-ses-key-and-secret.png)

Go to the Mailcoach Mail Configuration page (in the user menu in the top right), make sure you have selected the _Amazon SES_ driver and enter the Key and Secret:

![](https://mailcoach.app/images/docs/app/mail-configuration/amazon-ses-key-and-secret-in-mailcoach.png)

The amount of mails per second depends on your AWS account and settings. A good starting point is 5 mails per second.

### Configuration Set (tracking events)

Amazon SES requires users to track any bounced messages, you need to create an SES Configuration Set so Mailcoach can track these.

Open your AWS Dashboard and make sure you still have the same AWS region selected as where you verified your domain and sending email address. Go to the _Simple Email Service_ under _Services_, and find the _Configuration Sets_ menu item under _Email Sending_. Press the _Create Configuration Set_ button:

![](https://mailcoach.app/images/docs/app/mail-configuration/amazon-ses-configuration-set.png)

Choose a name for your configuration set and create it, then click the newly created item in the list to add some events destinations. Click the _<Select a destination type>_ dropdown and select the _SNS_ option:

![](https://mailcoach.app/images/docs/app/mail-configuration/amazon-ses-destination-type.png)

In the window that pops up, choose a name and select the following event types: _Reject_, _Bounce_, and _Complaint_. Next, press the _Topic_ dropdown and choose _Create SNS Topic_:

![](https://mailcoach.app/images/docs/app/mail-configuration/amazon-ses-sns-destination.png)

Another window will open, choose a topic and display name, press _Create Topic_ and _Save_. You should now see your newly created Configuration Set:

![](https://mailcoach.app/images/docs/app/mail-configuration/amazon-ses-finished-config-set.png)

Now, go to Simple Notification Service (SNS) in your AWS dashboard to further configure the topic you just created. Open the _Topics_ menu and select the topic.

![](https://mailcoach.app/images/docs/app/mail-configuration/amazon-ses-edit-sns-topic.png)

Press the _Create subscription_ button on this page, choose the _HTTPS_ protocol and enter the webhook URL that you can find on your Mailcoach Mail configuration page (`https://YOUR-DOMAIN.com/ses-feedback`):

![](https://mailcoach.app/images/docs/app/mail-configuration/amazon-ses-sns-subscription.png)

Scroll down and press the _Create subscription_ button. If everything was configured correctly, the subscription should confirm itself and the _Status_ should reflect this after a page refresh.

![](https://mailcoach.app/images/docs/app/mail-configuration/amazon-ses-sns-subscription-confirmed.png)

To complete you Amazon SES configuration, you need to enter your configuration set's name and AWS region in your Mailcoach Mail configuration:

![](https://mailcoach.app/images/docs/app/mail-configuration/amazon-ses-final-mailcoach-mail-config.png)

Your Amazon SES configuration should now be complete, and you can go ahead and try sending a test mail. It may go to your spam if you have not set up your domain settings.
