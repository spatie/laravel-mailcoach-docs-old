---
title: Tracking clicks
---

The package can track when and how many times a subscriber clicked on a link in a campaign.

## Enabling click tracking

To use this feature, you must set `track_clicks` to `true` on a campaign you're going to send. You can find an example of how to do this in the section on [how to create a campaign](https://docs.spatie.be/laravel-email-campaigns/v1/working-with-campaigns/creating-a-campaign/).

## How it works under the hood

When you send a campaign that has click tracking enabled, we'll replace each link in the mail with a link that points to the `Spatie\Mailcoach\Http\Controllers\TrackClicksController`. A route to this controller has been set up by the `Route::MailCoach` you used when [configuring the package](https://docs.spatie.be/laravel-email-campaigns/v1/installation-setup/#add-the-route-macro).

Here's an example: a link with `href` `https://spatie.be` will be replaced with `https://yourapp.com/email-campaigns/track-clicks/<campaign-link-uuid>/<subscriber-uuid>?redirect=https://spatie.be`.

## Queuing click tracking

When sending a campaign to a large list, that endpoint could be hit a lot over a short timespan. To ensure a fast response time, when the `TrackClicksController` is hit, it will not update the database, but dispatch a job named `RegisterClickJob`. The controller will respond with a redirect to the url that is the `redirect` URL parameter. The dispatch `RegisterClickJob` job update the database.

Because there's the potential for a great many of these jobs to be scheduled, we recommend using a separate queue for handling them. You can configure the queue to be used in the `perform_on_queue.register_click_job` key of the `email-campaigns` config file.
