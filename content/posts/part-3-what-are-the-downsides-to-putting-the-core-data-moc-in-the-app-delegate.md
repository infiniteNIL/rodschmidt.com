---
title: 'Part 3: What Are The Downsides to Putting the Core Data MOC in the App Delegate'
date: Tue, 25 Oct 2016 21:28:08 +0000
draft: false
tags: ['Apple Platforms', 'Software Development']
---

In [part 2]({{<ref "what-are-the-downsides-to-putting-the-core-data-moc-in-the-app-delegate-part-2.md#more-243">}}), I talked about why putting the MOC in the app delegate is a violation of the Single Responsibility Principle. 

In [part 1]({{<ref "what-are-the-downsides-to-putting-the-core-data-moc-in-the-app-delegate.md">}}), I talked about why putting the MOC in your app delegate makes you dependent on Core Data for your application’s persistence. 

Today I like to talk about the 3rd reason I gave in part 1, which is:

* Any code you write that uses `myManagedObjectContext` will be dependent on the App Delegate.

Any code you write in another source file, say a subclass of `UITableViewController` for example, that uses `myManagedObjectContext` from the app delegate will be dependent on the app delegate. In Objective-C this would mean you would need a `#import "AppDelegate.h` statement in your subclass’ source file. Every source file that accesses the app delegate’s MOC would need that import, which would bring in everything else the app delegate uses. In Swift, you don’t need the import statement, but it would increase your compile times, as it would in Objective-C as well. Every time you made a change to the app delegate, any source files that accessed the MOC in the app delegate would have to be recompiled. 

Big deal you say, so the compiler has to work harder. It’s true this is a minimal concern, but when working on a team, and especially with Swift, it can become a problem when you have a lot of dependencies scattered throughout the system. The bigger problem is the actual dependencies. When a module (a source file for our purposes) has a lot of dependencies on other modules in the system, it makes that module harder to change and more susceptible to bugs. We should strive to keep dependencies to a minimum. There is really no good reason for any module to be dependent on the app delegate when all it needs to do is persist something. 

The MOC should be somewhere else, such as the `DataStore` presented in part 1. Then our subclass example above is just dependent on DataStore, a much more sensible and understandable dependency.