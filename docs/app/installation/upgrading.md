---
title: Upgrading
---

If you installed Mailcoach as a package inside an existing Laravel app, you need to follow [these instructions](https://mailcoach.app/docs/v2/package/general/upgrading).

An in-place upgrade of the full Mailcoach app is hard. Instead, we recommend creating a new fresh Mailcoach 2 app and updating your current database and settings to the new v2 format.

You can create a new Mailcoach app with this command

```bash
composer create-project spatie/Mailcoach
```

## Upgrading the database

In your database you should add a few columns:

#### media

- `conversions_disk`: string, nullable
- `uuid`: uuid, nullable,

#### mailcoach_email_lists

- `campaign_mailer`: string, nullable
- `transactional_mailer`: string, nullable
- `welcome_mail_delay_in_minutes`: integer, default value `0`

#### mailcoach_campaigns

- `structured_html` : text, nullable

#### mailcoach_templates

- `structured_html` : text, nullable

## Upgrading settings

You should enter your mailer settings in the mail configuration section. You'll can find your old settings in the `mailConfiguration.json` of your old Mailcoach app.
