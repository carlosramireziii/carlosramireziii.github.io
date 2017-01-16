---
layout: post
title: A Cleaner Way To Set Multiple Conditional CSS Classes for 'link_to' 
---

## The commonly accepted practices are not good

Isn't it frustrating that there are _no_ elegant ways to conditionally append CSS class names to the `:class` option of a `link_to` helper call?

The code to do the conditionals clutters up the views, especially when there's more than one.

Everyone suggests a slightly different syntax, but which one is best? 
They all look messy and require logic in the view layer.

For such a common task, why isn't there a better solution?

## Finding inspiration in `React`

Facebook's `React` front-end framework has an add-on called `classNames` (formerly known as `classSet`) which offers a solution to this problem.

Given a Hash with key/value pairs consisting of CSS class name strings and boolean values, the `classNames` function returns a string of classes where the boolean value evaluated to `true`.

{%highlight JavaScript %}
classNames({ nav: true, active: false, hidden: someTruthyVar })
// => "nav hidden"
{% endhighlight %}

Wouldn't it be nice to have a similar method in Rails?

## Here is a better solution for Rails

The [`css-class-string`](https://github.com/nLight/css-class-string) gem provides just such a helper for our Rails views. 

Here's an example of a `link_to` which requires multiple conditional classes to be appended:

{% highlight erb %}
<%= link_to "Hello", root_path, class: "nav #{'active' if @some_boolean} #{@another_boolean ? 'hidden' : 'visible'" %>
{% endhighlight %}

Now see how it looks using the `class_string` helper method:

{% highlight erb %}
<%= link_to "Hello", root_path, class: class_string("nav", active: @some_boolean, [:hidden, :visible] => @another_boolean) %>
{% endhighlight %}

Although the code is not much shorter, removing the string interpolation makes it easier to read, and there is no more logic inside the view which is always a good practice.

The same helper can be used in any Rails tag helper which expects a `:class` option (e.g. `content_for`). 

## Don't want to use the gem? Implement it yourself

The gem has some useful advanced features such as using bare arguments for default classes and arrays for one-or-the-other classes,
however if you don't want to add another gem to your `Gemfile` then you can create a very simple version of the helper yourself.

{% highlight ruby %}
# Conditionally joins a set of CSS class names into a single string
# 
# @param css_map [Hash] a mapping of CSS class names to boolean values
# @return [String] a combined string of CSS classes where the value was true
def class_string(css_map)
  class_string = ""

  css_map.each do |css, bool|
    class_string << css if bool
  end
  
  class_string
end
{% endhighlight %}

Or if you prefer one-liners:

{% highlight ruby %}
def class_string(css_map)
  css_map.find_all(&:last).map(&:first).join(" ")
end
{% endhighlight %}
