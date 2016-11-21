---
layout: post
title: 3 Reasons Why Your SJR Template Isn't Working
---

When you are dealing with an SJR template that just isn't working it can be the most frustrating thing in the world.
It can seem like no matter what you put into that js.erb file, the resulting JS shows no life whatsoever. 
You spend days trying to figure out the problem, and no solutions are working.
Worst of all, there are usually no error messages to guide your debugging or make sense of the behavior you are seeing. 
Sometimes you get to the point where it just isn't worth it anymore, and you have to give up and move on. 

Luckily most issues in an SJR template fall into one of three categories. 
Once you learn to identify the *type* of error you are dealing with, debugging becomes much simpler.

Learn about each one below never feel helpless when encountering a broken SJR template ever again! 

## 1. Errors in your Ruby code

**Ruby Runtime Errors** are ones that you are already familiar with from your other Ruby code and are the easiest to spot and debug.

When embedded Ruby (found within `<%= %>` tags when using ERB) causes one of these errors, the entire request will fail.
This turns out to be a good thing because Rails "fails loudly" and lets you know all about it in a couple of ways. 

First, the **Rails server log** will print an error message along with a stack trace that points to the offending line in the SJR template.

You can also spot the error by using your web browser's **developer debugging tools**. 
There will typically be an alert or error message in the console that lets you know that the result of the AJAX request was a failure (500 server error).
You can even inspect the body of the response in the XHRs section and will show you the standard Rails error page. 

![Rails SJR Ruby Runtime Error - Developer Debugging Console](/assets/rails_sjr_ruby_runtime_error.png)
*Error message in browser debugging console*

![Rails SJR Ruby Runtime Error - Response Body](/assets/rails_sjr_ruby_runtime_error_2.png)
*Response body showing Ruby error*

## 2. Errors in your JavaScript code

Just like Ruby runtime errors, you can also have **JavaScript runtime errors** in your SJR templates. 
Unlike their Ruby counterparts, however, these are much harder to spot because they do not result in server errors or console alerts by default.

This means that when an error like this occurs, nothing will happen.
The AJAX request will receive a 200 success response and no errors or warnings will appear in the developer debugging tools.
Everything will look like it worked, except your template's JavaScript will not run.

When you think you are dealing with this kind of error, a useful trick to get more info is to **wrap your JavaScript code in a try-catch block**. 
This allows you to catch exceptions thrown by the JS code and print the error message into the console.

    try { 
      // SJR template code goes here 
    } catch(e) { 
      console.error(e); 
    }

Now instead of just failing silently, the browser debugging tools will show helpful error messages and stacktraces just like our Ruby runtime errors!

![Rails SJR JavaScript Runtime Error - Try-Catch in Console](/assets/rails_sjr_javascript_runtime_error.png)
*JavaScript exception error message in browser debugging console*

## 3. Malformed JavaScript code

**JavaScript syntax errors** occur when your code is not compiled correctly. 
These are the hardest to spot because not only do they fail silently by default, but wrapping them in a try-catch block like above won't work because the code itself is invalid and therefore won't even be executed.  

If you've eliminated runtime errors as the cause of your broken SJR template, see if it's a syntax error instead by trying the following:

* **Check the response body in the debugging tools**. 
  Does it look syntactically correct? Does it have proper syntax highlighting and spacing? 
  
  ![Rails SJR JavaScript Syntax Error - Response Body](/assets/rails_sjr_javascript_syntax_error.png)
  *Response body showing malformed JavaScript*

* **Replace embedded Ruby with static strings**.
  
  ```
  // original using ERB
  $("#foo").html("<%= ... %>");

  // replace with...
  $("#foo").html("Debugging test");
  ```
This will help you identify if your ERB calls are the culprit. 

---

Because of their unexpected behavior and lack of error messages, broken SJR templates can be one of the hardest things to debug in Rails.
But being able to distinguish between **runtime errors in Ruby or JavaScript** or **syntax errors in JavaScript** will make debugging SJR templates a breeze!

### Related Articles

- [Why Isn't Server-Generated JavaScript More Popular?]({% link
  _posts/2016-10-04-when-to-use-escape-javascript-in-an-sjr-template.md %})
- [When To Use `escape_javascript` in an SJR Template]({% link
  _posts/2016-10-04-when-to-use-escape-javascript-in-an-sjr-template.md %})
