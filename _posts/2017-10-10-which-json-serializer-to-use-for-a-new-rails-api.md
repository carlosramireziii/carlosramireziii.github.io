---
layout: post
title: Which JSON Serializer To Use For A New Rails API?
---

There's nothing more exciting than starting a new project from scratch.

A fresh project is an opportunity to do things your own way.
It is a clean slate and isn't tied down by the decisions of the past.
You get a chance to use all the best gems and practices you've been reading about.
You can finally do things the _right_ way.

For a new Rails API app,
there's one thing that can kill your new project high...
**having to decide which JSON serializer to use.**

Whichever option you choose will be hard to change later,
so you want to make an good choice from the start. 
Unfortunately, 
when it comes to JSON serializers there are _a lot_ of options. 

Rails provides Jbuilder by default.
Yet every single tutorial out there seems to use ActiveModel::Serializers.
Nowadays JSONAPI::Resources seems to be gaining traction.
Roar and Rabl are also options, 
not to mention plain old `render :json`. 

Picking the best one means a lot of Googling.
Isn't Rails supposed to be all about reducing decision fatigue..? 

Knowing about each serializer option and when to use which one would reduce this stress. 
You'd be able to choose the one that best suited your project's needs and your own coding style. 

**Below you'll find a list of the most popular ones and a short review of each.**
Learn what your options are so you pick the right one for your next Rails API project.

## Jbuilder

This is the default option provided to you from Rails.
It provides a simple DSL for creating JSON responses from the view layer. 

### Reasons to choose Jbuilder

- You usually stick with the "Rails Way" and the tools in the default Rails stack
- You believe JSON representations of your data belong in the view layer
- JSON templates living side-by-side with HTML templates is appealing to you
- You are building task-driven UIs which do not map 1:1 with your database schema.
  For example, including unrelated models and/or non-model data in a single response.
- You are creating complex JSON objects and need fine-grained control over the structure
- You are comfortable using helper methods and partials to create nested JSON structures

## ActiveModel::Serializers

AMS is the option most used in tutorials and online resources.
Instead of using a DSL or view template, 
it uses dedicated serializer objects in your model layer.

### Reasons to choose AMS

- You believe the model (not view) layer should handle the serialization of your data into JSON
- Your user interface is data-driven and roughly corresponds with your DB schema
- You are creating simple JSON structures with model-name-based keys
- You don't need a lot of customization and will be fine with convention-over-configuration. 
  As per the project's creator,
  "Rails is built on conventions. AMS is built on conventions. RABL and JBuilder are built on customizing every last detail of the response."
- You are more comfortable using inheritance and composition to create reusable and/or complex JSON

## JSONAPI::Resources

This is an all-encompassing framework that allows you to create an API that adheres to the JSON:API spec.
You specify the definition of your resources and the gem handles the rest. 
Serialization, deserialization, pagination, linking, and more are all included.

### Reasons to choose JSONAPI::Resources

- You plan to follow the JSON:API spec _exactly_
- You want to write the least amount of code possible
- You are comfortable baking the gem into your routes, controllers, and serializer models
- You don't mind when large amounts of functionality come from code that lives in a framework, 
  not your project itself
- You know how to override framework defaults when you need non-standard functionality

## Rabl

Rabl is another view-centric DSL for generating JSON, XML, and other response formats.
It has been around for a while and is quite mature. 

### Reasons to choose Rabl

- You need to serialize into other formats besides JSON
- You believe the view (not model) layer should handle data representations
- You are using a non-Rails framework (e.g. Sinatra, Padrino, plain old Ruby)
- You want there to be [good documentation][Rabl Testing Examples]{:target="_blank"} for testing serialization

## Roar

Roar is part of the suite of gems that comprise the [Trailblazer framework][Trailblazer Homepage]{:target="_blank"}.
It's concerned with parsing and rendering documents.

### Reasons to choose Roar

- You are a fan of Trailblazer and/or its philosophy
- You favor tools which are modular and framework agnostic
- Deserialization is as important as serialization
- You are a client of your own API and want an easy way to consume it

**Consult this review of the many JSON serializer options next time you start a new Rails API project.**
You'll reduce the mental load of decision making,
and you'll be confident you made the right choice.

---

_For a more in-depth walkthrough on creating Rails APIs, 
check out [this post][Rails API Walkthrough]._

[Rabl Testing Examples]: https://github.com/nesquena/rabl/wiki/Testing-with-rspec
[Rails API Walkthrough]: {{ site.baseurl }}{% post_url 2016-10-26-a-complete-guide-to-building-a-rails-5-api %}
[Trailblazer Homepage]: http://trailblazer.to
