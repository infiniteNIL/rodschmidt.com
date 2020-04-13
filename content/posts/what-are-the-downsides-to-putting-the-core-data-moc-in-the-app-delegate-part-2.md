---
title: 'Part 2: What Are The Downsides to Putting the Core Data MOC in the App Delegate'
date: Tue, 18 Oct 2016 22:33:49 +0000
draft: false
tags: ['Apple Platforms', 'Software Development']
---

In my previous [post](https://rodschmidt.com/what-are-the-downsides-to-putting-the-core-data-moc-in-the-app-delegate/), I gave some reasons why putting the Core Data MOC in your app delegate was a bad idea. Those reasons were:

1.  The app delegate is managing the Core Data stack. Classes should only have one responsibility. The app delegate is already responsible for managing application lifecycle. It shouldn’t be managing the Core Data stack as well.
2.  You are completely dependent on Core Data and using it as your persistence method for your app. What if you decide to switch to Realm or something?
3.  Any code you write that uses `myManagedObjectContext` will be dependent on the App Delegate.
4.  Any tests you write will be dependent on the App Delegate and Core Data and will be hard to test and slow as a result.

In the previous [post](https://rodschmidt.com/what-are-the-downsides-to-putting-the-core-data-moc-in-the-app-delegate/), we talked about how putting the Core Data MOC in your app delegate makes your app dependent on Core Data as your persistence mechanism and makes it hard to use something else. In this post, I’d like to talk about reason #1: That your app delegate is already responsible for managing the application life cycle and it shouldn’t be responsible for the Core Data stack as well. This comes from the [SOLID](https://en.wikipedia.org/wiki/SOLID_(object-oriented_design)) principles of good object-oriented design, specifically the [Single Responsibility Principle](https://en.wikipedia.org/wiki/Single_responsibility_principle), which states:

> Every module or class should have responsibility over a single part of the functionality provided by the software, and that responsibility should be entirely encapsulated by the class. All of its services should be narrowly aligned with that responsibility.

Or as Robert C. Martin (Uncle Bob) puts it, “_A class should have only one reason to change._” The documentation for `UIApplicationDelegate` states:

> The UIApplicationDelegate protocol defines methods that are called by the singleton UIApplication object in response to important events in the lifetime of your app.

So the app delegate is already handling the application’s life cycle, so by the **Single Responsibility Principle**, it shouldn’t be doing anything else. Doing so would lead to a app delegate that has too much code. If Apple adds more application life cycle events then its alright for the app delegate to change. _(I think that even with that single responsibility the app delegate is doing too much. One would be wise to delegate its handling of life cycle events to other classes, such as a class just to manage push notifications, as one example.)_ But if the app delegate is also managing the Core Data stack and has to change if you decide to use something else, or rename your database file, or something else related to data storage, then you’re violating the **Single Responsibility Principle**. **The Single Responsibility Principle** really boils down to a keeping a class cohesive, easy to change, easy to use, easy to understand, and easy to test.