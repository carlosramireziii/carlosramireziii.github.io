---
layout: post
title: Validating the Content-Type of Active Storage Attachments
---

Active Storage has the kind of easy-to-use features you'd expect from a Rails core library. 
Image variants, direct uploads, even JavaScript hooks are among [my favorites][favorite Active Storage features post]. 

But so far [Rails has not added the ability to add validations to file attachments][Rails validation issue]{:target="_blank"}. 
Requiring that an attachment be present or have a particular content-type is essential. 
It looks like [they are working on it][Rails validation PR]{:target="_blank"} for Rails 6, 
but what do we do in the meantime..? 

Luckily it's pretty easy to add custom validators to add in the missing functionality!

I've already shown [how to add the attachment presence validator][attachment validator post] to your Rails apps. 
Now I'll show how to add a validator that restricts an attachment's content-type. 
You can use this, for example, to ensure that your users only upload PNG, JPEG, or GIF files for their profile images. 
Or that documents are only PDF or DOC format. 

I'll also include a handy RSpec matcher that I use in my unit tests to test the validation. 

## Restricting the content-type of an attachment

The validation accepts an array of content-type strings which serves as an allow list.
Add this to your model and attachments will only be valid if its file type is in the list. 

{% highlight rb %}
class Person < ApplicationModel   
  has_one_attached :avatar

  validates :avatar, content_type: ["image/png", "image/jpeg"]
end
{% endhighlight %}

You'll find the code for the validator [here](https://gist.github.com/carlosramireziii/6d0ca6b414d8a6af08371c30ba4dedcd#file-content_type_validator-rb){:target="_blank"}

## Test allowed/denied content-types using this RSpec matcher

I use `shoulda-matchers` in my unit tests for a quick sanity check on my validations.
Below is an RSpec matcher modeled after the `allow_format` matcher that tests `validate_format_of`.
You can pass it a set of allowable content-types that should be valid.
You can also pass it a set of disallowed content-types that shouldn't be valid. 

{% highlight ruby %}
describe Person do
  it { is_expected.to allow_content_types("image/png", "image/jpeg").for(:avatar) }
  it { is_expected.not_to allow_content_type("image/gif").for(:avatar) }
end
{% endhighlight %}

You'll find the code for the matcher [here](https://gist.github.com/carlosramireziii/6d0ca6b414d8a6af08371c30ba4dedcd#file-allow_content_type-rb){:target="_blank"}

## What other validators are missing from Active Storage?

So far the main validations I've missed from Active Storage are for presence and content-type.
Which ones do you wish you had for your own work?

Share your thoughts in the comments below!

---

_One of my favorite features of Active Storage is the `variant` method._
_Not only is it super easy to use, but it's also very powerful._
_You can create image transformations using any option that's supported by ImageMagick._

_Unfortunately, I've found the documentation for these options isn't that accessible..._

_In my next post I'll be discussing [how options are passed to the `variant` method][Active Storage variant post]._
_I'll share the best online resources and documentation to take the mystery out of what transformations are possible and how to achieve them._
_You'll also learn how to fix upside down images, how to deal with images with different aspect ratios, and more._

**Read it here**: _[What Options Can Be Passed to the Active Storage `variant` Method?][Active Storage variant post]_

[attachment validator post]: {{ site.baseurl }}{% post_url 2018-09-03-adding-an-attachment-validator-to-active-storage %}
[favorite Active Storage features post]: {{ site.baseurl }}{% post_url 2018-08-27-my-favorite-active-storage-features %}
[Active Storage variant post]: {{ site.baseurl }}{% post_url 2018-09-20-what-options-can-be-passed-to-the-active-storage-variant-method %}
[Rails validation issue]: https://github.com/rails/rails/issues/31656
[Rails validation PR]: https://github.com/rails/rails/pull/33303
