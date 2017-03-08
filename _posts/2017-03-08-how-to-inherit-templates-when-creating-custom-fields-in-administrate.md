---
layout: post
title: How To Inherit Templates When Creating Custom Fields in Administrate
form_id: 72544570
---

## The recommended approach to extend default field behavior is to create a custom field

Administrate is built to have smart defaults,
but sometimes you need to modify the standard behavior to better suit your project.

Because each field type is an object, 
customizing behavior is as simple as creating a new field object.
To make it even easier, 
you can inherit from an existing field to use all of its behavior except where you want it changed.

For example, here's a modified `BelongsTo` field which adds a sort order to the collection.

{% highlight ruby %}
# app/fields/belongs_to_ordered_field.rb
class BelongsToOrderedField < Administrate::Field::BelongsTo
  def candidate_resources
    associated_class.all.order(:name)
  end
end
{% endhighlight %}

Easy, right...?

## Custom fields don't inherit templates as you would expect!

If you've ever tried this method, 
you were probably surprised and disappointed to realize that views don't automatically inherit from the parent field.
Even though you want the templates to be exactly the same, you are forced to create them.
This means copying the original views into your app.

This feels like unnecessary work and duplication, 
especially to change just one simple method.
You shouldn't have to copy/paste templates when you are inheriting from an existing field...
Custom fields should just inherit views from their parent.

## You can enable template inheritance using `to_partial_path`

A simple way to get the inheritance behavior you would expect for a custom field is to use `to_partial_path`. 

Overriding this method allows you to change where Rails looks for the templates associated with that field.
Set it to the partial path of the parent field and you'll get exactly the behavior you were expecting all along.

{% highlight ruby %}
class BelongsToOrderedField < Administrate::Field::BelongsTo
  # ...

  # tell this field to use the views of the `BelongsTo` parent class
  def to_partial_path
  "/fields/belongs_to/#{page}"
  end
end
{% endhighlight %}

Using this strategy you can feel free to create as many custom fields as you need without worrying about copying and maintaining all those templates.
