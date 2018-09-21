---
layout: post
title: The Best Features of Active Storage
---

The headlining feature of Rails 5.2 is a built-in file upload solution called Active Storage.

There have always been excellent third-party libraries to handle uploading files.
Paperclip, CarrierWave, Refile, Shrine are a few of the most popular ones,
and they have served us well.
When a project called for user uploaded content, you didn't sweat it.
You popped one of those gems into the `Gemfile` and you were on your way.

With so many well-known and mature options already out there, you may be wondering if Active Storage is worth trying on your next new Rails project... 

To help you decide, here are a few of my favorite features of the newest addition to the Rails core. 

## Create exactly the image you want on-the-fly

Active Storage has made doing image transforms simple and flexible. 

Want a thumbnail image? 
Easy.

Need to dynamically resize and crop a photo? 
Done and done. 

How about getting a set of images with completely different aspect ratios to each be same fixed size? 
That’s covered too!

That last one was the most impressive for me. 
I am finally able to solve the age-old issue of user uploaded images having inconsistent sizes and not lining up in my grid layouts. 

All it takes is a few simple options in the `variant` method. 
In fact, the full power of MiniMagick is available to you ([which can be a bit overwhelming at first][Active Storage variant post]).

Rails creates these image variants on-the-fly.
This means you can change your thumbnail images from 75px to 100px without having to run a script to re-process all your existing images.
This is handy when you are fast prototyping and aren’t quite sure what your image size requirements are yet. 

Rails even caches the variants so that processing only happens the first time it’s generated.
You won't have to worry about performance.

## No database migrations needed!

Behind-the-scenes, all Active Storage attachments are modeled using a polymorphic association.

Why does that matter?
Because when you want to add a new attachment to a model, you only have to add the `has_one/many_attached` macro and you're done.
There are no database columns to add or migrations to run. 

You can add as many named attachments to a model as you want.
All file metadata gets stored on the Attachment class too. 

## Direct uploads? Not a problem

Want to upload files from the client directly to a cloud provider? 
Active Storage comes with built-in direct upload functionality. 

Enabling this functionality is as simple as including the JS file and setting `direct_upload: true` on the file input.
In true Rails fashion, it Just Works &trade;

Rails even provides a set of JavaScript events to hook into.
You can use them to customize the user experience, e.g. adding a progress bar, integrating a front-end plugin, etc.

## Go ahead and try it in your next project

Despite its official release being only a few months ago, 
I have been using Active Storage in client work and side projects for almost a year. 

I was initially skeptical about how Rails would improve on existing file upload gems.
It turns out, they knocked it out of the park!

Active Storage is so easy to use.
Creating variants is a pleasure.
No migrations = less work.
Direct uploads with a single line of code... yes please! 
Plus, I always prefer to use built-in Rails functionality because I know it will be well-integrated with the rest of the framework

My only gripe is that it’s still missing an [attachment presence validator][Attachment validator] and a [content-type validator][Content-type validator].
Even so, I highly recommend giving Active Storage a try in your next project.

[Attachment validator]: {{ site.baseurl }}{% post_url 2018-09-03-adding-an-attachment-validator-to-active-storage %}
[Content-type validator]: {{ site.baseurl }}{% post_url 2018-09-15-validating-the-content-type-of-active-storage-attachments %}
[Active Storage variant post]: {{ site.baseurl }}{% post_url 2018-09-20-what-options-can-be-passed-to-the-active-storage-variant-method %}
