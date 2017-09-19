---
layout: post
title: When Should I Use SJR?
tags: sjr
related_posts: false
---

Rails encourages you to submit forms using AJAX by making it the new [default option][DHH form_with proposal]{:target="\_blank"}.
It does this by setting `remote: true` on forms created with the `form_with` helper.

This means SJR is about to get [a lot more popular][Why Isn't SJR More Popular?]{:target="\_blank"}, 
[even in Turblinks-enabled apps][SJR with Turbolinks?]{:target="\_blank"}.

With that in mind, 
let's review some common use cases for SJR in a Rails app. 

## Displaying form errors

Turbolinks already handles redirecting after a successful form submission for you.
Forms with validation errors are not supported ([yet][Turbolinks support for form errors]).

To display errors, re-render your form using SJR.
It will have the appearance of client-side validations with no JavaScript required.

## Simple actions

Adding a comment, liking a post, toggling an option, etc. are all great use cases for SJR. 

The user can perform the action and keep using the app without the interruption of a page refresh. 

## Displaying confirmation messages

Let the user know that his/her action was successful by showing a message on the page without refreshing it. 

This works well for contact forms, simple actions (like the ones above), etc. 

## Deleting records in a CRUD interface

We've all built admin panels with a table of records and associated CRUD actions.
A non-SJR delete action will cause a full page reload and cause the user to lose his/her place on the page.

## Generate and open modal windows dynamically

You can use SJR to create and open modals on-the-fly instead of including the markup on the initial page.
This is ideal for situations when you have to generate a modal for a collection of records. 

This technique does result in more requests hitting the server, so use caution. 

---

Turbolinks and a splash of SJR can give your app the feel of an SPA without the overhead of a front-end framework. 

_Brush up on your [SJR template debugging skills][Debugging SJR]{:target="\_blank"}, and_
_[subscribe to my list](#post_cta) more articles on SJR and Rails._

[DHH form_with proposal]: https://github.com/rails/rails/issues/25197
[Why Isn't SJR More Popular?]: {{ site.baseurl }}{% post_url 2016-10-30-why-isnt-server-generated-javascript-more-popular %}
[SJR with Turbolinks?]: {{ site.baseurl }}{% post_url 2017-08-26-do-turbolinks-apps-still-need-sjr %}
[Turbolinks support for form errors]: https://github.com/turbolinks/turbolinks/issues/85
[Debugging SJR]: {{ site.baseurl }}{% post_url 2016-11-30-a-step-by-step-guide-to-debugging-sjr-templates %}
