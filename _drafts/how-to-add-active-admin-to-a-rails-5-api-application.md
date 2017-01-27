---
layout: post
title: How To Add Active Admin to a Rails 5 API Application
---

## No one wants to manage application data without an admin panel

No developer wants to manage application data without the benefit of a front-end interface, 
and API applications are no exception.

Rails 5 makes it easy to build a beautiful and clean API-only application, 
but when data starts flowing you need an easy way to visualize and manage it.

## Rails has admin frameworks, but they don't work for API apps

The good news is that Rails already has plenty of great admin dashboard solutions.
One of the most popular is Active Admin.

But here's the problem: you can't use it with a Rails 5 API application.
Because API applications omit all the middleware for rendering HTML,
trying to install Active Admin will fail when it tries to generate views and assets for its dashboard.

## You have a hard choice to make...

Should you abandon API mode in favor of the full Rails stack?

You obviously don't want to lose the benefits of the slimmed-down middleware stack for your API controllers.
So having to set `config.api_mode = false` feels like a failure.

What about creating an entirely new and separate Rails app just for Active Admin?

Maintaining separate repositories, deployments, and trying to share business logic across apps is a lot more work and maintenance overhead.

## Why not the best of both worlds?

Here's how to get Active Admin working on a Rails 5 API-only application, without having to disable API mode altogether _or_ creating separate apps.
