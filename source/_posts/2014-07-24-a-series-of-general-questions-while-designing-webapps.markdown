---
layout: post
title: "A series of general questions while designing webapps"
date: 2014-07-24 15:36:16 +0800
comments: true
categories: web
---
In this post, I am going to list a series of questions as well as answers in order to figure out why contemporary webapps are designed and built like that.
####Why single page applications?
* The web is currently trending such that all data/content will be exposed through an API.
####Why backbonejs?
* It is essentially MVC for the client and allows you to make your code modular.
* Backbone.js is not strictly for single-age apps (SPA), but the strength of Backbone.js is in the SPA realm.
* Basically, backbone works best to untangle elaborate chains of rendering logic and model updating. If you just need to select and manipulate DOM elements, it's overkill. jQuery alone can handle that.

####Why AMD(RequireJS)?
Without RequreJS

* Browsers are limited in how many parallel requests they can make, so it can only do a certain number at a time.
* Scripts loaded synchronously. This means that the browser cannot continue page rendering while the script is loading.

With RequireJS

* Loading the scripts asynchronously.
* Ensuring that modules with dependencies are loaded in the right order. 
* You can use the optimizer to combine the JavaScript files together and minify it.
####Which is preferred about i18n? Client side or server side?




