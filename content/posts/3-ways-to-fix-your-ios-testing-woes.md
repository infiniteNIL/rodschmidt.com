---
title: '3 Ways To Fix Your iOS Testing Woes'
date: Mon, 03 Oct 2016 16:55:28 +0000
draft: false
tags: ['Apple Platforms', 'Cocoa Touch', 'Software Development', 'Testing']
---

Lots of companies have constant problems testing their iOS apps. Here are some ways to fix or ease them.

1\. Don’t Make Developers Run or Write UI Tests
===============================================

UI tests are black box tests and test the app from the perspective of the user. Your developers are the worst choice to test the app from this viewpoint and you need a fresh set of eyes for those tests. The QA engineer’s job is to test the app from the user’s viewpoint. Your QA Engineers should write these tests. Plus, UI tests run entirely too slow for your developers to run them all the time and are much too failure prone. Don’t waste their time by making them write and run these tests.

2\. Use Test Driven Design (TDD)
================================

By having your developers write there tests first and use the TDD approach, you’ll end up with more test coverage and you’ll have tests right from the get go. You’ll have a better design and better code as well.

3\. Only Test One Unit or Thing at a Time
=========================================

Unit tests are just that. They test a unit or one thing. Don’t test everything it interacts with as well. Mock out everything the unit depends on. Your tests will run faster and your developers can use them for constant and immediate feedback. It’s also easier to create specific scenarios that you want to test.