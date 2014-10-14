---
layout: post
title: getting started with full-stack javascript
date: 2014-10-13 02:46:32
categories: javascript
tags:
---
Since I was enlighted by Minix and started to learn nodejs at the beginning of the June, I have realized that full-stack javascript might be dominating the web framework instead of rails or J2EE. I started to investigate any alternative framework in this new tech concept.

In my initial project with full-stack javascript, requirejs/backbonejs/express/mongodb were selected. Backbone is lightweight frontend js mvc framework which has fewer learning curve and unopinionated, comparing with angularjs. Additionly, Express is the most common mvc server-side framework in nodejs. Moreover, mongodb is relatively friendly to nodejs json format in server-side code as well as command-line shell. Thus, the whole brand new tech stack has been determined.

Before we commerced to scanffold our project using [yeoman-generator](https://github.com/sicongtang/generator-marionette), we took a glimpse on tutorial i.e. [Developing Backbone.js Applications by Addy Osmani](http://www.amazon.com/Developing-Backbone-js-Applications-Addy-Osmani/dp/1449328253/ref=sr_1_2?ie=UTF8&qid=1413166509&sr=8-2&keywords=backbone), [Web Development with Node and Express](http://www.amazon.com/Web-Development-Node-Express-Leveraging/dp/1491949309/ref=sr_1_1?ie=UTF8&qid=1413166609&sr=8-1&keywords=node+express), in order to be familiar with mvc conecpt, restful api, and middileware in express. Besides, I also scan tricks in requirejs/bower/node package management in order to build a highly-maintainable project with a mainifest dependency.

<!--more-->

List difficulties that I encountered:
* Requirejs is extremely hard to config due to the fact that few js lib support AMD-style. I made serveral mistakes e.g. wrong varible declaration and backgrid with AMD support version doesn't upload in the bower. If more than two issues that occurred simultaneously, this will make complexity of trouble-shooting increase exponentially.
* While we are difining or requiring a AMD-module in requirejs, it is really subtle to deal with `baseUrl` property in `require.config()`.
* Body-parser is a middleware in express, which has been separated into independent npm module in express 4.x. You need to take care how it parse the http body with different MIME type.
* Check session auth function, `next()` must be added in if/else block due to asynchronized programming
* Backgrid newly added AMD support, which vitraully didn't upload to the bower
* R.js optimization on the whole project doesn't handle very well on min.js, you need to declare the dev-version javascript file path.
* npm set index as a default while not specifying the filename like `var routes = require('./handlers');`
