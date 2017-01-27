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

Here's how to get Active Admin working on a Rails 5 API-only application, **without** having to disable API mode altogether or create separate apps.

### 1. Inherit from `ActionController::Base`

Running the Active Admin install generator on a Rails 5 API application results in the following error

{% highlight ruby %}
undefined method `helper_method' for InheritedResources::Base:Class (NoMethodError)
{% endhighlight %}

To fix, you need to re-enable view rendering behavior for the Active Admin base controller which inherits from `ApplicationController`.

_NOTE: This inheritance relationship is an unfortunate design choice that has been brought up a [couple][refactor 3] [times][refactor 1] [before][refactor 2].
Hopefully it is refactored._

[AA inheritance]:https://github.com/activeadmin/activeadmin/blob/master/docs/14-gotchas.md#authentication--application-controller
[refactor 1]:https://github.com/activeadmin/activeadmin/pull/1934
[refactor 2]:https://github.com/activeadmin/activeadmin/pull/1935
[refactor 3]:https://github.com/activeadmin/activeadmin/issues/3143

To keep all the API-mode goodness for controllers handling API requests, 
you're going to want to create a new controller base class that inherits from `ActionController::API`.
Move any application code that applies to API requests there.

{% highlight ruby %}
class ApiController < ActionController::API
  # move API-specific application code from ApplicationController to here
end
{% endhighlight %}

Update all your API controllers to inherit from this new base class.

Finally, change the `ApplicationController` to inherit from `ActionController::Base`.
This will allow the Active Admin install generator to successfully run.

### 2. Enable the flash

The install generator runs, but we can't view our Active Admin dashboard yet.

{% highlight ruby %}
ActionView::Template::Error: undefined method `flash'
{% endhighlight %}

You need to add in the middleware which enables the flash.

{% highlight ruby %}
# config/application.rb
module MyApp
  class Application < Rails::Application
    # ...

    # add this
    config.middleware.use ActionDispatch::Flash
  end
end
{% endhighlight %}

You now have a working dashboard!

### 3. (_For Devise_) Allow session management

You are most likely using Devise for authentication on your admin panel.
If so, you'll notice that you won't be able to sign in.
It just keeps bringing you back to the sign in page even though your credentials are correct.

In order to sign in successfully, you need to enable the middleware for session management.

{% highlight ruby %}
# config/application.rb
module MyApp
  class Application < Rails::Application
    # ...

    # add these
    config.middleware.use ActionDispatch::Cookies
    config.middleware.use ActionDispatch::Session::CookieStore
  end
end
{% endhighlight %}

Congratulations, you can now successfully sign in and access your Active Admin dashboard.
