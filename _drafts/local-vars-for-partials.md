---
layout: post
title: "Stop using instance variables inside your partials"
---

Sadly, it's common to see Rails developers using instance variables
within their partials rather than explicitly passing the data through
local variables.

<!-- more -->

At some point you may have asked yourself the question, "should I bother
setting locals for this partial or can I just use the instance
variable that's already defined?"

It's tempting to take the instance variable route. For one thing, it's
more work to set local data for the partial when an instance
variable just works and is already defined for you by the controller.
Worse, if you are rendering partials within other partials, then you need
to pass the local data each time.

Redefining the data can seem weird and not DRY, especially
when the local variable shares the exact same name as the instance
variable, e.g. `locals: { user: @user }`

Lastly, maybe you are just following the Rails scaffold generator
convention, which uses instance variables in its `_form.html.erb`
partial.

Despite all of that, follow this rule: **always prefer
local variables for a partial over instance variables**.

Here's why...

## Partials using local variables are easier to reuse

Take the following `users/_mailing_address.html.erb` partial

{% highlight ruby %}
<address>
  <strong><%= @user.full_name %></strong><br>
  <%= @user.address %><br>
  <%= @user.city_state_zip %><br>
  <abbr title="Phone">P:</abbr><%= @user.phone %>
</address>
{% endhighlight %}

This works fine from a controller action that sets a `@user`
instance variable.

But what if we want to show the mailing address for the current user
instead? How about for the author of an article? A collection of
users?

Local variables make all of these scenarios easy.

```
<%= render "users/mailing_address", user: current_user %>

<%= render "users/mailing_address", user: article.user %>

<%= render "users/mailing_address", collection: users %>
```

Reusing a partial anywhere in your app becomes as simple as calling
`render` and passing it the data it needs. No controllers, no instance
variables, no hassle.

## Partials that use local variables are more maintainable

If a partial uses instance variables, then it is tightly coupled to the
controller action which uses it. This has the following downsides...

* **Changes in the controller require changes in the partial**. Want to
  rename that instance variable? You'll have to remember to rename it in
  the partial as well.

* **Changes in the partial require changes in the controller**.
  Don't even bother renaming an instance variable inside the partial because
  you'll find yourself hunting down every controller action which uses
  it and rename those variables too.

* **Reusing the partial requires specific setup in the controller**. Your
  partial won't work unless you set up an instance variable in the
  controller with the exact name and structure required by the partial.

Partials that are set up to use local data are much more resilient to
change.

## Partials that use local variables are easier to debug

Instance variables can be set anywhere: in a helper, in a controller, in
a controller high up in the inheritance chain (e.g.
`ApplicationController`), in a view, and many others.

If an instance variable is used within a partial then we have no idea
where it came from. A local variable, on the other hand, can be traced
back to the `render` call which used the partial.

## Conclusion: say no to instance variables

Go forward with confidence choosing local variables over instance
variables in your partials, and happily enjoy all the benefits described
above!

*[DRY]: Don't Repeat Yourself
