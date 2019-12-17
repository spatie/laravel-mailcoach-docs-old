---
title: Creating a campaign
---

When creating a campaign, you can either create a new one from scratch, or duplicate an existing one to copy most of its settings:

![](https://mailcoach.app/images/docs/app/campaigns/creating-a-campaign-index.png)

When creating a new campaign, you can choose which of your templates, if any, you want to use as a base for your email's content. You can also choose to duplicate an existing campaign. Use these actions to save time when creating a new campaign.

## Settings

![](https://mailcoach.app/images/docs/app/campaigns/creating-a-campaign-settings.png)

Most of the settings for creating a new campaign are pretty self explanatory:

The name of a campaign is only used within your Mailcoach UI. Subscribers will not see this anywhere.

The value for the _Subject_ field is used as the subject for sent emails.

The _List_ menu allows you to pick one of your lists, which you can further narrow down by picking one of its _Segments_ right below it. You can read more about list segments [here](todo:link-to-segment-docs).

Finally, the tracking options allow you to track how many subscribers have opened your email and whether they clicked any of the links you included. You can see what the result of this tracking look like in the [Campaign statistics](todo:link-to-campaign-stats-docs) section of the documentation.

## Content

![](https://mailcoach.app/images/docs/app/campaigns/creating-a-campaign-content.png)

This is the content of the email that will be sent to your subscribers. If you selected a template while creating your campaign, the field will be prefilled with that template. If you duplicated another campaign, this field will be the same as that campaign's content.

You can -_and should_- use placeholders in your emails. We strongly suggest including at least the `::unsubscribeUrl::` in every email you send, for example at the bottom of the email. Not including this link may lead to your users complaining or emails being marked as spam.

While editing your email, you can see what it will look like for subscribers by clicking the _Preview_ button.

## Delivery

![](https://mailcoach.app/images/docs/app/campaigns/creating-a-campaign-delivery.png)

This page provides a final checklist that you should go over before sending a campaign. It shows a summary of the campaign's settings, like how many people will be sent an email, the subject and any issues we found with your email's content.

You can also send a test email to yourself for a final review before sending it out to your subscribers' mailboxes.

Finally, you can set the timing for this campaign: whether to send it right now, or to schedule delivery for a moment in the future. You can use this to set up multiple campaigns to be sent without you having to micromanage the delivery. As long as the campaign hasn't started sending emails, you can reschedule or cancel it.

When sending a campaign, all the emails that need to be sent out will be placed in a queue, and you will be redirected to a page where you can track the progress and see your campaign's statistics trickling in:

// TODO // Screenshot campaign stats summary (with progress bar?)

For more information on what these statistics mean, continue reading in the [campaign's statistics](todo:link) section of the documentation.
