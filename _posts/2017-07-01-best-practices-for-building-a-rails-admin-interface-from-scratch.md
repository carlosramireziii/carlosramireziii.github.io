---
layout: post
title: Best Practices For Building A Rails Admin Interface From Scratch
---

## Sometimes "roll your own" is the best option

When it comes to admin interfaces in Rails, many of us turn to Active Admin or Rails Admin for a solution.

Using one of these gems is a no-brainer when all you need is a simple CRUD interface to manage your data,
but when your requirements call for something more custom,
you may find yourself considering building an admin interface from scratch.

## Going custom will be painful if you choose the wrong architecture

Deciding to go custom brings the flexibility to implement the admin panel whichever way you want.
With that freedom, however, comes the danger of making a poor design choice.

What if you accidentally choose a bad architecture?
What if other developers can't understand or disagree with your design?
Worst of all, what if you end up having to change it later on because the initial implementation wasn't good?

Luckily there are some best practices you can follow which mitigates this risk.

## Follow these best practices and you'll be happy in the long run

### Don't share controller logic

It may be tempting at first to reuse your existing controllers for your admin interface, but 
this turns out to be a bad idea.

Admin controllers require privileged access, and
you don't want to risk data leaking to a non-admin user. 
Using a separate controller for all admin actions (despite feeling like it may be breaking the DRY principle) is preferable because you reduce the risk of making a mistake that results in a security issue.

### Use a namespace

For organizational purposes, use a controller namespace (e.g. `Admin`).
All your admin-related controllers and views will be in an `'admin/'` directory which makes them easy to find and to distinguish from the non-admin ones.

### Create a separate base controller

Since admin controllers require strict access control, it's helpful to enforce that from the top-level (similar to how `require_authentication` often lives inside `ApplicationController`).

If all admin controllers inherit from a single parent that has the authorization check you won't risk inadvertently making one of them publicly accessible.

### (Optional) Use a separate layout and/or assets

Depending on how different you main application is from your admin interface,
you may want to consider using a separate application layout file and/or asset manifest file.

## Here's an example

[Click here](https://gist.github.com/carlosramireziii/651062e9fb434543a5865a9fc48aaee8){:target="_blank"} to see a sample of the admin setup I use that incorporates all of the best practices above.
