---
layout: post
title: Adding an Attachment Validator to Active Storage
---

As you know, Active Storage is the newest addition to Rails core. 
It provides a nice built-in solution for file uploading, 
and [my impressions are very positive so far][My Favorite Active Storage Features].
It has a nice set of features including image variants, direct uploads, support for major cloud providers, and more. 
All these make it surprisingly competitive with the existing file upload gems. 

But there's one feature which is conspicuously missing... attachment validation. 
There's no way to enforce that an attachment is present when you save a model!

Model validation, of course, is one of the fundamental features of Rails. 
It's also one of the things that made me fall in love with Rails in the early days. 
That's why it's surprising that Active Storage was released without it, 
especially since it's a feature of the other file upload gems. 
It's become one of the [top feature requests on the Rails repository][Rails validation issue]{:target="_blank"},
and many feel it's a major feature lapse. 

That said, it's pretty easy to add a custom validator to gain this missing functionality. 
Below I'll share the code that I use to validate my Active Storage attachments. 
I'll also include a handy RSpec matcher you can use in your unit tests. 

## Validate that a file is attached

This validation is straight-forward. 
It ensures that an attached file is present when a model is saved. 
It's like a `presence` validation but for your attachment. 

Here's some sample usage

{% highlight rb %}
class Person < ApplicationModel
   has_one_attached :file

   validates :file, attached: true
end
{% endhighlight %}

You'll find the code for the validator [here](https://gist.github.com/carlosramireziii/73f2c7b12dd6716482e41a3cd8e0a94d#file-attached_validator-rb){:target="_blank"}

## Write readable unit tests with this RSpec matcher

I usually use `shoulda-matchers` in my unit tests. 
It provides a quick way to test that validations have been added to your models. 
Below is an RSpec matcher that will work with our brand new attachment validator. 

Here's some sample usage

{% highlight rb %}
describe Person do
   it { is_expected.to validate_attachment_of(:file) }
end
{% endhighlight %}

You'll find the code for the matcher [here](https://gist.github.com/carlosramireziii/73f2c7b12dd6716482e41a3cd8e0a94d#file-validate_attachment_of-rb){:target="_blank"}

## Other validators coming soon

Another common validation for uploaded files is on the content-type. 
You'll usually want to restrict the file types that you allow the user to upload. 
For example, images should only be PNG, GIF, or JPEG. Another example is that documents should only be PDF or TXT. 

Active Storage does not have a built-in content-type validation, 
but we can [build one ourselves][Active Storage content-type validator post].


[My Favorite Active Storage Features]: {{ site.baseurl }}{% post_url 2018-08-27-my-favorite-active-storage-features %}
[Rails validation issue]: https://github.com/rails/rails/issues/31656
[Active Storage content-type validator post]: {{ site.baseurl }}{% post_url 2018-09-15-validating-the-content-type-of-active-storage-attachments %}
