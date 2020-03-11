---
title: Tracking opens
---

The package can track when and how many times a subscriber opens a campaign.

## Enabling open tracking

To use this feature, you must set `track_opens` to `true` of a campaign you're going to send. You can find an example of how to do this in the section on [how to create a campaign](/docs/v2/package/working-with-campaigns/creating-a-campaign).

## How it works under the hood

When you send a campaign that has open tracking enabled, the email service provider will add a web beacon to it..  A web beacon is an extra `img` tag in the HTML of your mail.  Its `src` attribute points to an unique endpoint on the domain of the email service provider.

Each time an email client tries to display the web beacon it will send a `get` request to email server. This way the email service provider will know the mail has been opened.

Via a web hook, the email service provider will let Mailcoach know that the mail has been opened.
