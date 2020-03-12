---
title: Monaco
---

<a href="https://microsoft.github.io/monaco-editor/">Monaco</a> is a powerful code editor created by Microsoft. It
provides code highlighting, auto completion and much more.

![screenshot](/images/docs/v2/app/editors/monaco.png)

## Installation

You can install the package via composer:

```bash
composer require spatie/laravel-mailcoach-monaco
```

### Publish the assets

You must publish the JavaScript and CSS assets using this command:

```bash
php artisan vendor:publish --tag mailcoach-monaco-assets --force
```

Every time the package is updated you'll need to run that command. You can automate this by adding it to your `post-update-cmd` script in `composer.json`.

```
"scripts": {
    "post-update-cmd": [
        "@php artisan vendor:publish --tag mailcoach-monaco-assets --force"
    ]
}
```

### Configuring Mailcoach

Set the `mailcoach.editor` config value to `\Spatie\MailcoachMonaco\MonacoEditor::class`

### Configuring the looks

You can change some Monaco editor options by adding a `monaco` configuration key to your `mailcoach.php` config file.

```php
'monaco' => [
    'theme' => 'vs-light', // vs-light or vs-dark
    'fontFamily' => 'Jetbrains Mono',
    'fontLigatures' => true,
    'fontWeight' => 400,
    'fontSize' => '16', // No units
    'lineHeight' => '24' // No units
],
```