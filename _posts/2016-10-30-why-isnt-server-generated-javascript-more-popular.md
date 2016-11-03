---
layout: post
title: Why Isn't Server-Generated JavaScript More Popular?
---

Do you remember discovering how easy it was to reuse a partial from
JavaScript using a `js.erb` template and the `remote: true` option? Or
using the same technique to AJAX-ify a search with just a line or two
of JS?  When it happened for me, I thought to myself "this is
wonderful" and immediately fell even more in love with Rails.

**If you've experienced the power of server-generated JavaScript (SJR)
then you may also be puzzled by the fact that you almost never
hear about other people using it.** How could a concept so awesome be
seemingly ignored? Is there some critical flaw with it that isn't obvious?
Why isn't it more popular?

What's going on here...?

I believe that the apparent lack of support within the community for SJR
as a valid technique for adding JavaScript interactions to an app is
**more a perception rather than a reality**, mainly due to the current
popularity of client-side JavaScript frameworks.

Today, JS frameworks dominate the conversation, and that's not
surprising. For one thing, they are exciting and new. It wasn't until
recently that browsers became powerful enough to support full
client-side MVC, and these capabilities are still evolving and being
explored. SJR on the other hand is just a simple and boring (in
a good way) solution that's been around for years, first appearing in conjunction
with RJS and more recently with UJS patterns. [DHH described it](https://signalvnoise.com/posts/3697-server-generated-javascript-responses){:target="_blank"}
in detail way back in 2013.

In addition to this, projects such as React and Angular are backed by
Facebook and Google, huge companies with many developers and tons of
resources to develop and popularize their solutions. What developer
wouldn't be curious and confident about using the same framework in his/her own app
that Facebook and Google use in production? Rails, while hugely popular
in its own right, does not have the same kind of reach that these
companies do.

Lastly, it shouldn't be shocking to us that SJR gets a lot less air time
than other solutions because, whereas client-side JS frameworks are
back-end agnostic, SJR (as we know it) is only part of the Rails
ecosystem. This means that those JS frameworks have a much wider audience
of developers who write and talk about them companred to the smaller
audience of Rails developers using SJR.

**The truth is, there _are_ a lot of people using and writing about
SJR,**
even in an age ruled by the client-side JS framework. Here are just a
few:

- ["Diving Into jQuery UJS"](https://jonathanpike.net/2016/02/Diving-into-jQuery-UJS){:target="_blank"}
- ["Robust AJAX UIs with jQuery and Server-Generated JavaScript
  Responses"](http://hackernotes.io/2016/10/06/robust-ajax-uis-with-jquery-ujs-and-server-generated-javascript-responses/){:target="_blank"}
- ["Your Front End Framework is Overkill - Server Side Javascript w/ Rails"](https://www.youtube.com/watch?v=7YLYZQJZB0E){:target="_blank"}
- ["Server JavaScript: A Single-Page App To...A Single-Page App"](https://www.betterment.com/resources/inside-betterment/engineering/server-javascript-a-single-page-app-toa-single-page-app/){:target="_blank"} (<= this one is from Betterment, a giant financial software company)

Let me assure you that there are *many* out there who love SJR as much
as you do, myself included. Not only do we think SJR is a legitimate
technique for adding JS interaction to a web app, but we also believe that there
are many use cases for which its even more preferable than a full-blown
client-side MVC framework.

So have no more doubts... use SJR with confidence!

### Related Articles

- [When To Use `escape_javascript` in an SJR Template]({% link
  _posts/2016-10-04-when-to-use-escape-javascript-in-an-sjr-template.md %})
