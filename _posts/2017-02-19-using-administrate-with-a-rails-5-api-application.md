---
layout: post
title: Using Administrate with a Rails 5 API Application
---

## Admin dashboards make life easier, but they don't necessarily work with API apps

As you know from [previous][active_admin_post] [posts][rails_admin_post], 
even though an admin panel is an essential tool for managing your application's data,
getting one set up for a Rails 5 API app isn't straightforward. 

In most cases, you'll need to re-add some of the middleware that Rails API-mode omits as part of its slimmed down stack.
But the requirements are different for each admin framework,
and there is no consistent set of instructions that works for all.

## Administrate is a great new admin framework

One of the newest additions to the scene is thoughtbot's [Administrate][administrate_gem] gem.

It seeks to improve upon existing solutions by avoiding DSLs, 
using conventional Rails architecture for overriding defaults, 
and breaking the library into small components and plugins.

If you haven't heard of it, [check it out][administrate_announcement].

Unfortunately it does not have out-of-the-box support for Rails 5 API mode (yet).

## With a few extra steps, you can have it working in your app without losing the benefits of API-only mode

Luckily, it isn't too difficult to set up.
Here are the steps:

### 1. Require `bourbon`

After adding the gem and following the install instructions, the first error you'll encounter when trying to view the `/admin` route is related to a Bourbon dependency.

{% highlight ruby %}
ActionView::Template::Error: File to import not found or unreadable: bourbon
{% endhighlight %}

The fix is to manually require the library,
which can be done in one of two ways:

1. Add `require 'bourbon'` to `config/application.rb`
1. ***or*** Add `bourbon` to the `Gemfile`

### 2. Add missing routes

The next error you'll have to deal with relates to routing.

{% highlight ruby %}
ActionView::Template::Error: undefined method `new_admin_foo_path'
{% endhighlight %}

In a standard Rails application,
declaring a resourceful route using the `resources` method will generate seven different CRUD endpoints, including `new` and `edit`.

In a Rails API-only application however,
the resourceful route method skips the `new` and `edit` routes since those are typically not needed by an API. 

Your Administrate dashboard needs all seven, so you'll have to add in the missing routes manually.

Again, there are a couple of different ways to do this.

{% highlight ruby %}
# OPTION 1: Ditch `resources` and re-write all the routes manually
namespace :admin, as: '' do
  # resources :foos
  get '/foos', to: 'foos#index', as: 'admin_foos'
  get '/foos/new', to: 'foos#new', as: 'new_admin_foo'
  post '/foos', to: 'foos#create'
  get '/foos/:id', to: 'foos#show', as: 'admin_foo'
  get '/foos/:id/edit', to: 'foos#edit', as: 'edit_admin_foo'
  patch '/foos/:id', to: 'foos#update'
  delete '/foos/:id', to: 'foos#destroy'
end

# OPTION 2: Keep `resources` and add the missing routes only 
namespace :admin, as: '' do
  get '/foos/new', to: 'foos#new', as: 'new_admin_foo'
  get '/foos/:id/edit', to: 'foos#edit', as: 'edit_admin_foo'
end
namespace :admin do
  resources :foos
end
{% endhighlight %}

Notice the use of `as: ''` to make sure the URL helper names are what Administrate expects.

### 3. Add missing middleware

Lastly, add in the middleware needed to generate views properly otherwise you'll run into the following errors

{% highlight ruby %}
ActionView::Template::Error: undefined method `flash'
{% endhighlight %}

{% highlight ruby %}
ActionController::InvalidAuthenticityToken: ActionController::InvalidAuthenticityToken
{% endhighlight %}

Put the middleware calls into `config/application.rb`

{% highlight ruby %}
module MyApp
  class Application < Rails::Application
    # ...

    # add these =>
    config.middleware.use ActionDispatch::Flash
    config.middleware.use Rack::MethodOverride
    config.middleware.use ActionDispatch::Cookies
    config.middleware.use ActionDispatch::Session::CookieStore
  end
end
{% endhighlight %}

That's it!
Your Administrate dashboard should now load and work properly.

## What about other admin panel frameworks?

Administrate is great,
but there are many other popular options (e.g. Active Admin, Rails Admin).

To see which ones are compatible with Rails 5 API apps,
I have created a handy chart for you to reference.

[Rails 5 API-mode Admin Framework Compatibility Chart][compatibility_chart_post]

Hope that helps!

[administrate_announcement]: https://robots.thoughtbot.com/announcing-administrate
[administrate_gem]: https://github.com/thoughtbot/administrate
[active_admin_post]: {{ site.baseurl }}{% post_url 2017-01-29-how-to-add-active-admin-to-a-rails-5-api-application %}
[rails_admin_post]: {{ site.baseurl }}{% post_url 2017-02-12-how-to-use-rails-admin-with-a-rails-5-api-application %}
[compatibility_chart_post]: {{ site.baseurl }}{% post_url 2017-02-12-rails-5-api-mode-admin-framework-compatibility-chart %}
