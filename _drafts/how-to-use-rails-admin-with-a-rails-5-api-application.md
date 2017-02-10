---
layout: post
title: How to Use Rails Admin with a Rails 5 API Application
---

## You can use API-only mode _and_ have an admin panel in the same Rails app

In my [previous article][previous_post], I discussed how you don't need to choose between abandoning Rails API-only mode or splitting up your API and admin panel into two separate apps.
Instead, you can get the best of both worlds.

[That article][previous_post] used the Active Admin framework.
But what if you want to use Rails Admin instead?

## Rails Admin now works by default with Rails 5 API-mode

Using Rails Admin with a Rails 5 API application is effortless.

As of `v1.0.0` Rails Admin boasts "out-of-box support" of Rails 5 API mode.
The gem itself injects the missing middleware it needs to render its views without requiring you to disable `config.api_mode` or make any other code or configuration changes.

Check out the [CHANGELOG][changelog] and [commit][commit] for the nitty gritty details.

## Does your favorite admin dashboard work with Rails 5?

There are plenty of other admin frameworks besides Active Admin and Rails Admin.
To see how each one is compatible (or not) with Rails 5, 
I have created a handy chart for you to reference. 

[Rails 5 API-mode Admin Framework Compatibility Chart](compatibility_chart)

[previous_post]: {{ site.baseurl }}{% post_url 2017-01-29-how-to-add-active-admin-to-a-rails-5-api-application %}
[changelog]: https://github.com/sferik/rails_admin/blob/master/CHANGELOG.md#100---2016-09-19
[commit]: https://github.com/sferik/rails_admin/commit/53eef4fe2ec0953381f4e3197c885adc0423dd49
[compatibility_chart]: www.google.com
