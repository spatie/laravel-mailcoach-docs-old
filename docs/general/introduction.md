---
title: Introduction
---

//TODO: add screenshot

This package allows you to send out email campaigns to a list of subscribers effortlessly. It also provides a UI to manage campaigns, email lists and subscribers.

Let’s take a quick look at how to use the package. First, create an email list:

```php
$emailList = EmailList::create('newsletter subscribers');
```

Next, we subscribe people to a list. There's also support for [double opt-in subscriptions](https://docs.spatie.be/laravel-email-campaigns/v1/working-with-lists/using-double-opt-in/).

```php
$emailList->subscribe('john@example.com');
$emailList->subscribe('paul@example.com');
```

You can send an email to all those subscribed to the list.

```php
Campaign::create()
    ->subject('test')
    ->content($html)
    ->trackOpens()
    ->trackClicks()
    ->sendTo($emailList);
```

After a campaign is sent, [interesting statistics](https://docs.spatie.be/laravel-email-campaigns/v1/working-with-campaigns/viewing-statistics-of-a-sent-campaign/) are made available.

## We have badges!

[![Latest Version on Packagist](https://img.shields.io/packagist/v/spatie/laravel-email-campaigns.svg?style=flat-square)](https://packagist.org/packages/spatie/laravel-email-campaigns)
[![Build Status](https://img.shields.io/travis/spatie/laravel-email-campaigns/master.svg?style=flat-square)](https://travis-ci.org/spatie/laravel-email-campaigns)
[![Quality Score](https://img.shields.io/scrutinizer/g/spatie/laravel-email-campaigns.svg?style=flat-square)](https://scrutinizer-ci.com/g/spatie/laravel-email-campaigns)
[![StyleCI](https://github.styleci.io/repos/210674796/shield?branch=master)](https://github.styleci.io/repos/210674796)
[![Total Downloads](https://img.shields.io/packagist/dt/spatie/laravel-email-campaigns.svg?style=flat-square)](https://packagist.org/packages/spatie/laravel-email-campaigns)
