---
title: Installation using the 1-click installer
---

You can use a one click installer to provision a new server at DigitalOcean. This new server will have Mailcoach and all its dependencies pre-installed. You don't need any Laravel or PHP knowledge to get started.

You can start the Mailcoach 1-click installer at [the market place of Digital Ocean](https://marketplace.digitalocean.com/apps/mailcoach?refcode=daf998eae49e).

Here's a video that walks you through the process.

<iframe src="https://player.vimeo.com/video/402762711" width="640" height="360" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>

After the server is set up, you are responsible for maintaining it. We highly recommend [enabling automatic backups at Digital Ocean](https://www.digitalocean.com/docs/images/backups/quickstart/).
 
 Automatic security updates are enabled by default. The database credentials can be found in `/root/.digitalocean_password`. You might want to copy that password to a safe place and delete the file.

### Making sure everything works

Before sending out a real campaign, we highly recommend creating a small email list with a couple of test email addresses and send a campaign to it. This way, you can verify that sending mails, and the open & click tracking are all working correctly.

## Troubleshooting

If after provisioning, you can't access it via the web it could be caused that certbot couldn't get a certificate. Make sure that the domain name you want to use has an A record that points to the server. You can let certbot retry getting a certificate by running `certbot --nginx` on your server.
