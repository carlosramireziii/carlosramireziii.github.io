---
layout: post
title: A Cleaner Way To Set Multiple Conditional CSS Classes for 'link_to' 
---

## The commonly accepted practices are not that good

Isn't it frustrating that there are _no_ elegant ways to conditionally append CSS class names to the `:class` option of a `link_to` helper call?

The code to do the conditionals is ugly and clutters up the views, especially when there's more than one.

Everyone suggests a slightly different syntax, but which one is best? 
They all look messy and require putting logic in the view layer.

For such a common task, why isn't there a better solution?

## Turning to `React` for inspiration

Facebook's `React` front-end framework has an add-on called `classNames` (formerly known as `classSet`) which offers a solution to this problem.

Given a hash with key/value pairs consisting of CSS class name strings and boolean values, 
the `classNames` function returns a string of classes where the boolean value evaluated to `true`.

{%highlight JavaScript %}
classNames({ nav: true, active: false, hidden: someTruthyVar })
// => "nav hidden"
{% endhighlight %}

Wouldn't it be nice to have a similar method in Rails?

## Finally, a better solution for Rails

The [`css-class-string`](https://github.com/nLight/css-class-string) gem provides just such a helper for our Rails views. 

Here's an example of a `link_to` which requires multiple conditional classes to be appended:

{% highlight erb %}
<%= link_to "Hello", root_path, class: "nav #{'active' if @some_boolean} #{@another_boolean ? 'hidden' : 'visible'" %>
{% endhighlight %}

Now here is the same example using the `class_string` helper method:

{% highlight erb %}
<%= link_to "Hello", root_path, class: class_string("nav", active: @some_boolean, [:hidden, :visible] => @another_boolean) %>
{% endhighlight %}

Although the code is about the same length, removing the string interpolation makes it **easier to read**, 
and there is **no more logic inside the view** which is a better practice.

The same helper can be used in any Rails tag helper which expects a `:class` option (e.g. `content_for`). 

## Don't want to use the gem? Implement it yourself

The `css-class-string` gem has some useful advanced features such as using bare arguments for default classes and arrays for "one-or-the-other" classes,
however if you don't want to add another gem to your `Gemfile` then you can create a very basic version of the helper yourself.

{% highlight ruby %}
# Conditionally joins a set of CSS class names into a single string
# 
# @param css_map [Hash] a mapping of CSS class names to boolean values
# @return [String] a combined string of CSS classes where the value was true
def class_string(css_map)
  classes = []

  css_map.each do |css, bool|
    classes << css if bool
  end
  
  classes.join(" ")
end
{% endhighlight %}

Or if you prefer one-liners:

{% highlight ruby %}
def class_string(css_map)
  css_map.find_all(&:last).map(&:first).join(" ")
end
{% endhighlight %}

I'll be adding `css-class-string` or a helper like above to all of my projects moving forward.
