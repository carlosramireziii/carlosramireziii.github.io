---
layout: post
title: "A Step-By-Step Guide to Debugging SJR Templates"
tags: sjr
custom_cta: ctas/debug_sjr_guide_cta.html
related_posts: false
---

In my [previous article][sjr_errors_post], we went over the different types of errors you'll commonly encounter in an SJR template.
But even armed with that knowledge, you may still find yourself at a loss for how to debug a template that isn't working.
Sometimes you've run out of ideas and just need to know what to do next. 

For times like those, I find it helpful to have a systematic debugging plan. 
Having a step-by-step debugging process keeps you from just taking shots in the dark, hoping to get lucky and find the issue by chance.

Here are the steps that I take whenever I encounter a buggy SJR template. 

<!-- more -->

## The process in action

To illustrate the strategy, let's follow along with a simple SJR template example.

{% highlight erb %}
<!-- _comment.html.erb -->
<li>
  <%= comment.content %>
  replied to <%= @post.title %>
</li>
{% endhighlight %}

{% highlight js %}
// create.js.erb
$("ul#comments").appendOnto("<%= render(@comment) %>");
{% endhighlight %}

Believe it or not, this small example contains 3 errors commonly responsible for most SJR issues:

1. Because `appendOnto` is not a valid jQuery function, a **JavaScript runtime error** will occur
1. Rendering the comment partial without defining the `@post` variable in the `CommentsController#create` action results in a **Ruby runtime error**
1. Forgetting to call `escape_javascript` (or `j`) in the embedded Ruby `render` call will cause the JavaScript to be malformed, a **JavaScript syntax error**

You'll see how having a process for debugging can help uncover each of these errors in a systematic way. 

### Step 1: Confirm the SJR template is even being rendered

When your JavaScript isn't executing, it's tempting to jump right into the template and start looking for an error there.
But sometimes the issue lies before the template is even rendered, somewhere in the Rails stack. 
For this reason, I always start by confirming that the SJR template is definitely being rendered in the first place.

The simplest way to do this is by replacing the entire contents of the template with an `alert`. 

{% highlight js %}
// create.js.erb
alert("Hello world!");
{% endhighlight %}

If the `alert` displays properly, then you know that your template is being rendered. 
If the `alert` *does not* show up, that means that your template is never being rendered and that you have a problem in your controller or business layer. 
In this way, you avoid scrutinizing the template when your problem lies elsewhere.  

#### Beware of this GOTCHA...

Be sure to physically remove *all* the template code; don't just comment it out. 
ERB tags are still processed even within a JS comment, 
so any Ruby errors contained within will prevent your JS code from running and you'll never see the `alert` message be displayed.

### Step 2: Rule out ERB rendering errors

The most common issue with SJR templates is malformed JavaScript code resulting from rendering unescaped HTML.
What makes this so troublesome is that it prevents any code from being executed (including `alert`s, log messages, etc.).
I like to rule out those kinds of errors by replacing all ERB `render` calls with static strings instead.

{% highlight js %}
$("ul#comments").appendOnto("<%= render(@comment) %>");

// becomes...

$("ul#comments").appendOnto("Hello world!");
{% endhighlight %}

This step alone won't reveal any of the errors in our template yet, 
but it will allow us to proceed with confidence that the JavaScript will be well-formed and executed properly. 

### Step 3: Identify errors using try-catch

As I mentioned when discussing the [different types of SJR template errors][sjr_errors_post], a good way to spot *JavaScript runtime errors*  is to wrap your template in a try-catch statement and log the error message of any exception which is caught.

{% highlight js %}
try {
  $("ul#comments").appendOnto("Hello world!");
} catch (e) {
  console.error(e);
}
{% endhighlight %}

With this in place, we can spot our first error in the example template. 

![SJR Debugging Guide - JavaScript Runtime Error](/assets/debugging_guide_javascript_runtime_error.png)
*Debugging console showing JavaScript runtime error*

The browser console informs us that `appendOnto` is not a valid function. 
The fix is simply to change it to `append`.
Once that's done, we'll see the string "Hello world!" get appended to the `ul` when the SJR template is run. 

### Step 4: Reintroduce ERB `render` calls

Now that we know the JS is working, we can put our original ERB `render` calls back in.

{% highlight js %}
try {
  $("ul#comments").append("<%= render(@comment) %>");
} catch (e) {
  console.error(e);
}
{% endhighlight %}

The template will fail, however we now know that it's due to a server error in Ruby.
This is the *Ruby runtime error* resulting from the undefined instance variable in the comment partial.

{% highlight erb %}
<!-- this partial fails if @post is not defined! -->
<li>
  <%= comment.content %>
  replied to <%= @post.title %>
</li>
{% endhighlight %}

We can fix by either (1) defining `@post` in the `CommentsController#create` action which renders this SJR template OR (2) replace the instance variable with a local variable. 

My recommendation is to [stop using instance variables in your partials][instance_var_in_partials_post].

### Step 5: Look for missing `escape_javascript` calls

At this point we've caught any Ruby and/or JavaScript runtime errors. 
The only error left preventing this template from executing properly is a *JavaScript syntax error* caused by malformed JS.

The way to debug this is to check each ERB `render` call to ensure that it is preceded by `escape_javascript` or `j`.

In our example template, we'll notice that the rendered HTML was *not* escaped properly, and we'll need to add that in. 

{% highlight js %}
$("ul#comments").append("<%= render(@comment) %>");

// becomes...

$("ul#comments").append("<%= escape_javascript render(@comment) %>");
{% endhighlight %}

With this fix in place, the template finally renders and runs properly in the browser.
Debugging success!

## Review the steps (and download the cheatsheet)

The following outline can serve as your new step-by-step process to use whenever you encounter an SJR template that isn't working.

1. Confirm template is being rendered using `alert`
1. Rule out ERB rendering issues by replacing with static strings
1. Wrap template content with a try-catch
1. Re-introduce ERB rendering calls and look for Ruby errors
1. Make sure every ERB render call is escaped properly

To download a PDF cheatsheet to use as a reference, just [drop your email below](#post_cta).

[sjr_errors_post]: {{ site.baseurl }}{% post_url 2016-11-20-3-reasons-why-your-sjr-template-isnt-working %}
[instance_var_in_partials_post]: {{ site.baseurl }}{% post_url 2016-09-19-stop-using-instance-variables-in-partials %}
