---
title: Creating a custom editor
---

By default, Mailcoach ships with a standard HTML field as editor. If you want some more wysiwyg functionality, we also provide an [Unlayer editor package](https://github.com/spatie/laravel-mailcoach-unlayer).

You can also create and provide your own editor.

## Creating an editor

An Editor is a simple PHP class that implements the `\Spatie\Mailcoach\Support\Editor\Editor` interface.

It requires that you implement a `render()` function that accepts a `\Spatie\Mailcoach\Models\Concerns\HasHtmlContent` object, this is a model that has 2 methods on it.

You can use the default `\Spatie\Mailcoach\Support\Editor\TextEditor` implementation as a reference

```php
public function render(HasHtmlContent $model): string
{
    return view('mailcoach::app.campaigns.draft.textEditor', [
        'html' => $model->getHtml(),
    ])->render();
}
```

This renders the `textEditor.blade.php` view of Mailcoach that makes heavy use of Blade Components.

This view will be rendered inside the forms of Campaigns and Templates, the only thing we require is that the Editor passes **valid html** through an input with the name `html`.

Mailcoach provides a secondary `structured_html` field in which you can pass anything your Editor needs to rebuild its view (our Unlayer editor uses this to store the Unlayer specific JSON format).
