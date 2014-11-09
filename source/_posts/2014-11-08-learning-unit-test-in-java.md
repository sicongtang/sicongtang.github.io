---
layout: post
title: learning unit test in java
date: 2014-11-08 21:44:02
categories: java
tags:
---
The post aims to share my some thoughts of how to learn unit test in java, I will give a little bit more insights on how I am thinking on this. Hope this will bring you more inspirations.

There are two popular testing philosophys a.k.a TDD(Test-Driven Development) & BDD(Behaviour-Driven Development), I started to search some good frameworks with BDD. I knew about BDD from javascipt testing framework, e.g. Mocha, Jasmine, so I am pretty eager to know whether there are any popular BDD frameworks in Java perspective. After reading couples of posts, [JBehave](https://github.com/jbehave/jbehave-core) and [Cucumber](https://github.com/cucumber/cucumber) are relatively welcomed in the community. Cucumber works with Ruby, Java, .NET, Flex or web applications written in any language. It has been translated to over 40 spoken languages, and gains more stars on github. Moreiver, TDD is much more well-known for most Java developer like JUnit, TestNG etc.

<!--more-->

Since I am also not quite familiar with JUnit testing, I begin with offical [documentation](http://junit.org/) and [examples](https://github.com/junit-team/junit/wiki/Assertions) hosted by github wiki pages, which gives me a glimpse about basic JUnit usage and idioms. Then you will find [Hamcrest CoreMatchers](https://github.com/hamcrest/JavaHamcrest) lib that you can easily invoke assertThat instead of assertTure etc.

Moreover, I search more stars projects on github, you can checkout [elasticsearch](https://github.com/elasticsearch/elasticsearch) that will give you a more practical example of how to write code with JUnit test. On the other hand, [RxJava](https://github.com/ReactiveX/RxJava) use [mockito](https://github.com/mockito/mockito) instead of hamcrest. Mockito is a Java mocking that is dominated by expect-run-verify libraries like EasyMock or jMock.

Eventually I am going to start using basic JUnit test with mockito mocking, which looks slim and compact.


