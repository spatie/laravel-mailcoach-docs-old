---
title: Tracking clicks
---

The package can track when and how many times a subscriber clicked on a link in a campaign.

## Enabling click tracking

To use this feature, you must set `track_clicks` to `true` on a campaign you're going to send. You can find an example of how to do this in the section on [how to create a campaign](/docs/v3/package/working-with-campaigns/creating-a-campaign).

## How it works under the hood

When you send a campaign that has click tracking enabled, the email service provider you use will replace each link in your mail by a unique link on it's own domain. When somebody clicks that link, the email service provider will get a request, and it will know that the link was clicked. Next the request will be redirected to the original link.

Via a web hook, the email service provider will let Mailcoach know that a link has been clicked.
