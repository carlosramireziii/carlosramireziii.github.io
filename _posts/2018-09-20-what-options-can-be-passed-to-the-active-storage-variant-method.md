---
layout: post
title: What Options Can Be Passed to the Active Storage variant Method?
---

When Rails adds a big new feature it takes some time before good Internet resources are available for it.

This is currently the case for Active Storage...
The documentation is still pretty sparse.
There aren’t many blog articles, tutorials, or Stack Overflow answers yet.
Learning about how it works requires searching through the GitHub issue tracker or reading through the source code itself. 

This was a real challenge when learning how to use the `variant` method for image transformations. 

I had lots of questions...
What’s the proper format for specifying width and height?
What do those special symbols mean?
What is the difference between `"50x50^"` vs. `"50x50<"` vs. `"50x50>"`?
Are there any other useful options besides `resize`? 

I spent a lot of time blindly trying different combinations of parameters and hoping for the best.
Through trial and error I was finally able to get the hang of it. 

To save you from the same painful experience, 
I'm going to share with you the resources and tips I discovered along the way.

Armed with these you should be able to tackle any image transformation task that you encounter.

## The `variant` method accepts any valid ImageMagick flag

The `variant` method will accept any transformation supported by the image processor adapter.
In the case of ImageMagick, that is any flag which you would pass to its `convert` method. 

But how do we know what those are..? 

Here’s a great resource which lists out every single transformation option possible:
[https://www.imagemagick.org/script/command-line-options.php][ImageMagick documentation]{:target="_blank"}

There are a LOT of options, but the ones I use most are `resize`, `auto-orient`, `extent`, `background`, and `gravity`. 

## Some flags take arguments, others don't

You’ll notice that some of the ImageMagick options need a parameter.
For example, `resize` requires a "geometry" (which is a way of specifying height and width).
Others, such as `auto-orient`, don’t need a parameter. 

If you don’t already know, all options are passed to `variant` via a `Hash`. 

For options which *need a parameter*, the key is the name of the flag and the value is its parameter.

{% highlight rb %}
my_attachment.variant(resize: "100x100")
{% endhighlight %}

For options which *don’t need a parameter*, 
the key is also the name of the flag, 
but the value should be set to `true`.

{% highlight rb %}
my_attachment.variant(auto_orient: true)
{% endhighlight %}

## Advanced transformations may need `combine_options`

By default, the image transformations passed to `variant` are applied sequentially.
But some advanced transformations need to occur at the same time.
For example, an image with cropping + centering + colored whitespace background. 

In cases like this, you’ll need to use `combine_options`. 

The following example resizes an image such that it’s no larger than 400x300 and fills in any whitespace with a grey background color. 

{% highlight rb %}
my_attachment.variant(combine_options: { resize: "400x300>", extent: "400x300", background: "grey", gravity: "center"})
{% endhighlight %}

I use this technique for grids of user uploaded images.
It makes all the images exactly the same size, even if the originals had different aspect ratios.

## Miscellaneous wisdom

A couple more tips:

- If your images (especially iOS selfies) are showing up sideways, use `auto-orient` to fix it
- The following documentation describes the different geometry options and symbols for `resize`: [https://www.imagemagick.org/Usage/resize][Resize documentation]{:target="_blank"}

## You're ready to handle any transformation now

One of [my favorite features of Active Storage][favorite Active Storage features post] is transforming images with the `variant` method. 
Once you get the hang of all the options, 
you’ll see that it’s quite powerful and easy-to-use. 

I hope these tips save you some time and help you understand how it works. 
Make sure you bookmark the documentation, 
and try out as many options as you can to find the useful ones.

Let me know what you find in the comments below!

---

_Active Storage makes it so easy to upload files and attach them to your Rails model._
_There’s even a way to do direct uploads with only a single line of code._

_Rails has you covered for the server side portion of file uploads, but what about the front-end..?_
_Are you stuck with those super basic (not to mention ugly) browser file input fields?_
_What if you want to use Uppy, Dropzone, or any other JavaScript plugin?_

_The good news is YOU CAN,_
_but there isn’t that much documentation on doing it yet._
_**In my next series of posts I’ll show you how to integrate your favorite front-end plugins to upgrade your file upload experience.**_

_Don't miss it... [Subscribe below](#post_cta)_

[favorite Active Storage features post]: {{ site.baseurl }}{% post_url 2018-08-27-my-favorite-active-storage-features %}
[ImageMagick documentation]: https://www.imagemagick.org/script/command-line-options.php
[Resize documentation]: https://www.imagemagick.org/Usage/resize
