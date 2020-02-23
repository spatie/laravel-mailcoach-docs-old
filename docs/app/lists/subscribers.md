---
title: Subscribers
---

This page shows an overview of all the email addresses that have subscribed to this email list. When clicking on an email address, you can see some more of their details, and which campaigns they have already received while on this list.

![](https://mailcoach.app/images/docs/app/lists/subscribers-index.png)

## Statuses

There are a couple of different statuses that a subscriber can have: _unconfirmed_, _subscribed_ and _unsubscribed_.

**Unconfirmed**

When people have signed up for your list, but have not yet confirmed their subscription, they will be _unconfirmed_. You will only see this status the list requires confirmation.

You can manually confirm email addresses using the action menu next to their name, but we don't suggest doing this without the consent of the person this account belongs to.

**Subscribed**

These are the email addresses that will actually receive your emails.

**Unsubscribed**

If people have clicked the "unsubscribe" link in an email campaign, they will not receive any new emails from this list. Their email address will keep showing in this list until you manually remove them. You can easily remove all the unsubscribed addresses by using the action menu at the top.

You can manually revert an unsubscribe by using the action menu, but we strongly suggest not to abuse this action to respect users' privacy.

## Importing subscribers

![](https://mailcoach.app/images/docs/app/lists/subscribers-import.png)

You can easily import a list of subscribers into an existing list. Upload a CSV file with these columns: `email`, `first_name`, `last_name` and `tags`, and Mailcoach will start importing these into the list. When you want to attach multiple tags to a subscribe, use `;` as the delimiter.  You can follow the progress of the import, see any errors that occurred during the process, and download the uploaded file by using the action menu on the right.

![](https://mailcoach.app/images/docs/app/lists/subscribers-import-page.png)

Email addresses that have the _unsubscribed_ status will not be resubscribed when running an import. Confirmation mails will not be sent out to users, even if you have enabled double opt-in for this list. Imported email addresses that had not already unsubscribed, will receive the _Confirmed_ status and will receive any subsequently sent campaigns.

Because these lists can get quite large and an import might take a while, we send you an email to inform you when Mailcoach has finished importing the subscribers:

![](https://mailcoach.app/images/docs/app/lists/subscribers-import-report.png)

## Exporting subscribers

You can also export the entire list or parts of it. By filtering by name email address, status or tags, you can choose which lines will be included in an export. An export will create a CSV file with email addresses, first names, last names, and tags.

![](https://mailcoach.app/images/docs/app/lists/subscribers-export.png)
