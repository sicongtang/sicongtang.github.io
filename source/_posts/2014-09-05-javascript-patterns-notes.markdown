---
layout: post
title: "Javascript Patterns Notes"
date: 2014-09-05 17:20:45 +0800
comments: true
categories: javascript
---
This post aims to summarize the js patterns in Javascript Patterns book written by Stoyan-Stefanov. The category order is followed by the chapter of this book.
##1 Essentials
####1.1 Single var Pattern
{% gist 85d9fea488160c21d1a7 single_var_pattern.js %}
####1.2 Switch Pattern
{% gist 85d9fea488160c21d1a7 switch_pattern.js %}
##2 Literal Patterns
####2.1 Patterns for Enforcing new
If we forgot to add new, this.value will become global obejct.
{% gist 85d9fea488160c21d1a7 patterns_for_enforcing_new.js %}
####2.2 Self-Invoking Constructor
{% gist 85d9fea488160c21d1a7 self_invoking_constructor.js %}
####2.3 Error Objects
On the other hand, throw works with any object, not necessarily an object created with one of the error constructors, so you can opt in for throwing your own objects.
{% gist 85d9fea488160c21d1a7 error_objects.js %}
##3 API Patterns
####3.1 Callback Pattern
{% gist 85d9fea488160c21d1a7 callback_pattern.js %}
####3.2 Configuration Object Pattern
The configuration object pattern is a way to provide cleaner APIs, especially if you’re building a library or any other code that will be consumed by other programs.
{% gist 85d9fea488160c21d1a7 configuration_object_pattern.js %}
##4 Performance Patterns
####4.1 Self-Defining Functions
This pattern is useful when your function has some initial preparatory work to do and it needs to do it only once.

A drawback of the pattern is that any properties you’ve previously added to the original function will be lost when it redefines itself.
{% gist 85d9fea488160c21d1a7 self_defining_functions.js %}
####4.2 Memoization Pattern(Function Properties)
You can add custom properties to your functions at any time. One use case for custom properties is to cache the results (the return value) of a function, so the next time the function is called, it doesn’t have to redo potentially heavy computations. Caching the results of a function is also known as memoization.
{% gist 85d9fea488160c21d1a7 memoization_pattern.js %}
##5 Object Creation Patterns
####5.1 Prototypes and Privacy
{% gist 85d9fea488160c21d1a7 prototypes_and_privacy.js %}
####5.2 Evelation Pattern(revealing module pattern)
####5.3 Static Memebers
{% gist 85d9fea488160c21d1a7 public_static_members.js %}

{% gist 85d9fea488160c21d1a7 private_static_members.js %}
##6 Code Reuse Patterns
####6.1 The Default Pattern
####6.2 Rent a Constructor
####6.3 Rent and Set Prototype
{% gist 85d9fea488160c21d1a7 rent_and_set_prototype.js %}
####6.4 Share the Prototype
{% gist 85d9fea488160c21d1a7 share_the_prototype.js %}
####6.5 A Temporary Constructor
{% gist 85d9fea488160c21d1a7 a_temporary_constructor.js %}
####6.6 Mix-ins
{% gist 85d9fea488160c21d1a7 mixin_pattern.js %}

##7 Design Patterns

##References:
1. [Javascript Patterns](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752/ref=sr_1_1?ie=UTF8&qid=1410247009&sr=8-1)
