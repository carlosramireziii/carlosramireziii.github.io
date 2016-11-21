---
layout: post
title: When To Use 'escape_javascript' in an SJR Template
---

A server-generated JavaScript response (SJR for short) is a perfectly
valid way to handle simple AJAX operations within a Rails app, and there
are
[many
benefits](https://signalvnoise.com/posts/3697-server-generated-javascript-responses)
in doing so. There is one common GOTCHA however that trips up even the most experienced
Rails developer: forgetting to use `escape_javascript` when rendering
content to the DOM.

<!-- more -->

## Does this sound familiar?

```
$("#posts-list").html("<%= render(@posts) %>");
```

Have you ever written a `create.js.erb` SJR template like the one above
and found that when you tested it in your app nothing happened? If so,
then you have experienced this pain.

Even after a couple run-ins with this issue, perhaps you still aren't
exactly sure _which_ situations require `escape_javascript` and which don't.

And what the heck is the difference between `escape_javascript` and `j`?
Are these interchangeable?

## What does `escape_javascript` do?

From the [Rails
documentation](http://api.rubyonrails.org/classes/ActionView/Helpers/JavaScriptHelper.html#method-i-escape_javascript),

> Escapes carriage returns and single and double quotes for JavaScript
> segments.

All we are doing is taking a string of text and making sure that it
doesn't contain any invalid characters when the browser tries to parse
it. Simple!

For a more in-depth discussion, read
[here](http://stackoverflow.com/questions/1620113/why-escape-javascript-before-rendering-a-partial#1623813).

## To escape or not to escape

Now that you know what `escape_javascript` does, the next question is
when to use it.

Here's a short answer: **always** :)

More specifically, any time you are trying to render HTML as a string
and insert it into the DOM you'll want to escape it first.

Some common examples include

```
// rendering a collection of records as a partial
$("#comments").html("<%= escape_javascript render(@comments) %>");

// rendering a single record as a partial
$("#comments").append("<%= escape_javascript render(@comment) %>");

// rendering a single bit of text from a record's attribute
$("#post-title").html("<%= escape_javascript @post.title) %>");
```

Not _every_ situation requires `escape_javascript`, e.g.

```
// Not required here
if (<%= @record.valid? %>) {
  // ...
}
```

however you'll find yourself getting into trouble more often by
forgetting to escape as opposed to escaping unnecessarily.

## What's the deal with `j`?

You may have seen `j` being used in some code samples.

The `j` method is nothing more than an alias for `escape_javascript`. It's a shortcut to save space and make the code more readable.

```
// This...
$("#comments").html("<%= j render(@comments) %>");
// is equivalent to...
$("#comments").html("<%= escape_javascript render(@comments) %>");
```

## Now you know

SJR templates are a great tool to have in your Rails arsenal, and by
learning how and when to use `escape_javascript` (or `j`) within those
templates, you will save yourself hours of headaches debugging an
unresponsive template.

### Related Articles

- [3 Reasons Why Your SJR Template Isn't Working]({% link
  _posts/2016-11-20-3-reasons-why-your-sjr-template-isnt-working.md %})
- [Why Isn't Server-Generated JavaScript More Popular?]({% link
  _posts/2016-10-04-when-to-use-escape-javascript-in-an-sjr-template.md %})

*[SJR]: Server-Generated JavaScript
