---
layout: post
title: Error Messages For Your SJR Templates Using BetterSJR
tags: sjr
---

### Why does debugging SJR templates have to be so hard?

It's no secret that trying to figure out what's causing an SJR template to fail can be a nightmare. 
The main cause of this pain is simply the lack of error messages when something goes wrong.

When SJR templates fail, you are treated to deafening silence rather than the helpful Rails error explanations that you are accustomed to.
This means that you, the developer, have no idea where to look and what is to blame.

For all of the niceities that the Rails framework provides, it's clear that the SJR debugging experience is lacking.
So why does Rails leave us high-and-dry in this regard?
Shoudn't we expect something better?

### Simply showing error messages would help a lot.

What if, instead of failing silently, SJR templates caught and printed JavaScript errors to the console?
Not only would this tell you that the problem is definitely in the template (not elsewhere in the request cycle), 
but it also tells you exactly what kind of error has occurred.

With error messages guiding the way, identifying and fixing common JS runtime errors such as undefined variables or functions becomes much easier.

### Announcing BetterSJR

That's exactly what the BetterSJR gives you.

It prints error messages to the console of your browser's development tools.
It does this by automatically wrapping every SJR template in a try-catch statement and catching any JavaScript runtime errors which occur.

Here are some samples of the gem in action:

![BetterSJR Example - jQuery Not Loaded](/assets/better_sjr_example-jquery_not_loaded.png)
*Example #1 - jQuery isn't loaded*

![BetterSJR Example - Typo In Variable Name](/assets/better_sjr_example-variable_name_typo.png)
*Example #2 - Typo in a variable name*

Whereas before nothing at all would show up in the console, with BetterSJR there are now helpful log messages pointing you toward the source of your error(s).

### Try it out!

A better debugging experience is necessary in order to make SJR a more viable solution for simple JavaScript interactions.
That's what BetterSJR hopes to provide.

[Click here](https://github.com/carlosramireziii/better_sjr) to view the project's repository.
