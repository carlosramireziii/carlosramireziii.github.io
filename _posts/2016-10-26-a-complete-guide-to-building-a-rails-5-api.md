---
layout: post
title: A Complete Guide to Building a Rails 5 API
tags: api rails-5
---

**Have you been looking for a thorough, easy to follow guide to building
an API using Rails and its new API-only mode?**

If so, I recommend checking out Kam Low's [_Building the Perfect Rails 5 API Only
App_](http://sourcey.com/building-the-prefect-rails-5-api-only-app/){:target="_blank"}

Unlike other tutorials, which can either be too basic or incomplete, this one outlines every step-- from generating
the initial Rails project all the way to adding authentication and
securing the API from malicious attacks. It's also straightforward to follow
along, much like the Michael Hartl Tutorial for learning Rails.

The topics covered are:

* Project setup
* Testing with RSpec
* JSON serialization using `ActiveModelSerializers`
* Enabling cross domain requests using CORS _<== this one is huge_
* Versioning the API
* Rate limiting, throttling, and other security measures
* Authentication using tokens

By the end of the tutorial you'll have a fully-functioning API ready to
be consumed by a client-side JS framework, native mobile app, or
other integration. Check it out!

---

_Want to add an admin dashboard to your API app?_

_Most admin gems don't work out-of-the-box with Rails 5 API mode,_
_but with a little extra setup we can get them running._

_[Click here][Compatibility Chart] to see how._

[Compatibility Chart]: {{ site.baseurl }}{% post_url 2017-02-12-rails-5-api-mode-admin-framework-compatibility-chart %}
