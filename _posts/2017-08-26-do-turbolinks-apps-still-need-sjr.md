---
layout: post
title: Do Turbolinks Apps Still Need SJR?
tags: sjr
---

Have you ever wondered about the role of SJR in a Turbolinks-enabled app?
Turbolinks already handles so much out-of-the-box, 
why would you ever want to manually code a `js.erb` template?
In this case, it's hard to imagine a scenario that calls for a JS response, which raises the question...
**has Turbolinks replaced SJR?**

Nope, it hasn't.

Turbolinks and SJR are perfect complements to each other because **SJR can handle scenarios that Turbolinks cannot.**
Turbolinks is built to handle `GET` requests only;
it doesn't handle XHR `POST` requests,
which is exactly where SJR shines.

According to [the Turbolinks maintainer](https://github.com/turbolinks/turbolinks/issues/119#issuecomment-227221738){:target="\_blank"}:

> Turbolinks doesn't handle XHR form submission. If you want to update the page in response to an XHR, you can return a JavaScript string and evaluate it. Since you're using Rails with jQuery, the JavaScript response will be evaluated automatically.
> 
> This technique is known generally as "Server-generated JavaScript Responses" or SJR for short.

and [DHH](https://github.com/rails/rails/issues/25197#issue-157523625){:target="\_blank"}:

> When using Turbolinks, a normal redirect will generate a Turbolinks.visit() call, and otherwise there's SJR.

In summary, **Turbolinks will automatically handle your page-to-page navigation,**
whereas **SJR can be used to handle XHR form submissions.**

---

_My next article will discuss the specific use cases where you should consider using SJR in your Rails app.
[Subscribe below](#post_cta) so you don't miss it!_
