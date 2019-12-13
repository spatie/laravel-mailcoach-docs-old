---
title: Settings
---

## General settings

Name —> naam van de lijst
From email —> mensen op deze lijst gaan mail van deze persoon krijgen
From name —> ^

Publish feed = publieke RSS feed die iedereen kan bekijken

## Reports

- “a confirmation when an campaign gets sent to this list” —> krijg een mail wanneer de campagne wordt **verzonden** (bij scheduled campaigns worden deze dus pas verstuurd wanneer ze naar de subscribers van de list worden verstuurd)

- “a summary of opens, clicks & bounces a day after a campaign to this list has been sent”: —> 1 dag erna krijg je een summary van alle opens, clicks & bounces

- “a weekly summary on the subscriber growth of this list” —> elke week zien hoeveel mensen er zijn subscribed en unsubscribed zijn

- TODO: voorbeelden van zo’n report emails (bestaan nog niet)

## Subscriptions

- “Require confirmation”: (Double-opt in gedoe)
  --> By default via ons (+ screenshot van die confirmation pagina, staat bij ons)
  --> Je kan een custom mail versturen, verwijzen naar "## Confirmation mail" (titel op dezelfde pagina)

- “Allow POST from an external form” —> https://github.com/spatie/laravel-mailcoach-docs/blob/master/docs/working-with-lists/using-subscription-forms.md
  —-> bestaande docs overnemen maar zonder code-voorbeelden etc, HTML-dingen mogen wel (hidden fields)

## Landing pages

Urls die gebruikt worden als iemand subscribed/confirmed

- enkel in te vullen als je je eigen pagina’s met eigen layout wilt gebruiken en onze defaults wil overschrijven (foto’s van defaults?)

- Deze kunnen per inschrijving-formulier apart overschreven worden dmv hidden fields in het form
  --> Zie hidden fields van hierboven

// TODO: screenshot default welcome mail

## Welcome mail

Opties uitleggen

// TODO: screenshot default welcome mail

## Confirmation mail

Uitleggen

// TODO: screenshot default welcome mail
